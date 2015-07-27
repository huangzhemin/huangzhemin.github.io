---
layout: post
title: 《Objective-C高级编程》笔记(日更)
comments: true
date: 2015-07-22 16:21:07
categories: iOS
tags: [高级编程]
---
《Objective-C高级编程——iOS与OS X多线程和内存管理》这本书，是两位叫*Kazuki Sakamoto*和*Tomohiko Furumoto*的日本人写的，之前是有问师兄借过，那个时候只是走马观花似的看了第三章关于GCD的介绍，觉得很一般，不知为何大家推荐这本书，时隔近一年，再次翻开，细细研读，才发现干货确实不少，下面就把阅读此书的笔记分享给大家。
<!--more-->
全书分三个大章节，分别为：
1.自动引用计数
2.Blocks
3.Grand Central Dispatch
下面就按这个顺序记录读书笔记。
#自动引用计数
章节开始是用办公室照明的例子来说明内存管理引用计数这一概念的，简单易懂，但这是每一个初学iOS都应该掌握的，直接略过。
##内存管理的思考方式
- 自己生成的对象，自己持有。
- 非自己生成的对象，自己也能持有。
- 不再需要自己持有的对象时释放。
- 非自己持有的对象无法释放。
具体的例子程序这里就不详细写了，因为在任何一本书上都能看到，此书真正引起我的注意，是从下面这段内容开始的。
##alloc/retain/release/dealloc实现
OS X、iOS中的大部分作为开源软件公开在Apple Open Source上，但是遗憾的是，包含NSObject类的Foundation框架并没有公开。不过Foundation框架使用的Core Foundation框架的源代码，以及通过调用NSObject类进行内存部分的源代码是公开的。但是，没有NSObject类的源代码，就很难了解NSObject类的内部实现细节。所以作者使用了开源软件GNUStep来说明。
对于一下代码。
```
id obj = [NSObject alloc];
```
调用NSObject类的alloc类方法在NSObject.m源代码中的实现如下：
```
+ (id)allc {
    return [self allocWithZone: NSDefaultMallocZone()];
}

+ (id)allocWithZone:(NSZone *)z {
    return NSAllocateObject(self, 0, z);
}
```

通过allocWithZone：类方法调用NSAllocateObject函数分配了对象。下面我们来看看NSAllocateObject函数。
```
struct obj_layout {
    NSUInteger retained;
};

inline id NSAllocateObject(Class aClass, NSUInteger extraBytes, NSZone *zone) {
    int size = 计算容纳对象所需内存大小;
    id new = NSZoneMalloc(zone, size);
    memset(new, 0, size);
    new = (id) & ((struct obj_layout *)new) [1];
}
```

NSAllocateObject函数通过调用NSZoneMalloc函数来分配存放对象的内存空间，之后将该内存空间置0，最后返回作为对象而使用的指针。
此处特别提一下NSZone这个类，简单理解就是将小容量和大容量分开存储，使得内存高效利用而不会产生碎片化的这么一个概念。
但是现在运行时系统忽略了区域这一概念，对于内存的管理本身已经机具效率，这种使用区域来管理内存反而会引起内存使用效率低下以及源代码复杂化等问题。
