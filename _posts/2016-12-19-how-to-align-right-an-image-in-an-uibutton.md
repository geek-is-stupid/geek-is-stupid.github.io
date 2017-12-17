---
layout: post
title: How to align an image in UIButton to right side by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, UIButton, align, right, left, image]
fullview: true
---

Everything you need is:

```swift
extension UIButton {
  func imageToRight() {
      transform = CGAffineTransform(scaleX: -1.0, y: 1.0)
      titleLabel?.transform = CGAffineTransform(scaleX: -1.0, y: 1.0)
      imageView?.transform = CGAffineTransform(scaleX: -1.0, y: 1.0)
  }
}
```

Show time:

```swift
button.imageToRight()
```

Cheers 👉