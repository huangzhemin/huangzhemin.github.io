layout: post
comments: true
title: injection
date: 2018-07-02 19:44:17
categories: 工作
tags: [毕业, 随笔, 记录]
---
injection 注入方式

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
#if DEBUG
    [[NSBundle bundleWithPath:@"/Applications/InjectionIII.app/Contents/Resources/iOSInjection.bundle"] load];
    [[NSBundle bundleWithPath:@"/Applications/InjectionIII.app/Contents/Resources/tvOSInjection.bundle"] load];
    [[NSBundle bundleWithPath:@"/Applications/InjectionIII.app/Contents/Resources/macOSInjection.bundle"] load];
#endif
    return YES;
}
```

然后在需要调整的类中实现injected方法

```
- (void)injected {
    [self setNeedsLayout];
    [self layoutIfNeeded];
}
```
