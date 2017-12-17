---
layout: post
title: How Did You Manage Constants in Your iOS App?
categories: [ios]
tags: [swift, xcode, ios, constants]
fullview: true
---

The reason to manages the Constants in app is: We have a lot of static values and which makes easier to know where they are when you wanna reuse them.

Step 1. Create a file with the name: Constants.swift

Step 2. Add this struct inside your created file:

```swift
import Foundation

struct Constants {
}
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

