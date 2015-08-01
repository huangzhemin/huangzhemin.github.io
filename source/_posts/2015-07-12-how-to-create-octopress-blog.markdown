---
layout: post
title: "MakeDown写一篇关于octopress配置的文章"
date: 2015-07-09 22:53:10 +0800
comments: true
categories: 博客
tags: [工具, 捣拾]
---
一晚上的时间，终于用Octopress搭建起来了自己的博客，上传到自己的github上，希望这个博客能见证我这个菜鸟的进步。
先尝试用MakeDown来写一篇今天配置Octopress的整个流程吧。
<p>先发张拉风的图片</p>
![blog_pic](/images/2.1_Octopress_log.png)
<!--more-->
<p>再发一个参考博客的连接</p> http://sonnewilling.com/blog/2013/11/14/shi-yong-octopressda-jian-ge-ren-bo-ke/
  
文章中写的步骤很详细，只要一步步做基本是能完成。这里补充说明一下rvm的安装，因为我在这碰到一点小问题最终的解决步骤是这样的，方案来源于 http://blog.csdn.net/chszs/article/details/42462517

RVM是Ruby的版本管理器工具。
>1.安装RVM
 sudo apt-get curl
 curl -L https://get.rvm.io | bash -s stable
 source ~/.rvm/scripts/rvm
2.安装RVM的环境依赖
 rvm requirements
3.安装Ruby
 rvm install ruby
如果想在Ubuntu上安装多个Ruby版本，那么可以使用下面的命令来指定使用rvm作为默认的Ruby版本管理。
rvm use ruby --default
检查当前成功安装的Ruby版本
ruby -v

接下来是配置主题，最终选用名为"greyshade"的经典主题，安装步骤如下：
>git clone git@github.com:shashankmehta/greyshade.git .themes/greyshade
echo "\$greyshade: color;" >> sass/custom/_colors.scss //Substitue 'color' with your highlight color
rake install/['greyshade']
rake generate

当然如果**git clone git@github.com:shashankmehta/greyshade.git .themes/greyshade**报出限制访问的错误的话，就直接采用https的方式进行访问**https://github.com/shashankmehta/greyshade.git**

