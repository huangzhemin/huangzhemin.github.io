---
layout: post
title: '@synthesize和@dynamic'
comments: true
date: 2018-07-14 13:40:51
categories: iOS
tags: [开发]
---
发现自己对property中synthesize和dynamic的用法这么陌生，找到一篇挺不错的博客

原文链接：[https://www.jianshu.com/p/c883687c6405](https://www.jianshu.com/p/c883687c6405)

@property有两个对应的词

一个是 @synthesize，一个是 @dynamic。如果 @synthesize和 @dynamic都没写，那么默认的就是@syntheszie var = _var;

### @synthesize

@synthesize表示如果属性没有手动实现setter和getter方法，编译器会自动加上这两个方法。

### @dynamic

@dynamic 告诉编译器：属性的 setter 与 getter 方法由用户自己实现，不自动生成。假如一个属性被声明为 @dynamic var，而且你没有提供 @setter方法和 @getter 方法，编译的时候没问题，但是当程序运行到 instance.var = someVar，由于缺 setter 方法会导致程序崩溃；或者当运行到 someVar = var 时，由于缺 getter 方法同样会导致崩溃。编译时没问题，运行时才执行相应的方法，这就是所谓的动态绑定。

例如：

<!--more-->

```
#import <Foundation/Foundation.h>

@interface People : NSObject

@property (nonatomic, copy) NSString *name;

@end

#import "People.h"

@implementation People

@dynamic name;

@end
```

当执行到：

```
People *p= [People new];
p.name = @"Peter";
```

程序就会crash，原因是

“[People setName:]: unrecognized selector sent to instance”。

解决这种奔溃的方法有三种：

### 方法一：

最简单粗暴，但是还是挺管用，直接注释掉@dynamic name这行代码即可，由编译器自动添加。

> 但是如果我们不想让编译器自动添加，那么我们可以手动添加或则在运行时添加都可以。
	
### 方法二：

手动添加，由于@dynamic不能像@synthesize那样向实现文件(.m)提供实例变量，所以我们需要在类中显式提供实例变量。
自己编写setter getter方法

```
#import <Foundation/Foundation.h>

@interface People : NSObject

@property (nonatomic, copy) NSString *name;

@end

#import "People.h"

@interface People ()
{
    NSString *_name;
}
@implementation People

@dynamic name;

- (void)setName:(NSString *)name {

    _name = [name copy];
}

- (NSString *)name {

    return _name;
}

@end
```

### 方法三

通过runtime机制在运行时添加属性的存取方法。

> 在C函数中不能直接使用实例变量，需要将Objc对象self转成C中的结构体，因此在Person类同样需要显式声明实例变量而且访问级别是@public
	
```
#import <Foundation/Foundation.h>

@interface People : NSObject

@property (nonatomic, copy) NSString *name;

@end

#import "People.h"
#import <objc/objc-runtime.h>

@interface People ()
{
    @public
    NSString *_name;
}
@end

@implementation People

@dynamic name;

+ (BOOL)resolveInstanceMethod:(SEL)sel {


    if (sel == @selector(setName:)) {
        
        class_addMethod([self class], sel, (IMP)setName, "v@:@");
        return YES;
    }else if (sel == @selector(name)){
        
        class_addMethod([self class], sel, (IMP)getName, "@@:");
        return YES;
    }
    
    return [super resolveInstanceMethod:sel];
}

void setName(id self, SEL _cmd, NSString* name)
{
    
    if (((People*)self)->_name != name) {
        ((People *)self)->_name = [name copy];
    }
}

NSString* getName(id self, SEL _cmd)
{
    return ((People *)self)->_name;
}
@end
```