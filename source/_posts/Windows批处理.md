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
`:<标签名>和goto <标签名>`：用来定义标签名，goto实现跳转。
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

例子：用户管理批处理
```bash
@echo off
title 账户管理工具
:menu
cls
echo =========================
echo.
echo         1.查看所有用户
echo         2.添加用户
echo         3.删除用户
echo         4.修改密码
echo         5.退出程序
echo.
echo =========================
set /p choice=请输入你的选项：
if %choice%==1 goto list
if %choice%==2 goto add
if %choice%==3 goto del
if %choice%==4 goto change
goto exit
:list
net user
pause
goto menu
:add
set /p username=请输入要添加的用户名：
set /p password=请输入要设置的密码：
net user %username% %password% /add > nul 2>nul
if %errorlevel%==0(
echo 账户添加成功
) else (
echo 用户添加失败，请检查下列问题
echo.
echo 添加的账户是否已经存在
echo 密码是否符合安全策略
echo 您是否具有管理员权限
echo.
)
pause
goto menu
:del
set /p usernamedel=请输入要删除的用户
net user %usernamedel% /del > nul 2>nul
if %errorlevel%==0 (
echo 账户删除成功
) else (
echo 账户删除失败，请检查。
)
pause
goto menu
:change
set /p name=请输入要修改密码的用户
set /p passwd=请输入要修改的密码
net user %name% %passwd% >nul 2>nul
if %errorlevel%==0(
echo 密码修改成功
) else (
echo 密码修改失败。
)
pause
goto menu

:exit
exit
```