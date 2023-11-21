---
title: Flutter Web 字体白框和白屏解决方案
date: 2023-11-21 16:11:04
tags:
---

### 1. 字体白框解决方案
原因是使用canvaskit渲染方式时，代码必须要fonts字体文件支持，而flutter默认会从https://fonts.google.com/ 获取对应语言的fonts，因为某网络原因，该网站访问被限制，导致一直加载不出字体。有两种解决方案。
#### 1.1. 解决方案一
将fonts文件本地化，参考官网解决方案：https://docs.flutter.dev/cookbook/design/fonts 。
#### 1.2. 解决方案二
强制使用html渲染方式，关于两种渲染方式的区别，参考官网：https://docs.flutter.dev/platform-integration/web/renderers
解决方法参考：https://docs.flutter.dev/platform-integration/web/initialization
简明操作：修改<program_root>/web/index.html文件，相关代码区段修改为如下，也就是给`initializeEngine()`函数添加一个`renderer`参数为字符串`'html'`：
```javascript
 window.addEventListener('load', function (ev) {
      // Download main.dart.js
      _flutter.loader.loadEntrypoint({
        serviceWorker: {
          serviceWorkerVersion: serviceWorkerVersion,
        },
        onEntrypointLoaded: function (engineInitializer) {
          engineInitializer.initializeEngine({
            renderer:'html'
          }).then(function (appRunner) {
            appRunner.runApp();
          });
        }
      });
    });
```
需要注意的是，使用html渲染方式的话，有些地方可能跟其他平台表现不同。例如：`Image()`组件的
`opacity`属性无效，需要使用`Opacity()`组件包裹来代替（截止flutter3.16.0版本依旧存在，github issue：https://github.com/flutter/flutter/issues/104114 ）。


### 2. 白屏解决方案
原因是flutter正在下载所需文件，如果渲染引擎是canvaskit，那么它默认会从https://unpkg.com/browse/canvaskit-wasm 网站下载，这个文件有6MB左右，因为某些网络原因，下载速度会变成几十KB甚至几KB每秒，偶尔还会断掉，所以flutter就一直白屏。当然白屏还有可能是因为你自己的服务器太慢了，下载网页图片等文件也很慢。还有一种原因就是你的代码有误，这个不在这里讨论，可以根据自己实际情况调试。这里解决第一种原因，也是最常见的一种原因。
#### 2.1. 解决方案一
还是强制使用canvaskit渲染，但是将canvaskit-wasm放在自己的服务器，也就是把它本地化，参考官网解决方案：https://docs.flutter.dev/platform-integration/web/initialization 。
简明操作：修改<program_root>/web/index.html文件，相关代码区段修改为如下，也就是给`initializeEngine()`函数添加一个`canvasKitBaseUrl`参数为字符串`'./canvaskit/'`：
```javascript
 window.addEventListener('load', function (ev) {
      // Download main.dart.js
      _flutter.loader.loadEntrypoint({
        serviceWorker: {
          serviceWorkerVersion: serviceWorkerVersion,
        },
        onEntrypointLoaded: function (engineInitializer) {
          engineInitializer.initializeEngine({
            renderer:'canvaskit',
            canvasKitBaseUrl:'./canvaskit/',
          }).then(function (appRunner) {
            appRunner.runApp();
          });
        }
      });
    });
```
这里解释以下，之所以设置成`'./canvaskit/'`，是因为`flutter build web`的时候，会默认在生成的`build/web`文件夹下生成`canvaskit`文件夹，里面就有canvaskit-wasm。开发debug模式运行的时候这样设置也有效果，虽然我没找到debug生成的目录。
#### 2.2. 解决方案二
不使用canvaskit渲染，改为使用html，这样就不用下载canvaskit了，也最省心......吧。
方法跟1.1一样。