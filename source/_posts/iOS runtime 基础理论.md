---
layout: post
title: 'iOS runtime 基础理论'
comments: true
date: 2025-02-06 20:01:51
categories: iOS
tags: [笔记]
---

iOS的runtime分为三个阶段：消息发送、动态方法解析和消息转发。

<!--more-->

首先，我们来看消息发送阶段。当我们在代码中通过[]给对象发送一个消息时，实际上就会转化为对objc_MsgSend()函数的调用。这个函数首先会检查消息接收者是否为nil，如果是nil，则直接返回nil。然后，它会从方法缓存中查找对应的方法。如果找到了，就直接调用这个方法。如果没找到，它会递归地在父类的方法缓存列表和父类的方法列表中查找。如果整个过程下来都找不到需要调用的方法，就会进入动态方法解析阶段。

动态方法解析阶段是一个非常重要的过程，它允许我们在运行时动态地添加或修改方法。当Runtime在方法缓存和父类中都无法找到对应的方法时，它会调用resolveInstanceMethod或resolveClassMethod函数来尝试动态地解析这个方法。如果这些方法返回YES，表示已经成功解析了方法，Runtime会再次尝试发送消息。如果返回NO，表示解析失败，Runtime会进入消息转发阶段。

消息转发阶段是对象接收到无法处理的消息时的最后一道防线。在这个阶段，Runtime会尝试通过其他途径来找到可以处理这个消息的方法。首先，它会调用forwardingTargetForSelector函数来询问对象是否愿意将这个消息转发给其他对象处理。如果这个函数返回了一个非nil的对象，Runtime就会将这个消息转发给这个对象。如果这个函数返回nil，Runtime就会进入完整的消息转发流程，包括调用methodSignatureForSelector和forwardInvocation这两个函数。

