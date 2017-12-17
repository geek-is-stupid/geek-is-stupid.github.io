---
layout: post
title: How to Localise a String With Enum by using Swift?
categories: [ios]
tags: [swift, xcode, ios, NSLocalizeString, string, localise]
fullview: true
---

1. Fist thing, please don’t use localized string with dynamic text, it doesn’t work
2. How it works:

```swift
enum Status: String {
  case verified = "Verified"
  case unverified = "Unverified"

  var localizedString: String {
      switch self {
      case .verified:
          return NSLocalizedString("Verified", comment: "")
      case .unverified:
          return NSLocalizedString("Unverified", comment: "")
      }
  }
}
```

