---
layout: wiki
title: gitHub
categories: gitHub
description: gitHub 相关使用技巧
keywords: gitHub
---

## 打开/下载文件加速

1. 项目内的文件

    比如要下载 https://github.com/junegunn/vim-plug/plug.vim，那可以将 github.com 替换成 cdn.jsdelivr.net/gh，最终地址 https://cdn.jsdelivr.net/gh/junegunn/vim-plug/plug.vim

    参考 <https://mazhuang.org/2020/05/01/cdn-for-github-pages/>

2. Releases 里的文件

    比如要下载 https://github.com/PowerShell/PowerShell/releases/download/v7.1.0-preview.6/PowerShell-7.1.0-preview.6-win-x64.zip，可以将 github.com 替换成 download.fastgit.org，最终地址 https://download.fastgit.org/PowerShell/PowerShell/releases/download/v7.1.0-preview.6/PowerShell-7.1.0-preview.6-win-x64.zip

    参考 <https://toolwa.com/github/>

3. 设置代理

   在当前项目中设置代理

   ```shell
   git config http.proxy "socks5://192.168.1.8:30000"
   git config https.proxy "socks5://192.168.1.8:30000"
   ```

   取消代理

   ```shell
   git config --unset http.proxy
   git config --unset https.proxy
   ```
   
