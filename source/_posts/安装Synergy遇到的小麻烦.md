---
layout: post
title: 安装Synergy遇到的小麻烦
comments: true
date: 2015-07-14 19:39:19
categories: 工具使用
tags: [心得]
---
在实验室的开发机，一边是Ubuntu系统，一边是Mac系统，开发起来需要两套键盘和鼠标，说实话，虽然不太方便，但是还是可以接受的。
今天同实验室的同学推荐了一款叫**Synergy**的软件，可以连接屏幕，只用一套键鼠就可以操作两台电脑，心里还是有些小兴奋的，于是赶紧安装玩耍，感觉不错，所以分享一下。惯例贴图上正文：
![apple_ubuntu_windows](/images/5.1_apple_ubuntu_windows.jpg)
<!--more-->
可以访问官网[http://synergy-project.org](http://synergy-project.org)下载，不过需要注册，同学推荐的网址是[http://synergy-project.org/nightly](http://synergy-project.org/nightly)，可以很容易的找到最新版本，现在的时候一定要注意**版本号要一致**。
在各自平台上安装好之后，现在要确定服务端和客户端，选定一台，结果悲剧的事情发生了，报出：failed to connect to server的错误，查找之后一种说法是将屏幕名改为不含'-'或者是大写字母的，更改之后发现，启动之后报出代号为１的错误信息，最终方法来源于博客《[花了近4个小时装synergy](http://blog.sina.com.cn/s/blog_5ae7a1de0100sbcp.html)》。
选择使用已有的配置，创建**/etc/synergy.conf**，然后按照如下方法配置：
>section: screens
linux-name:
mac-name:
end
section: links
mac-name:
right = linux-name
linux-name:
left = mac-name
end
  
连接完成，喜大普奔～
