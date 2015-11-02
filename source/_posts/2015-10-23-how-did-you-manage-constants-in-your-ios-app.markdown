---
layout: post
title: "How did you manage Constants in your iOS app?"
date: 2015-10-23 14:37:59 +0700
comments: true
categories: 
---
The reason to manages the <strong>```Constants```</strong> in app is: We have a lot of static values and which makes easier to know where they are when you wanna reuse them.

Step 1. Create a file with the name: <strong>```Constants.swift```</strong>

Step 2. Add this struct inside your created file:

```swift
import Foundation

struct Constants {}
```

Step 3. In the class which you want to add new constant:

```swift
class Staff {
}

extension Constants {
    struct Staff {
        static let DefaultHeight = 1.70
    }
}
```
Step 4. Just use this wherever you want:

```swift
let height = Constants.Staff.DefaultHeight
```
