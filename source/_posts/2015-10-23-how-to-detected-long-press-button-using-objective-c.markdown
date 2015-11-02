---
layout: post
title: "How to detected long press on a button using Objective C?"
date: 2015-10-23 15:43:02 +0700
comments: true
categories: 
---
Add gesture:

```objc
UIButton *button = [UIButton buttonWithType:UIButtonTypeRoundedRect];
UILongPressGestureRecognizer *longPress = [[UILongPressGestureRecognizer alloc] initWithTarget:self action:@selector(longPress:)];
[button addGestureRecognizer:longPress];
```
And implement to handle long event:

```objc
- (void)longPress:(UILongPressGestureRecognizer*)gesture {
    UIButton *button = (UIButton *)gesture.view;
//    if (gesture.state == UIGestureRecognizerStateEnded) {
//        Do something…
//    }
}
```