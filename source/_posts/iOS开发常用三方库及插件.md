---
layout: post
title: iOS开发常用三方库及Xcode插件集锦(转载)
comments: true
date: 2015-07-22 09:18:51
categories: 转载
tags: [第三方库]
---
2015.7.27 补充：
[iOS开源库汇总](http://github.ibireme.com/github/list/ios/)
[iOS GitHub Top 100 简介](https://github.com/Aufree/trip-to-iOS/blob/master/Top-100.md)
其实在看**唐巧**写的《iOS开发进阶》这本书的时候，就像把iOS常用到的第三方库和Xcode插件总结一下，写一篇文章，恰巧在*iOS开发*微信公众号上看到了这篇文章，原文如下。
![apple_logo](/images/9.1_apple_logo.jpg)
<!--more-->
#前言
第三方库是现在的程序员离不开的东西 不光是APP开发 基本上所有的商业项目 都会或多或少的使用到第三方库
Github上Star>100的开源库数量如下

可以看到JS以绝对的优势排名第一 一定程度上也说明了JS在这几年为什么发展得这么迅速 不会点JS都不好意思说自己是码农 不过现在JS圈的造轮子浪潮也是愈演愈烈了 自己不写个框架都不好意思打招呼

OC排名第四 相信这个排名还会上升 Swift暂时还挤不进前十 不过超过OC 也是指日可待(毕竟亲儿子)

Xcode基本是Mac/iOS程序员的必备工具(AppCode我没用过 不知道有多少人用?) 但是能说有多好用..这就仁者见仁了(跟Visual Studio是没得比了) 不过在用了插件以后 倒也能让生产力提升一大截

接下来我会介绍一些我本人常用的第三方库和插件 也许不多 但是一定是久经考验 用了一定不会错

#介绍
##第三方库
###CocoaPod
CocoaPod并不是iOS上的第三方库 而是大名鼎鼎的第三方库的管理工具
在CocoaPod没有出现之前 第三方库的管理是非常痛苦的 尤其是一些大型的库(比如nimbus) 每次对库进行更新 都可能会非常的痛苦
CocoaPod的出现解决了这些问题 以Framework的方式引入第三方库 极大的节约了集成的时间 而且通吃Objective-C和Swift(Swift上的Cathatage我没有实际用过 但是它的那种集成方式还是比CocoaPod麻烦点)
###iCarousel
在iOS 4.x的时代(我也是从4.0开始接触iOS开发的) Coverflow的效果可谓是风靡一时 初出茅庐的我当年对如何实现几乎是束手无策(当年所有的电影资讯类的APP 如布丁爱生活等 都需要实现Coverflow选片的效果 碰巧我也要做一个) iCarousel的出现替我解决了一个大的难题
从此iCarousel成为了我每个项目的必备良药(也是我最喜欢的库) 不管是Coverflow还是轮播广告 都能轻描淡写的搞定 其内置的十来种显示类型 基本可以解决90%的UI需求 而其强大的自定义功能 则可以解决剩余的10% :)
不熟悉的朋友可以尝试一下其精美的demo(pod try iCarousel) 所有的功能都在demo中一览无余
作者nicklockwood也是一个高产的大神 同时维护着数量众多且优质的开源库(比如iVersion iRate) 大家不妨去看一看 淘一淘
###AFNetworking/Alamofire
HTTP框架的龙头老大 当年在与ASIHTTPRequest的竞争中笑到了最后(当然也是因为ASIHTTPRequest的作者不维护了 不过国内很多公司 因为历史原因都在自行维护) 由于及时维护和拥抱语言的新特性 迅速被大家所接受和喜爱
相信每个人都用过 这里就不赘述了
###MKNetworkKit
如果说AFNetworking是老大 那么MKNetworkKit可以说是小弟了 不过也因为比AFNetworking轻量的原因 也获得了许多开发者的青睐
当年因为ASIHTTPRequest停止维护了 在机缘巧合之下 我在AFNetworking和MKNetworkKit之间选择了后者 并在很多项目中进行了使用
不过也许是因为AFNetworking发展得更快更迅速 而作者本人是单兵作战的原因 作者也基本放弃了更新(版本号在停留在0.8x) 十分可惜
###SDWebImage
图片异步下载和缓存管理的集大成者 UITableView的黄金搭档 其代码被开发者研究分享过无数次喵大的Kingfisher(可以说是Swift版的SDWebImage)也是深受其影响
异步下载AFNetworking和MKNetworkKit都有实现 数据缓存也有FastImageCache与TMCache等 但是将其融会贯通的 唯有SDWebImage了
除了简单的使用sd_setImageWithURL之外 SDImageCache也是可以独立使用的 功能也相当之强大
###ZXingObjC
zxing是Google出品的二维码扫描组件 原本是Java编写的 现在也有了各种语言的移植版 而ZXingObjC就是其中之一
zxing支持各种主流的一维码二维码扫描 简单易用 小厂如果要实现二维码扫描这个功能 基本都会选择zxing
不过iOS7已内置了二维码摄像头扫描 而iOS8也已内置了二维码静态图扫描 以后可能再也不需要用到ZXing了 :(
###MBProgressHUD
简单易用且稳定的HUD组件 半透明黑底白字的风格也经久不衰 loading提示的最佳选择
###Masonry/SnapKit
最好用的Autolayout手写库(Cartography也不错啦 但是还是用不惯) 帮助我轻易的跨越了Autolayout这道坎
我也曾多次在文章中提到过关于Masonry的使用方法 如果还没有用过的朋友 不妨看一看
###pop
Facebook的工程师一直是神一般的存在 对开源社区的贡献也是巨大的 极大的推动了各种变成语言的发展 比如HipHop之于PHP react之于JavaScript pop之于Objective-C等等
不管是HipHop react Facebook的工程师总是抱着颠覆的态度来开源 pop也不例外 这点之前我也简单介绍过 而以pop为基础的paper一发布就震惊整个APP届 在这点上pop也是厥功甚伟
pop对自定义动画也支持得很好 我也以pop的自定义动画为基础写过MMTweanAnimation
###ReactiveCocoa
说起来惭愧 大名鼎鼎的RAC 我只使用了点皮毛(只拿来做输入验证了)
暂时还没有进行深层次的使用 对RAC的理解也停留在表面阶段 不过这篇文章介绍得非常详细 值得一看
###GPUImage
如果你要做图像(照片或者视频)的相关处理 或者只是简单的想做个像Camera360一样的拍照滤镜 那么你一定要研究一下GPUImage
如它的名字所述 GPUImage是基于GPU的图像处理框架 我们都知道 GPU是提升性能的关键 这也就是GPUImage如此重要 如此受欢迎的原因
###Lumberjack
log系统是每个项目都应该有的东西 而Lumberjack是log系统中的翘楚
你可以简单的把它当成NSLog的替代品(简单来说 Lumberjack比NSLog速度更快) 或者根据你的需要来打造一个更强大的日志系统
###NSLogger
从名字可以看出 NSLogger也是一个log系统 其特点是附带了一个功能强大的Desktop Viewer 可以让你方便的查看APP产生的日志(支持分级筛选等等 甚至可以直接log一张图片)
###AwesomeMenu
当年横空出世的Path 其优美的设计 精彩的动画 不知让多少人嘴巴都合不拢 而最赞的 就是它的弹出菜单 一时成为了每个APP争相模仿的对象
有了AwesomeMenu 你可以轻易的实现它
###MMDrawerController
普通的侧滑菜单 用MMDrawerController就搞定了
###realm
作为数据存储的一等公民 CoreData的地位不言而喻 不过也因为使用起来不够方便 才会出现MagicalRecord这种辅助类 甚至fmdb这种基于纯sqlite的库
而realm以挑战者的身份闪亮登场 不仅读取性能更快(据说数倍于CoraData) 接口简单易用(以对象的形式使用数据 这点和leancloud的思路很相似) 并且还跨平台(iOS/Android均可使用 OC/Swift/Java都支持)
如果你习惯使用Mantle之类的Modal转换的话Realm-JSON肯定能讨你欢心
作为YCombinator的孵化项目 其质量还是能得以保证的 至少我试用下来 确实给我以很大的惊喜 不管是API的设计 还是数据对象的定义 就连数据库的版本升级 都非常的方便)
##Xcode插件
###Alcatraz
与CocoaPod类似 Alcatraz是Xcode的插件管理器 能够让你方便的管理Xcode的插件(不仅可以管理插件 还可以管理主题等等)
###FuzzyAutocomplete
如果只让我选一个插件留下 那必须是FuzzyAutocomplete 强大的模糊匹配输入 让你写代码的时候再也不用费脑子去记住名字那么长的对象或者函数名了 好用到让你想哭
###XAlign
作为有洁癖的码农 看到不对齐的代码一定是不能忍的 XAlign可以轻松解决你的烦恼
###VVDocumenter-Xcode
喵大的又一力作 能够识别当前函数的参数和返回类型 帮助你快速编写符合规范的注释(目前是以Javadoc为标准)
###deriveddata-exterminator
如果你老是遇到Xcode抽风 提示你要因为某个原因要删除某个目录下的文件 否则编译不过 那你一定会被这个插件感动 因为说明了遇到这个事的人不只你一个
###Xcode-Quick-Localization
多语言在iOS开发中一直不是很方便 有了它 你可以省不少事
###Backlight-for-XCode
就如果Xcode默认的80个字的分页提示一样 高亮当前正在编辑的一行 也是一种友好的提示 喜不喜欢也就因人而异了
#小结
以上的介绍 都是从我自身的使用经验触发的 可能大部分大家都用过了 而我也将会持续的更新这份列表 只要有优秀使用的东东 我都会添加进来

来源：里脊串的开发随笔


