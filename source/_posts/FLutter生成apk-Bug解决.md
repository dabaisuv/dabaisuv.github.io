---
title: FLutter生成apk Bug解决
date: 2023-11-24 01:45:13
tags:
---
**作者：dabaisuv，原文链接：https://dabaisuv.github.io/2023/11/21/FLutter生成apk-Bug解决**

```shell
FAILURE: Build failed with an exception.

* Where:
Settings file '<flutter root directory>\android\settings.gradle' line: 26

* What went wrong:
Plugin [id: 'com.android.application', version: '7.3.0', apply: false] was not found in any of the following sources:

- Gradle Core Plugins (plugin is not in 'org.gradle' namespace)
- Included Builds (None of the included builds contain this plugin)
- Plugin Repositories (could not resolve plugin artifact 'com.android.application:com.android.application.gradle.plugin:7.3.0')       
  Searched in the following repositories:
    MavenRepo
    Gradle Central Plugin Repository
    Google

* Try:
> Run with --stacktrace option to get the stack trace.
> Run with --info or --debug option to get more log output.
> Run with --scan to get full insights.

* Get more help at https://help.gradle.org

BUILD FAILED in 10s
```
有多种错误原因，`cd`到`android`目录，运行`gradlew.bat --info`获取详细信息

解决方案一：
网络原因，修改`~\.gradle\gradle.properties`增加如下行：
```shell
systemProp.http.proxyHost=127.0.0.1
systemProp.http.proxyPort=<代理端口>
systemProp.https.proxyHost=127.0.0.1
systemProp.https.proxyPort=<代理端口>
```

解决方案二：
更换jdk版本为jdk11或以上，
官网jdk11下载地址：https://www.oracle.com/sg/java/technologies/javase/jdk11-archive-downloads.html
官网jdk21下载地址（不用登录）：https://www.oracle.com/sg/java/technologies/downloads/#jdk21-windows
安装好后注意修改JAVA_HOME环境变量指向安装位置，这步不会的就自行百度。