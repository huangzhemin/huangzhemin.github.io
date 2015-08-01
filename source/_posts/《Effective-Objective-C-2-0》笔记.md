layout: post
comments: true
title: 《Effective Objective-C 2.0》笔记（更新中）
date: 2015-07-28 09:05:39
categories: iOS
tags: [进阶, Matt Galloway]
---
尽管这本书的作者Matt Galloway完成这本书只用了6个月时间，但是酝酿过程长达数年。这本书不是讲基本的语法，而是发挥Objective-C语言的优势，编写出好代码所必须的，全书分为7个大的章节。
1. 熟悉Objective-C
2. 对象、消息、运行时
3. 接口和API设计
4. 协议和分类
5. 内存管理
6. 块与大中枢派发（block和GCD）
7. 系统框架
下面开始本书的阅读。
<!--more-->
##熟悉Objective-C
###了解Objective-C语言的起源
Objective-C语言使用“消息结构”而非“函数调用”，由Smalltalk演化而来。
两中调用方式的区别在于：
- 消息结构的语言，运行时所需执行的代码由运行环境来决定。
- 使用函数调用的语言，由编译器决定。
如果调用的函数是多态的，那么在运行时就要按照“虚方法表”来查出到底应该执行哪个函数实现。采用消息结构的语言，无论是否多态，总是在运行时才会去查找所要执行的方法。
所有Objective-C语言的对象所占内存总是分配在“堆空间”中，绝不会分配在“栈”上。举例：
```
NSString *string = @"example string";
而不能
NSString string;
// error: interface type cannot be statically allocated
```
所谓的浅拷贝，
```
NSString *someString = @"the string";
NSString *anotherString = someString;
```
是因为，此时的内存布局是 someString和anotherString分配在栈上，而他们存储的都是内存特定区域（"the string"）的地址。
分配在堆上的内存必须直接管理，而分配在栈上用于保存变量的内存则会在其栈帧弹出时自动清理。

Objective-C代码中，会遇到一些定义里不含*的变量，这些变量保存的不是Objective-C对象，比如CoreGraphics框架中的CGRect
```
typedef struct CGRect {
  CGPoint origin;
  CGSize size;
} CGRect;
```
与创建结构体相比，创建对象还需要额外开销，对于这种整个系统框架都在使用的类型，如果只需要保存int、float、double、char的话，那就用struct可以提高性能。
###在类的头文件中尽量少引入其他头文件
看到这个标题一开始还不能理解，慢慢的才知道尽管#import不会像#include那样会出现重复引用的情况，但是如果在头文件.h中大量使用#import会增加编译时间，而解决这一问题，最为优雅的方法就是使用“向前声明”该类，@class，告诉编译器有这么一个类存在就可以了，但是作为实现.m文件而言，就必须知道引用类中的所有接口的细节，代码如下：
```
//EOCPerson.h
#import <Foundation/Foundation.h>

@class EOCEmployer;

@interface EOCPerson : NSObject
@property (nonatomic, copy) NSString *firstName;
@property (nonatomic, copy) NSString *lastName;
@property (nonatomic, strong) EOCEmployer *employer;
@end

//EOCPerson.m
#import "EOCPerson.h"
#import "EOCEmployer.h"

@implementation EOCPerson
//Implement of methods
@end
```
如果遇到编译器需要了解具体的定义的方法的时候，则必须要使用#import来引入，比如父类继承，协议的引入。
第二条#import是难免的，所以最好把协议单独放在一个头文件中，如果不这样做，把协议放在某个大的文件中，如果要引入此协议，就必定会引入那个头文件中的全部内容，这样就会产生相互依赖的问题，而且会增加编译的时间。
依赖关系不宜过于复杂，每次引用头文件的时候多要先问问自己有没有必要。如果可以使用向前声明代替，就不要引入。如果因为要实现属性、实例变量或者要遵循协议而必须引入头文件，则应尽量将其移至“class-continuation”中。
###多用字面语法，少用与之等价的方法
用字面量语法，其实就是只直接用数值来创建变量。
```
1. 使用
NSString *someString = @"Effective Objective-C 2.0";
而不用
NSString *someString = [[NSString alloc] initWithString: @"another string"];
2. 使用
NSNumber *intNumber = @1;
NSNumber *floatNumber = @2.5f;
NSNumber *doubleNumber = @3.14159;
NSNumber *boolNumber = @YES;
NSNumber *charNumber = @'a';
而不用
NSNumber *someNumber = [NSNumber numberWithInt:1];
3. 使用
NSArray *animals = @[@"cat", @"dog", @"mouse", @"badger"];
而不用
NSArray *animals = [NSArray arrayWithObjects:@"cat", @"dog", @"mouse", @"badger", nil];
4. 使用
NSString *dog = animals[1];
而不用
NSString *dog = [animals objectAtIndex: 1];
5. 使用
NSDictionary *personData = @{@"firstName" : @"Matt",
                             @"lastName" : @"Galloway",
                             @"age" : @28};
而不用
NSDictionary *personData = [NSDictionary dictionaryWithObjectsAndKeys:
                            @"Matt", @"firstName",
                            @"Galloway", @"lastName",
                            [NSNumber numberWithInt:28], @"age",
                            nil];
6. 使用
NSString *lastName = personData[@"lastNmae"];
而不用
NSString *lastName = [personData objectForKey:@"lastName"];
```
对于数组来说，采用字面量方法，可以更好的发现问题，抛出异常，而采用arrayWithObjects方法，会因为其中的某个值为nil，而将问题暂时的忽略掉，未来很难发现。
不足是在创建可变量时
```
NSMutableArray *mutable = [@[@1, @2, @3, @4, @5] mutableCopy];
```
>现在是广告时间，刚刚看了一篇[Rob Napier](http://robnapier.net/)写的[《我不懂Swift语言》](http://robnapier.net/i-dont-know-swift?utm_content=buffer6acce&utm_medium=social&utm_source=twitter.com&utm_campaign=buffer)([译文](http://tech2ipo.com/79181))，里面讲到Swift的地基已经打好，大楼才刚刚起步，一切都是尝试和摸索，现在加入进去，会打开新的天地，时间是去年7月份，距离Swift发布1个月。

###多用类型常量，少用#define预处理指令
```
使用   static const NSTimeInterval kAnimationDuration = 0.3
而不用 #define ANIMATION_DURATION 0.3
```
因为可以清楚的表明常量的含义，常量的类型。
这里要注意常量的名称，如果局限于“编译单元”，也就是“实现文件”之内，则在前面加字母k，若常量在类外可见，则通常以类名为前缀。
变量一定要用同时用static和const来声明，这样如果修改变量的话，编译器会报错。
如果在头文件中使用extern来声明全局常量，并在相关文件中定义其值，这种常量会出现在全局符号表中，所以名称需要加以区分，通常使用与之相关的类名做前缀。  
###用枚举表示状态、选项、状态码
关于枚举首先要对枚举变量起一个简单易懂的名字，定义的枚举类型如果没有特殊指名，每个枚举变量会在第一个的基础上依次+1，这样固然是可以的，但是如果遇到多个枚举常量可以同时存在，这样的方法就不是一个明智的选择
```
enum UIViewAutoresizing {
    UIViewAutoresizingNode                  = 0,
    UIViewAutoresizingFlexibleLeftMargin    = 1 << 0,
    UIViewAutoresizingFlexibleWidth         = 1 << 1,
    UIViewAutoresizingFlexibleRightMargin   = 1 << 2,
    UIViewAutoresizingFlexibleTopMargin     = 1 << 3,
    UIViewAutoresizingFlexibleHeight        = 1 << 4,
    UIViewAutoresizingFlexibleBottomMargin  = 1 << 5,
}
```
这样的结构就像一个开关一样，可以使用位运算来控制每个选项。
然后就是使用NS_ENUM与NS_OPTIONS宏来定义枚举类型，并指名其底层的数据类型。
```
typedef NS_ENUM(NSUInteger, EOCConnectionState) {
    EOCConnectionStateDisconnected,
    EOCConnectionStateConnecting,
    EOCConnectionStateConnected,
};
typedef NS_OPTION(NSUInteger, EOCPermittedDirection) {
    EOCPermittedDirectionUp     = 1 << 0,
    EOCPermittedDirectionDown   = 1 << 1,
    EOCPermittedDirectionLeft   = 1 << 2,
    EOCPermittedDirectionRight  = 1 << 3,
};
```
在使用switch语句的时候不要实现default分支。这样的话，如果加入新的枚举之后，编译器就会提醒开发者，如果加入default，就变成了不可控。
##对象、消息、运行时
###理解“属性”这一概念
在Objective-C中很少采用类似C++和Java的定义类的方式，因为这样写的话，对象布局在编译期就已经固定了，只要碰到访问类变量的代码，编译器就会把其替换为“偏移量（offset）”这个偏移量是“硬编码”，标识该变量距离存放对象的内存区域的起始地址有多远。如果在没有变化的时候是没有问题的，如果在前面就添加定义了一个变量，这样的话，偏移量会发生变化，就无法访问到正确的内存单元。对于这类问题每种语言都有自己的解决方案，Objective-C的做法是把实例变量当做是存储偏移量所用的“特殊变量”，交由“类对象”保管。
当然还有一种解决办法就是，就是不直接访问实例变量，而是采用存取方法来做。这是@property语法就派上用场了。
####属性的特质
这一块要单独拿出来写一下
#####原子性
- atomic 原子的
- nonatomic 非原子的，不使用同步锁
#####读写权限
- readwrite 特质用用getter和setter方法，若该属性由@synthesize实现，则编译器会自动生成两个方法。
- readonly 仅拥有只读属性，只有当@synthesize时，才会合成其方法。
#####内存管理语义
- assign 针对“纯量类型”（CGFloat， NSInteger等）简单赋值操作。
- strong “拥有关系”，设置方法会先保留新值，在释放旧值，在将新值设置上去。
- weak “非拥有关系”，设置方法既不保留新值，也不释放旧值，同assign类似，然而在属性所指的对象销毁时，属性值也会随之清空。
- unsafe_unretained 语义同assign相同，但是从命名上也可以知道它是不安全的，之前在weak没出现的时候使用，现在基本由weak替代，当目标对象销毁时，该属性值不会自动清空。
- copy 表达的所属关系与strong类似，不同的是，设置方法不保留新值，而是将其“拷贝”，只要实现属性所用的对象是“可变的”（mutable），就应该在设置新属性的时候拷贝一份。
#####方法名
自定义获取方法
```
@property (nonatomic, getter=isOn) BOOL on;
```
自定义设置方法不太常见。

开发iOS程序时，应该使用nonatomic，因为使用atomic会严重影响性能。
###在对象内部尽量直接访问实例变量
在写入实例变量时，通过其“设置方法”来做，而在读取实例变量时，则直接访问。这样既可以提高读取操作的速度，又能控制对属性的写入操作。之所以要通过“设置方法”来写入实例变量，原因在于，这样做能够确保相关的属性的“内存管理语义”得以贯彻。
如果采用了lazy initialization，就必须通过“获取方法”来访问属性。
```
- (EOCBrain *)brain {
    if (!_brain) {
        _brain = [Brain alloc];
    }
    return _brain;
}
```
在初始化方法既dealloc方法中，总应该直接通过实例变量来读写数据。