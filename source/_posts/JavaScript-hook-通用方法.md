---
title: JavaScript hook 通用方法
date: 2024-01-05 00:14:46
tags:
---
**作者：dabaisuv，原文链接：https://dabaisuv.github.io/2024/01/05/JavaScript-hook-通用方法**
例子，hook alert函数
```javascript
// 保存原始的 window.alert 函数
var originalAlert = window.alert;

// 创建新的函数来替代 window.alert
window.alert = function(message) {
    // 添加你的逻辑
    console.log('Alert Hooked!');
    debugger;
    // 调用原始的 window.alert 函数
    originalAlert(message);
};

// 测试钩取后的效果
window.alert('Hello, World!');
```