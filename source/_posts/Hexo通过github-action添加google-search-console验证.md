---
title: Hexo通过github action添加google search console验证
date: 2023-11-22 00:17:05
tags:
---
**作者：dabaisuv，原文链接：https://dabaisuv.github.io/2023/11/22/Hexo通过github-action添加google-search-console验证**

所有权验证 -- 文件验证 -- 下载验证文件

该文件内容类似：`google-site-verification: googlexxxxxxxxx.html`
文件名类似：`googlexxxxxxxxx.html`

**目标：**在Hexo Github Action 生成的页面源码根目录，添加`googlexxxxxxxxx.html`文件，内容为`google-site-verification: googlexxxxxxxxx.html`

**简明步骤：**修改`.github/workflows/pages.yml`，在`npm run build`后加入两行:
```yml
 - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
        # 下面是添加的内容
      - name: add google verify
        run: echo "google-site-verification:googlexxxxxxxxx.html" > ./public/googlexxxxxxxxx.html
        # 上面是添加的内容
      - name: Upload Pages artifact
        uses: actions/upload-pages-artifact@v2
        with:
          path: ./public
```

**注意事项：**“验证文件”的内容中的空格得去掉，不然echo函数会出错。

收录的问题和其他验证方法可以参考：https://jactorsue.github.io/blog/2018/04/how-blog-on-githubpages-can-be-searched-by-google.html