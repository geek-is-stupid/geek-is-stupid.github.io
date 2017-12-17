---
layout: post
title: How to compress an UIImage by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, compress, UIImage, NSData, png, jpg, jpeg, quality]
fullview: true
---

Following:

```swift
extension UIImage {
    var data: Data? { return UIImagePNGRepresentation(self) }
    var highestQualityJPEGData: Data? { return UIImageJPEGRepresentation(self, 1.0)  }
    var highQualityJPEGData: Data?    { return UIImageJPEGRepresentation(self, 0.75) }
    var mediumQualityJPEGData: Data?  { return UIImageJPEGRepresentation(self, 0.5)  }
    var lowQualityJPEGData: Data?     { return UIImageJPEGRepresentation(self, 0.25) }
    var lowestQualityJPEGData: Data?   { return UIImageJPEGRepresentation(self, 0.0)  }
}
```

Time is your!!!

```swift
yourImage.data
yourImage.highestQualityJPEGData
```

# 🏙🛤🎑🌄🌁🌉