---
layout: post
title: How to align vertical the image and the title in an UIButton by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, image, title, uibutton, align, vertically]
fullview: true
---

Following code below:

```swift
extension UIButton {
  func alignImageAndTitleVertically(padding: CGFloat = 4.0) {
        let imageSize = imageView!.frame.size
        let titleSize = titleLabel!.frame.size
        let totalHeight = imageSize.height + titleSize.height + padding

        imageEdgeInsets = UIEdgeInsets(
            top: -(totalHeight - imageSize.height),
            left: 0,
            bottom: 0,
            right: -titleSize.width
        )

        titleEdgeInsets = UIEdgeInsets(
            top: 0,
            left: -imageSize.width,
            bottom: -(totalHeight - titleSize.height),
            right: 0
        )
    }
}
```

Show time:

```swift
button.alignImageAndTitleVertically()
```

Good luck 🔘🔲