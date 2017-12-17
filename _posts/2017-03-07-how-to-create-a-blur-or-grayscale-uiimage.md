---
layout: post
title: How to create a blur or grayscale UIImage by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, blur, grayscale, UIImage]
fullview: true
---

Basically we are using `CIFilter` for everything

Who want a BLUR image:

```swift
extension UIImage {
  var blur: UIImage {
        let inputImage: CIImage = CIImage(image: self)!
        let blurFilter: CIFilter = CIFilter(name: "CIGaussianBlur")!
        blurFilter.setDefaults()
        blurFilter.setValue(inputImage, forKey: "inputImage")
        blurFilter.setValue(50.0, forKey: "inputRadius")
        let outputImage: CIImage = blurFilter.outputImage!
        let context: CIContext = CIContext()
        let cgImage: CGImage = context.createCGImage(outputImage, from: outputImage.extent)!
        let image: UIImage = UIImage(cgImage: cgImage)
        return image
    }
}
```

Now it’s GRAYSCALE:

```swift
extension UIImage {
  var grayscale: UIImage {
        let ciImage: CIImage = CIImage(image: self)!

        let colorFilter: CIFilter = CIFilter(name: "CIColorControls")!
        colorFilter.setValue(ciImage, forKey: kCIInputImageKey)
        colorFilter.setValue(0.0, forKey: kCIInputBrightnessKey)
        colorFilter.setValue(0.0, forKey: kCIInputSaturationKey)
        colorFilter.setValue(1.1, forKey: kCIInputContrastKey)
        let intermediateImage: CIImage = colorFilter.outputImage!

        let exposureFilter: CIFilter = CIFilter(name:"CIExposureAdjust")!
        exposureFilter.setValue(intermediateImage, forKey: kCIInputImageKey)
        exposureFilter.setValue(0.7, forKey: kCIInputEVKey)
        let output: CIImage! = exposureFilter.outputImage

        let context: CIContext = CIContext(options: nil)
        let cgImage: CGImage = context.createCGImage(output, from: output.extent)!

        let image: UIImage = UIImage(cgImage: cgImage, scale: scale, orientation: imageOrientation)
        return image
    }
}
```

Cooool:

```swift
let bluredUmage: UIImage = yourImage.blur
let grayscaledImage: UIImage = yourImage.grayscale
```

Feel the beat 🤪🤪🤪