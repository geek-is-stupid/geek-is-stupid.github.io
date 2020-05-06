---
layout: post
title: "Transfer decelerating of UIScrollView to another UIScrollView"
categories: ios
tags: swift ios UIScrollView gesture decelerate
fullview: true
---


<img width="499" alt="Screen Shot 2020-05-06 at 17 37 05" src="https://user-images.githubusercontent.com/6329656/81168337-01ce9f80-8fc1-11ea-9816-70a55cb69f29.png">

When the dragging of Outer UIScrollView is ended, and it continues decelerating, we want to transfer that decelerating to the Nested UIScrollView to make users feel they're only one UIScrollView

## Decelerator

I was inspired by this nice article about [Deceleration mechanics of UIScrollView](https://medium.com/@esskeetit/scrolling-mechanics-of-uiscrollview-142adee1142c) 

So this article will show you how to apply it to Outer/Nested UIScrollView 👆

I create a class called **[ScrollingDecelerator](https://gist.github.com/levantAJ/7860d3053324e4fc20e12d5dd3c51fb1)**, and it will manage the transfering decelerating between outer and nested UIScrollView.


## How to use

### Downward Decelerating: Outer UIScrollView ==> Nested UIScrollView

For the **Outer UIScrollView**:

```swift
    var outerDeceleration: ScrollingDeceleration?

    func scrollViewWillEndDragging(_ scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>) {
        outerDeceleration = ScrollingDeceleration(velocity: velocity, decelerationRate: scrollView.decelerationRate)
    }

    func scrollViewWillBeginDragging(_ scrollView: UIScrollView) {
        outerDeceleration = nil
    }

    func scrollViewDidEndDragging(_ scrollView: UIScrollView, willDecelerate decelerate: Bool) {
        outerDeceleration = nil
    }
```

In some place where you need to pass the **outerDeceleration** into the **Nested UIScrollView**


Continue downward decelerating for the **Nested UIScrollView**:

```swift
    let nestedScrollingDecelerator = ScrollingDecelerator(scrollView: nestedScrollView)

    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        nestedScrollingDecelerator.invalidateIfNeeded()
    }
```

When the **Nested UIScrollView** got the **outerDeceleration**, we can continue the downward decelerating by:

```swift
    nestedScrollingDecelerator.decelerate(by: outerDeceleration)

```

### Upward Decelerating: Outer UIScrollView <== Nested UIScrollView

For the **Nested UIScrollView**: We constructed the deceleration, and send it out!
 

```swift
    var nestedDeceleration: ScrollingDeceleration?

    func scrollViewWillEndDragging(_ scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>) {
        nestedDeceleration = ScrollingDeceleration(velocity: velocity, decelerationRate: scrollView.decelerationRate)
    }

    func scrollViewWillBeginDragging(_ scrollView: UIScrollView) {
        nestedDeceleration = nil
    }

    func scrollViewDidEndDragging(_ scrollView: UIScrollView, willDecelerate decelerate: Bool) {
        nestedDeceleration = nil
    }
```

And pass the **nestedDeceleration** out to the **Outer UIScrollView**


And for the **Outer UIScrollView**:

```swift
    let outerScrollingDecelerator = ScrollingDecelerator(scrollView: nestedScrollView)

    func scrollViewDidScroll(_ scrollView: UIScrollView) {
        outerScrollingDecelerator.invalidateIfNeeded()
    }
```

When the **Outer UIScrollView** got the **nestedDeceleration**, we can continue the upward decelerating by:

```swift
    souterScrollingDecelerator.decelerate(by: nestedDeceleration)

```

## Ref

- [https://medium.com/@esskeetit/scrolling-mechanics-of-uiscrollview-142adee1142c](https://medium.com/@esskeetit/scrolling-mechanics-of-uiscrollview-142adee1142c)
- [https://developer.apple.com/videos/play/wwdc2018/803/](https://developer.apple.com/videos/play/wwdc2018/803/)
- [ScrollingDecelerator.swift](https://gist.github.com/levantAJ/7860d3053324e4fc20e12d5dd3c51fb1)


