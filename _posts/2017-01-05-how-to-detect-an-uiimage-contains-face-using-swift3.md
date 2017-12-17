---
layout: post
title: How to detect an UIImage is containing any face by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, face detection, uiimage]
fullview: true
---

Face Detection:

```swift
import ImageIO

extension UIImage {
  var hasFaces: Bool {
        let ciImage: CIImage = CIImage(cgImage: cgImage!)
        let options: [String: Any] = [CIDetectorAccuracy: CIDetectorAccuracyHigh]
        let faceDetector: CIDetector = CIDetector(ofType: CIDetectorTypeFace, context: nil, options: options)!
        let faces: [CIFaceFeature] = faceDetector.features(in: ciImage).flatMap({ $0 as? CIFaceFeature })
        return !faces.isEmpty
    }
}
```

Show time:

```swift
debugPrint(yourImage.hasFaces)
```

Nice coffee ☕️