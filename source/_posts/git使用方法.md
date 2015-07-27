---
layout: post
title: git基本使用方法
comments: true
date: 2015-07-22 23:19:57
categories: 通用技术
tags: [闲扯]
---
早在2011年的暑假就曾接触过git这个版本控制工具，当时的主流版本控制还是CVS,SVN之类的，那时候跟着俄亥俄州立大学的一个软件工程老师学习开发iOS程序，他搭建了git server，用的稀里糊涂的，因为那时候对Linux是一窍不通，命令什么的都还只是停留在电影画面里，后来github火起来了，用git的人也就自然而然的多了起来，2013年刚刚开始研究僧的生活，那个时候创建了github账号，可空有程序员的架势，却怀着一颗文艺的心，直到近些日子再次捡起，才倍感酸爽，早些时候咨询同实验室的一位大神朋友[ictxiangxin](https://github.com/ictxiangxin)如何用正确的姿势使用git，大神的回答很简单，说看过git的官方文档，不可否认，官方的才是第一手资料，能有最真切的体会，不过确实是太多，就放弃了，年初时师兄推荐的一个简单的介绍git的网页颇为好用，原链接地址[git使用简易指南](http://www.bootcss.com/p/git-guide/)。这里简单写写git的使用方法，算是回顾一下。
<!--more-->
最初使用git是去下载github上的一些开源的OC代码，使用
```
git clone username@host:/path/to/repository
```
这样的命令，但后来自己创建的项目如何push给remote repository就成了问题，才知道原来git和项目本来就是分开的，如果需要将其绑定在一块，需要一个重要的命令
```
git init
```
直接在工程目录中git init，这样的话算是把git和项目进行了绑定，而在oh-my-zsh当中，直接用特别可爱的绿色将已经绑定的Path写了出来，而且还指名了具体的分支。
理解了git基本的分布式原理之后，才明白，原来这无非是本地和远程的副本而已，所有的修改操作，直接使用命令
```
git add .
```
将修改推入缓存区域，临时保存改动，然后使用命令
```
git commit -m 'lalala'
```
来提交修改，但是这样的修改只是局限在本地仓库，如果要将改动提交到远端仓库就要使用命令
```
git push origin master
```
这里可以把master换成任何想要推送的分支，比如master:branch，这个时候发现，自己还没有创建远程仓库，这个就是github的由来了，这里直接在github上创建自己账户下的repository，然后使用命令
```
git remote add origin <server>
```
将本地的改动推送到所添加的服务器上去。这里光自己开发不够，如何让别人也加入自己的项目，或者自己加入别人的项目，这里就要用到**分支**这个概念。master是默认分支。简单介绍一下分支的操作。
创建一个叫做“feature_x”的分支，并切换过去：
```
git checkout -b feature_x
```
切换回主分支：
```
git checkout master
```
再把新建的分支删掉：
```
git branch -d feature_x
```
除非你将分支推送到远端仓库，不然该分支就是 不为他人所见的：
```
git push origin <branch>
```
更新本地仓库至最新改动，执行：
```
git pull
```
pull等价与在自己的工作目录下面fetch并merge远端的改动，要合并其他的分支到当前的分支，执行：
```
git merge <branch>
```
如果此处碰到conflicts，需要手动修改冲突文件，然后手动
```
git add <filename>
```
将他们标记为合并成功。在合并改动之前，可以使用命令
```
git diff <source_branch> <target_branch>
```
来查看两个文件。如果要进行版本回退，就是代码没写好，想重写，又不想慢慢删，使用命令
```
git checkout -- <filename>
```
就能很方便的把HEAD中的最近一次提交的filename文件替换当前的文件，其他的添加到缓存区的以及新的文件都不会受到影响。
下面的命令将会重置本地目录。
```
git fetch origin
git reset --hard origin/master
```
如果这些你觉得开枯燥的话，前些日子我在知乎上看到的一哥的回答，目前得到1539个赞，一定会提起你的兴趣。
[原文链接](http://www.zhihu.com/question/20070065/answer/30521531)
**此处应有分割线**
<hr>
Github的基本功能：
- Repository：你和我一起做“知乎首页”，“知乎首页”就是Repository，即项目或者”未来武器T2级425mm磁轨炮“之类，怎么叫随你，你只需知道Repository是个放项目的地方就行。有时候会出现Repositories，是多个Repository的意思。
- Fork：我们把制作“知乎首页“的工作分开，你负责美工，我负责前端开发，但我们还需要数据服务器高手。你找来了一位php大牛，这位大牛很快搞定了服务器端，闲来无事，就看了看我的前端代码，一看，“我靠，这怎么一点也不语义化呢？全是尼玛的清一色的<div>啊，将来做交互js还搞不搞dom了……”于是这大牛在Repository中找到了我写的“zhi.html”，Fork了一份，也就是授权拷贝。
- Branch：Fork之后，在大牛的Github上出现了一个同样叫做“知乎首页”的Repository，但是这个Repository是复制品，只归他，这就是他的Branch，也就是分支。
- Pull Request：大牛做完了一份全新的高端zhi.html，点了Pull Request，也就是推送请求。我接受了，看了一眼，顿时惊讶爆表，“中国足球——高，实在是高！”
- 现在你懂了，Github的结构是Repository-Branch-(获取/推送)文件。你又发现Github可以比较两个文件的异同，新增的部分用绿色标记，删除的部分用红色标记。Pull Request还可以控制，甚至可以合并Branch，这就是团队合作利器啊，真乃高大上也，手痒了吧？心动了吧？

1. 请注册Github并登录。
2. 下载客户端并登录，客户端负责你硬盘上的数据与Github服务器数据的交互，然后设置存储目录。为了表现你的才华，你决定将此目录命名为“诸神之爹”。
3. 既然有这么多的国外开源项目，我们国内哪有不自主的道理。必须要实践一下这个顶好赞的Fork功能。现在你来到了Fadeoc/frontend · GitHub，你看到了这是用户Fadeoc的一个叫做“frontend”的Repository，你笑了，这家伙学习前端知识不过十天，代码一片渣，竟然有的代码里只写了“土豆”和“二狗子”几个汉字。你点了一下右上角的Fork，然后clone in desktop，保存到“诸神之爹”，哇！文件已经在你电脑里了，完全免费耶！+10086！
4. 一个小时后，你对Fadeoc的渣代码颇有心得，决定帮他改良，不然他这项目就完了。你改好之后，Pull Request，这丫的竟然说你的代码太渣，不吸收。贱人！老子自己做，抢你市场份额！
5.你点了右上角自己头像后面的+号，选择了第一个New repository，即新建repository，并且起了个名字，叫做“完爆Fadeoc”，然后点击绿色按钮set up in desktop，弹出保存框，选择“诸神之爹”。于是“诸神之爹”下出现了一个“完爆Fadeoc”的文件夹。
6. 你自己写了一份“神爹首页.html”，把它放在了“完爆Fadeoc”文件夹下。
7. 你打开了客户端，看到客户端界面中master Branch（主人分支，这名字太云端了）出现了一个Uncommitted changes，即未提交的变动，也就是你刚写的“神爹首页.html”。你点开show按钮，在summary（摘要）的部分添上“滚你丫的Fadeoc”，在Description（细节描述）的位置是没必要写的，但你还是决定添上“爆你菊花”四个大字。然后选择“Commit to 你的用户名”。
8. 为了把这个提交上传到Github上让贱人Fadeoc看到，你点击了客户端右上角的后面显示了一个“+1”的Sync，即同步，过了几秒，Sync前的两个曲线箭头停止了转动，同步成功了，“+1”消失，表示一个文件成功上传。
9. 你来到Github，刷新自己的个人页，“完爆Fadeoc”这个Repository出现在页面上，点开它，在里面你看到了”神爹首页.html”。
10. 为了让这个项目的初始目的更加浅显易懂，你决定添加一个Readme.txt，虽然从前下载的N多软件的文件夹里总是有一个Readme.txt，你一个都没打开过。但在圈里混，就得混的人模狗样的，于是你在“完爆Fadeoc”下新建了一个Readme.txt，里面写上，“Fadeoc，没错，说的就是你，看我口型，你个贱人！”
11. 同样使用客户端commit，然后sync，过了几秒，刷新github，你看到又多出了一个readme.txt。而且在下面又多出一个文字显示框，里面显示的就是readme.txt里面的内容“Fadeoc，没错，说的就是你，看我口型，你个贱人！”，避免了Fadeoc这个贱人不想打开readme.txt也就看不到你亲切问候的尴尬局面。Github真是贴心呐。
12. 你复制了这个Repository的地址，Email给了Fadeoc。
13. Fadeoc不是那么容易被打败的，于是他Fork了你的Repository，修改了readme.txt，然后pull request，你看到fadeoc新生成的branch下的readme.txt被改成了“你才是贱人”。你拒绝了合并请求。
14. Fadeoc再次pull request，readme.txt改成了“敢不做恶吗？”
15. 你有点烦了，这他妈的怎么才能不让他pull request，将来大项目N多陌生人菜鸟pull request烦不烦，就不能不开源，转私有吗？你终于找到了Github的升级服务，你笑了，将这个Repository从Public转成了Private。Fadeoc肯定会继续pull request，得不到你回应的他只会渐渐被复仇的怒火烧尽理智，可是，谁在乎呢？

Github还有更多细节功能，在使用过程中，你会慢慢发现，慢慢学会。但是不管如何，现在你会使用Github的基本功能了。 