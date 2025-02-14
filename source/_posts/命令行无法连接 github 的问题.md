---
layout: post
title: '命令行无法连接 github 的问题'
comments: true
date: 2025-02-14 10:22:51
categories: 工具
tags: [笔记]
---

连接了 lantern 翻墙软件之后，网页端可以访问外网，但是命令行无法访问 GitHub，解决办法如下

<!--more-->

> 查看当前代理设置
> 
git config --global --get http.proxy
git config --global --get https.proxy

> 根据端口号重新设置 proxy
>
git config --global http.proxy http://127.0.0.1.50364
git config --global https.proxy http://127.0.0.1.50364

> 导出 proxy
> 
export http_proxy=http://127.0.0.1:50364
export https_proxy=http://127.0.0.1:50364
