---
layout: post
title: How to hide status bar by repss an uibutton in iOS 9 by using Swift?
categories: [ios]
tags: [swift, xcode, ios, status, bar, animations, uibutton, press]
fullview: true
---

#### With animations:

- Solution 1:

![https://stackoverzone.files.wordpress.com/2015/10/hide-status-bar.gif](https://stackoverzone.files.wordpress.com/2015/10/hide-status-bar.gif)

```swift
class ViewController: UIViewController {
  var hideStatusBar = false
  
  @IBAction func buttonTapped(button: UIButton) {
          hideStatusBar = !hideStatusBar
          UIView.animateWithDuration(0.25) { () -> Void in
              self.setNeedsStatusBarAppearanceUpdate()
          }
  }

  override func preferredStatusBarUpdateAnimation() -> UIStatusBarAnimation {
      return .Fade
  }

  override func prefersStatusBarHidden() -> Bool {
          return hideStatusBar
  }
}
```

Ref: [http://stackoverflow.com/questions/32808593/setstatusbarhidden-withanimation-deprecated-in-ios-9#answer-32808743](http://stackoverflow.com/questions/32808593/setstatusbarhidden-withanimation-deprecated-in-ios-9#answer-32808743)

- Solution 2:

```swift 
@IBAction fun buttonTapped(button: UIButton) {
  UIApplication.shared.setStatusBarHidden(false, withAnimation: .Fade)
}
```

But this was deprecated on iOS 9 and later :[

##### What should I do if: `"View controller-based status bar appearance" = NO`


```swift
UIApplication.shared.statusBarHidden = false
```

But this solution without animation :[







