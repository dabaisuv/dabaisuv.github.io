---
title: Windows批处理
date: 2023-11-16 19:13:52
tags:
---
`@echo off`：关闭回显功能，也就是屏蔽过程，建议放置在批处理的首行
`rem`：添加注释
`pause`：暂停批处理运行
`title`：为批处理脚本设置标题
`echo`：在执行批处理脚本时，可以空一行
`set`：设置变量，常用与在脚本中的互动值，也可用于查看系统变量
`%<变量名>%`：取变量的值
`<标签名>:和goto`：用来定义标签名，goto实现跳转。
`start <可执行文件>`：如果参数为空则启动新的命令行，如果不为空，则启动对应的文件。

例子： 定时关机小程序
```bash
@echo off
echo ================================
echo.
echo                             定时关机小程序
echo.
echo ================================
title 定时关机小程序
set /p time=请输入您要设置的时间（单位/秒）：
shutdown -s -t %time% -f
pause
```


`errorlevel`：错误等级，0代表上一条命令执行成功

例子：if - else的使用
```bash
@echo off
set /p var1=请输入第一个变量
set /p var2=请输入第二个变量
if "%var1%==%var2%"(
echo 两个字符相同)
else(
echo 两个字符不同
)
```

例子: goto的使用(无限循环打开新命令行)
```bash
:test
start
goto test
```