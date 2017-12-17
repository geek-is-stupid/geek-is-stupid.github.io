---
layout: post
title: How to detect that a string is containing an emoji by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, string, emoji, contains]
fullview: true
---

We can do like this:

```swift
extension String {
  var containsEmoji: Bool {
        for scalar in unicodeScalars {
            switch scalar.value {
            case 0x3030, 0x00AE, 0x00A9,// Special Characters
            0x1D000...0x1F77F,          // Emoticons
            0x2100...0x27BF,            // Misc symbols and Dingbats
            0xFE00...0xFE0F,            // Variation Selectors
            0x1F900...0x1F9FF:          // Supplemental Symbols and Pictographs
                return true
            default:
                continue
            }
        }
        return false
    }
}
```

Show time:

```swift
let string: String = "Hello world!!! 😘😍🤗"
debugPrint(string.containsEmoji)
```

Have a nice day!!!