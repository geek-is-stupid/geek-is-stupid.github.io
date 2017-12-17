---
layout: post
title: How to go to iOS settings from your applications by using Swift3?
categories: [ios]
tags: [swift, xcode, ios]
fullview: true
---


OK let’s do something like this:

```swift
extension UIApplication {
  class func openSettings() {
        let url = NSURL(string: UIApplicationOpenSettingsURLString)
        UIApplication.sharedApplication().openURL(url!)
    }
}
```

Show time:

```swift
UIApplication.openSettings()
```

Hope you like it!

