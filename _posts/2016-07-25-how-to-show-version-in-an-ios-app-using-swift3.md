---
layout: post
title: How to show version in an iOS application?
categories: [ios]
tags: [swift, xcode, ios, version, UIApplication]
fullview: true
---

Let make your code be complicated…

```swift
struct Version {
    let short: String
    let full: String
    let display: String
}
```

And make an extension for `UIApplication`:

```swift
extension UIApplication {
  static var version: Version {
        let short = NSBundle.mainBundle().objectForInfoDictionaryKey("CFBundleShortVersionString") as! String
        let full = NSBundle.mainBundle().objectForInfoDictionaryKey(kCFBundleVersionKey as String) as! String
        let display = full.containsString(".") ? full : "\(short).\(full)"
        return Version(short: short, full: full, display: display)
  }
}
```


Your time:

```swift
debugPrint(UIApplication.version.short)
debugPrint(UIApplication.version.full)
debugPrint(UIApplication.version.display)
```