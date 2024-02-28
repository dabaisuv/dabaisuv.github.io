---
title: golang官方漏洞检测工具 - govulncheck
date: 2024-02-19 23:42:14
tags:
---
**作者：dabaisuv，原文链接：https://dabaisuv.github.io/2024/02/19/golang官方漏洞检测工具-govulncheck**
> govulncheck是Go语言官方发布的漏洞管理工具，用于分析您的代码库和二进制文件，以发现您的依赖中已知的漏洞。这些工具是由Go安全团队维护的Go漏洞数据库支持的。Go的工具通过仅显示您的代码实际调用的函数中的漏洞，减少了结果中的噪音。

###### 架构图
![](2024/02/19/golang官方漏洞检测工具-govulncheck/architecture.png)

###### 安装方法
`go install golang.org/x/vuln/cmd/govulncheck@latest`
###### 使用方法
在module目录中使用命令`govulncheck ./...`

###### 参考
[1]https://go.googlesource.com/vuln
[2]https://pkg.go.dev/golang.org/x/vuln/cmd/govulncheck#section-sourcefiles

> 后续据说会并入go语言的二进制中。
