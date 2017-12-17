---
layout: post
title: How to set background color for status bar in iOS application by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, UIApplication, background color, status bar]
fullview: true
---

There are many way to do, I show a trick, don’t follow me :D

```swift
extension UIApplication {
  class func setStatusBarBackgroundColor(color: UIColor) {
        guard let statusBar = UIApplication.sharedApplication().valueForKey("statusBarWindow")?.valueForKey("statusBar") as? UIView else { return }
        statusBar.backgroundColor = color
    }
}
```

Do and see the change:

```swift
UIApplication.setStatusBarBackgroundColor(.grayColor())
```