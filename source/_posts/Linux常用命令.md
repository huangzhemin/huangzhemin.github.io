---
layout: post
title: Linux常用命令(持续更新中)
comments: true
date: 2015-07-19 16:19:15
categories: Linux
tags: [命令, 日常]
---
做自然语言处理实验，Linux系统肯定是不二的选择，这片文档主要记录一下自己用到过的命令，会持续更新下去。
<img style="display:block;width:60%", src="/images/7.1_Linux_versions.png"/>
<!--more-->
<hr>
####查看文件夹下的文件数
ls -l | grep "^-" | wc -l
####查看文件夹下的文件数，包括子文件夹里的
ls -lR | grep "^-" | wc -l
####查看文件夹大小
du -sh
####将文件夹列表放入文件中
ls > filelist
####统计文件
wc - lcw file
- c 统计字节数。
- l 统计行数。
- w 统计字数。