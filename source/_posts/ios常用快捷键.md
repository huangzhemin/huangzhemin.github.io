title: ios常用快捷键
date: 2015-07-13 10:36:59
categories: iOS
tags: [快捷键]
---
为了写这篇博客也是历经坎坷，发现了Hexo这个比Octopress更加简洁方便的工具，而且主题样式不错，最重要的是搭建容易，谁知碰到了一系列没有预料到的问题，此处写一个小的知识点，算是给自己的一个提醒。
>zsh: command not found: 这个错误报告描述的是命令无法找到，我首先想到的是直接把Hexo的路径加入到了profile里面，开始是用户的，后来到系统的，可都没效果，细查以后才知道是支持库nvm没有找到对应的路径，使用source命令很容易的添加完成，切记切记～
  
此处按照惯例发一张图片
![Hexo](/images/4.1_Hexo.gif)
<!--more-->
刚刚又遇到了一个Hexo发布的问题，hexo ERROR Deployer not found，解决办法如下：
* 安装 npm install hexo-deployer-git --save
* 将deploy 的 type由github改为git
再在此处做一个标记，本来以为nvm问题解决了，可开启新的终端后，依然要执行一次source命令，才能开启nvm，回头要好好检查一下。
　　言归正传，这篇博客是要写ios常用快捷键的，这里做一个记录，方便自己，同时如果能为大家做点什么，是很开心的。
<hr>
1.Cmd + Shift + O
  快速查找类，快速定位指定类的源代码中。
2.Ctrl + 6
  列出当前文件中的所有方法，输入关键字过滤，快速定位想要编辑的方法。
3.Cmd + 1
  常用命令，左边导航栏切换到工程目录
4.Cmd + Ctrl + Up
  .h和.m之间切换
5.Cmd + Enter
  切换到standard editor，方便快速编辑
6.Cmd + Option + Enter
  切换到assistant editor
7.Cmd + Shift + Y
  切换Console view显示或隐藏
8.Cmd + 0
  隐藏左边的Navigator
9.Cmd + Opt + 0
  隐藏右边的Utility。
10.Cmd + Ctrl + Left/Right
  跳到上/下一次编辑的位置
11.Cmd + R
  运行代码
12.Cmd + B
  编译代码
13.Esc
  调出代码补全
14.Cmd + 单击方法
  查看方法的实现
15.Opt + 单击方法
  查看方法的文档
16.Cmd + t
  新建一个Tab栏
17.Cmd + Shift + [
  在Tab栏之间切换
<hr>
内容来源：**唐巧**的《ios开发进阶》中的快捷键章节。
  我选择了其中我平时会用到的一些，和未来将会用到的，写在这里。