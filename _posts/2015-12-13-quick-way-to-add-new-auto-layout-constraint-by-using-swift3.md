---
layout: post
title: Quick qay to add new Auto Layout Constraints by using Swift
categories: [ios]
tags: [swift, xcode, ios, auto layout, constraints, NSLayoutConstraint]
fullview: true
---


What you will do when you wanna add some spaces between a label and a button? Or add width for a label?

```swift
let spaceConstraint = NSLayoutConstraint(
    item: label,
    attribute: .Leading,
    relatedBy: .Equal,
    toItem: button,
    attribute: .Trailing,
    multiplier: 1, constant: 20)
    
let widthConstraint = NSLayoutConstraint(
    item: label,
    attribute: .Width,
    relatedBy: .LessThanOrEqual,
    toItem: nil,
    attribute: .NotAnAttribute,
    multiplier: 0, constant: 200)

spaceConstraint.active = true
widthConstraint.active = true
```

OK, Now I will show you a simple way:

```swift
let spaceConstraint = label.leadingAnchor.constraintEqualToAnchor(button.trailingAnchor, constant: 20)
let widthConstraint = label.widthAnchor.constraintLessThanOrEqualToConstant(200)
spaceConstraint.active = true
widthConstraint.active = true
```

Cheers, 

    

