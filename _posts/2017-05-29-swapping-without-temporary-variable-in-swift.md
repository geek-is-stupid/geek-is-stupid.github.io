---
layout: post
title: Swapping Without Temporary Variable in Swift
categories: [ios]
tags: [swift, xcode, ios, temporary, variable]
fullview: true
---

Using a temporary is normal way, try something new :]

#### Using + and -:

```swift
var a = 5
var b = 6
a = a + b
b = a - b
a = a - b
```

#### Using tuple:
```swift
var a = 5
var b = 6
(a, b) = (b, a)
```