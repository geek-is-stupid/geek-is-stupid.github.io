---
layout: post
title: How to make an animation within an UIImageView from a list of images by using Swift?
categories: [ios]
tags: [swift, xcode, ios, UIImageView, UIImage, animations]
fullview: true
---

If you have some images like this:

```swift
let firstImage = UIImage(named: "first")
let secondImage = UIImage(named: "first")
let thirdImage = UIImage(named: "first")
```

How to display these images on UIImageView and it change image automatically?

Easy, try this:

```swift
let imageView = UIImageView()
imageView.animationImages = [firstImage, secondImage, thirdImage]
imageView.startAnimating()
```

So how to set number of loops?

```swift
imageView.animationRepeatCount = 0 // 0 means infinite (default is 0)
```

Hummm, ok so how to change velocity?

```swift
imageView.animationDuration = 0.5 // 0.5 seconds  for one cycle of images
```

Now stop it!

```swift
imageView.stopAnimating()
```

Fun, right?, cheers!




