---
layout: post
title: How to center text vertically in an UITextView by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, center, vertically, UITextView]
fullview: true
---

I create a custome text view called by: `VerticallyCenteredTextView`

```swift
class VerticallyCenteredTextView: UITextView {
    override var contentSize: CGSize {
        didSet {
            var topCorrection = (bounds.size.height - contentSize.height * zoomScale) / 2.0
            topCorrection = max(0, topCorrection)
            contentInset = UIEdgeInsets(top: topCorrection, left: 0, bottom: 0, right: 0)
        }
    }
}
```

# 😂😂😂