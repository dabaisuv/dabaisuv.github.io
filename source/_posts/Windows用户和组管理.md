---
title: Windows用户和组管理
date: 2023-11-14 20:47:42
tags:
---
##  一：用户概述
SID也就是安全标识符（Security Identifiers），是标识用户，组和计算机账户的唯一号码。如果创建账户，再删除账户，然后使用相同的用户名创建另一个账户，则新账户将不具有授权给前一个账户的权力或权限，原因是该账户具有不同的SID号。普通用户的UID是1000开始。
  查看当前用户的SID：`whoami /user`
  查看所有用户的SID：`wmic useraccount get name,sid`
  windows server系统上，默认密码的最长有效期42天
  账户密码储存位置为`C：\windows\system32\config\SAM` 该文件再系统运行时锁定的，无法对文件进行操作，且只有system账户可读写
## 二、常见内置账户
**给用户使用的账户：**
`administrator` 管理员账户
`guest` 来宾账户

**计算机服务组件相关的系统账号：**
`system` 系统账户，权限至高无上，真正意义上的管理账户
`local services` 本地服务账户，权限等于普通用户，主要负责一些网络相关的服务，如DNS客户端服务
`network services` 网络服务账户，权限比普通用户更小，主要负责系统中的一些本地服务
三、用户管理命令
`net user` #查看用户列表
`net user <用户名> <密码>` #改密码
`net user <用户名> <密码> /add` #创建一个新用户
`net user <用户名> /del` #删除一个用户
`net user <用户名> /active:yes` #激活账户
`net user <用户名> /active:no` #禁用账户
## 三、组概述
组的作用：简化权限的赋予

组和用户的关系：一个组可以有多个用户、一个用户可以属于多个组

常用的内置组：内置组的权限默认已经被系统赋予
`administrators` #管理员组
`guest` #来宾组
`users` #普通用户组，默认新建用户都属于该组
`network` #网络配置组
`print` #打印机组
`remote desktop` #远程桌面组
## 四、组管理命令
`net localgroup` #查看组列表
`net localgroup <组名>` #查看该组成员
`net localgroup <组名> /add` #创建一个新的组
`net localgroup <组名> <用户名> /add` #添加用户到组
`net localgroup <组名> <用户名> /del` #从组中踢出用户
`net localgroup <组名> /del` #删除组

