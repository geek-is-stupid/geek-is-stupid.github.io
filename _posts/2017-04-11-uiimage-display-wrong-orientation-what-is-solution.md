---
layout: post
title: UIImage displays wrong orientation, what is solution?
categories: [ios]
tags: [swift, xcode, ios, UIImage, orientation]
fullview: true
---

Some time UImage show wrong orientation, and it make you annoyed!!!

Don’t worry, I’m here:

```swift
extension UIImage {
  func rotateCameraImageToProperOrientation(maxResolution: CGFloat) -> UIImage {
        let imgRef: CGImage = cgImage!
        let width: CGFloat = CGFloat(imgRef.width)
        let height: CGFloat = CGFloat(imgRef.height)

        var bounds: CGRect = CGRect(x: 0, y: 0, width: width, height: height)

        var scaleRatio: CGFloat = 1
        if width > maxResolution || height > maxResolution {
            scaleRatio = min(maxResolution / bounds.size.width, maxResolution / bounds.size.height)
            bounds.size.height = bounds.size.height * scaleRatio
            bounds.size.width = bounds.size.width * scaleRatio
        }

        var transform: CGAffineTransform = .identity
        let orient: UIImageOrientation = imageOrientation
        let imageSize: CGSize = CGSize(width: imgRef.width, height: imgRef.height)

        switch imageOrientation {
        case .up :
            transform = .identity
        case .upMirrored :
            transform = CGAffineTransform(translationX: imageSize.width, y: 0)
            transform = transform.scaledBy(x: -1, y: 1)
        case .down :
            transform = CGAffineTransform(translationX: imageSize.width, y: imageSize.height)
            transform = transform.rotated(by: CGFloat.pi)
        case .downMirrored :
            transform = CGAffineTransform(translationX: 0, y: imageSize.height)
            transform = transform.scaledBy(x: 1, y: -1)
        case .left :
            let storedHeight = bounds.size.height
            bounds.size.height = bounds.size.width
            bounds.size.width = storedHeight
            transform = CGAffineTransform(translationX: 0, y: imageSize.width)
            transform = transform.rotated(by: 3.0 * CGFloat.pi / 2.0)
        case .leftMirrored :
            let storedHeight = bounds.size.height
            bounds.size.height = bounds.size.width
            bounds.size.width = storedHeight
            transform = CGAffineTransform(translationX: imageSize.height, y: imageSize.width)
            transform = transform.scaledBy(x: -1, y: 1)
            transform = transform.rotated(by: 3.0 * CGFloat.pi / 2.0)
        case .right :
            let storedHeight = bounds.size.height
            bounds.size.height = bounds.size.width
            bounds.size.width = storedHeight
            transform = CGAffineTransform(translationX: imageSize.height, y: 0)
            transform = transform.rotated(by: CGFloat.pi / 2.0)
        case .rightMirrored :
            let storedHeight: CGFloat = bounds.size.height
            bounds.size.height = bounds.size.width
            bounds.size.width = storedHeight
            transform = CGAffineTransform(scaleX: -1, y: 1)
            transform = transform.rotated(by: CGFloat.pi / 2.0)
        }

        UIGraphicsBeginImageContext(bounds.size)
        let context = UIGraphicsGetCurrentContext()

        if orient == .right || orient == .left {
            context!.scaleBy(x: -scaleRatio, y: scaleRatio)
            context!.translateBy(x: -height, y: 0)
        } else {
            context!.scaleBy(x: scaleRatio, y: -scaleRatio)
            context!.translateBy(x: 0, y: -height)
        }

        context!.concatenate(transform)
        context!.draw(imgRef, in: CGRect(x: 0, y: 0, width: width, height: height))

        let imageCopy = UIGraphicsGetImageFromCurrentImageContext()
        UIGraphicsEndImageContext()

        return imageCopy!
    }
}
```

Now just update your image:

```swift
let maxResolution: CGFloat = max(yourImage.size.width, yourImage.size.height)
let updatedImage: UIImage = yourImage.rotateCameraImageToProperOrientation(maxResolution: maxResolution)
```

See you!!!