---
layout: post
title: "Transfer decelerating of UIScrollView to another UIScrollView"
categories: ios
tags: swift ios UIScrollView gesture decelerate
fullview: true
---

## Context

<img width="499" alt="Screen Shot 2020-05-06 at 17 37 05" src="https://user-images.githubusercontent.com/6329656/81168337-01ce9f80-8fc1-11ea-9816-70a55cb69f29.png">

When the dragging of Outer UIScrollView is ended, and it continues decelerating, we want to transfer that decelerating to the Nested UIScrollView to make users feel they're only one UIScrollView

## Decelerator

I was inspired by this nice article about [Deceleration mechanics of UIScrollView](https://medium.com/@esskeetit/scrolling-mechanics-of-uiscrollview-142adee1142c) 

So this article will show you how to apply it to Outer/Nested UIScrollView 👆

I create a class called **ScrollingDecelerator**, and it will manage the transfering decelerating between outer and nested UIScrollView.

[ScrollingDecelerator](https://gist.github.com/levantAJ/7860d3053324e4fc20e12d5dd3c51fb1)

## How to use

### To transfer the decelerating from Outer UIScrollView to Nested UIScrollView:

For the Outer UIScrollView:

```
var outerDeceleration: ScrollingDeceleration?

func scrollViewWillEndDragging(_ scrollView: UIScrollView, withVelocity velocity: CGPoint, targetContentOffset: UnsafeMutablePointer<CGPoint>) {
    deceleration = ScrollingDeceleration(velocity: velocity, decelerationRate: scrollView.decelerationRate)
}

func scrollViewWillBeginDragging(_ scrollView: UIScrollView) {
    outerDeceleration = nil
}

func scrollViewDidEndDragging(_ scrollView: UIScrollView, willDecelerate decelerate: Bool) {
    outerDeceleration = nil
}
```


For the Nested UIScrollView:

```
let scrollingDecelerator = ScrollingDecelerator(scrollView: outerScrollView)

func scrollViewDidScroll(_ scrollView: UIScrollView) {
    scrollingDecelerator.invalidateIfNeeded()
}

// The place where to start deceleration from the Outer UIScrollView

scrollingDecelerator.decelerate(by: outerDeceleration)

```



## Ref

- [https://medium.com/@esskeetit/scrolling-mechanics-of-uiscrollview-142adee1142c](https://medium.com/@esskeetit/scrolling-mechanics-of-uiscrollview-142adee1142c)
- [https://developer.apple.com/videos/play/wwdc2018/803/](https://developer.apple.com/videos/play/wwdc2018/803/)


