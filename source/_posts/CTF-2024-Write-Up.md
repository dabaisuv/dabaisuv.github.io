---
title: CTF 2024 Presented by Cyandev Write Up
date: 2024-01-05 00:17:30
tags:
---
**作者：dabaisuv，原文链接：https://dabaisuv.github.io/2024/1/05/CTF-2024-Write-Up**

CTF 2024 Presented by Cyandev https://ctf.cyandev.app 的通关笔记

### 1.Check in
看情况是需要我输入一个flag

![](/images/CTF-2024-Write-Up/1.png "1")

随意输入字母a并单击提交，出现了如下提示。

![](/images/CTF-2024-Write-Up/2.png "2")

这是`javascript`的`alert`提示，这里我们可以通过hook` alert`并栈回溯找到调用源，但是我们先试试直接寻找`submit`的回调函数。如图，找到`click`回调。

![](/images/CTF-2024-Write-Up/3.png "3")

，由此再跳转到源码处，断点后发现此处函数为空。

![](/images/CTF-2024-Write-Up/4.png "4")

进一步跟下去，各种跳转，应该是一种反跟踪措施。正着走行不通就倒着hook`alert`试试，
在控制台执行以下命令：

```javascript
// 保存原始的 window.alert 函数
var originalAlert = window.alert;

// 创建新的函数来替代 window.alert
window.alert = function(message) {
    // 添加你的逻辑
    console.log('Alert Hooked!');
    // 触发中断
    debugger;
    // 调用原始的 window.alert 函数
    originalAlert(message);
};

// 测试钩取后的效果
window.alert('Hello, World!');
```
上面的命令会hook住`alert`函数，并在被调用时触发调试器中断。
回到提交flag处再次点击`submit`，此时`alert`被调用并触发中断。

![](/images/CTF-2024-Write-Up/5.png "5")

栈回溯找到`if`判断，将条件直接改为改为`true`并再次运行。

![](/images/CTF-2024-Write-Up/6.png "6")
![](/images/CTF-2024-Write-Up/7.png "7")

成功！
![](/images/CTF-2024-Write-Up/8.png "8")

直接成功过了，其实还有一种方法可以获取flag1的真实值，就是直接一路跟到它的`checkFlag1`函数并修改暴力破解的。

![](/images/CTF-2024-Write-Up/9.png "9")

Challenge 2也可直接修改if的判断值绕过。

![](/images/CTF-2024-Write-Up/10.png "10")

作者还给了一个通关奖励，需要flag1的真实值。

![](/images/CTF-2024-Write-Up/11.png "11")

可以利用我上面的暴力破解法获取真实值，打码了。

![](/images/CTF-2024-Write-Up/12.png "12")

最终是一个支付宝口令红包，不过好像被领完了。

![](/images/CTF-2024-Write-Up/13.png "13")
