---
title: Windows下利用docker打包linux的dart可执行文件
date: 2023-12-18 00:16:16
tags:
---
### 
**作者：dabaisuv，原文链接：https://dabaisuv.github.io/2023/12/18/Windows下利用docker打包linux的dart可执行文件**

> 因为dart现在还不支持交叉编译（2023.12.18），出于特殊情况需要在windows下快速打包Linux下的可执行文件。

#### 先行需要：
安装好docker：https://docs.docker.com/desktop/install/windows-install/

可能会遇到个错误是：*Docker for Windows error: "Hardware assisted virtualization and data execution protection must be enabled in the BIOS"*
解决链接：https://stackoverflow.com/questions/39684974/docker-for-windows-error-hardware-assisted-virtualization-and-data-execution-p
#### 执行步骤：
在项目根目录（`pubspec.yaml`所在路径）执行下面语句，则会默认compile `bin/main.dart`，生成`bin/main`，也可以通过`-e i=bin/xxx.dart -e o=bin/xxx`直接指定输入与输出（好像多此一举）
```bash
docker run -v .:/app --rm dart /bin/bash -c '
    mkdir /app;
    cd /app;
    if [ -z "$i" ]; then i="bin/main.dart"; fi;
    if [ -z "$o" ]; then o="bin/main"; fi;
    dart pub get;
    dart compile exe "$i" -o "$o";'
```

`