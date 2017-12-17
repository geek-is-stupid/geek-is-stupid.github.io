---
layout: post
title: How to set placeholder text color for UITextField by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, placeholder, color, UITextField]
fullview: true
---

Worked on UI is interesting part of iOS development

```swift
extension UITextField {
    func setPlaceholderColor(color: UIColor) {
        if let placeholder = placeholder {
            attributedPlaceholder = NSAttributedString(string: placeholder, attributes: [NSForegroundColorAttributeName: color])
        }
    }
}
```

Way to use:

```swift
let textField = UITextField()
textField.setPlaceholderColor(.redColor())
```

Let’s try, it’s delicious! 🌯🌮🍗🍜

