---
layout: post
title: "How to hide status bar by press a button in iOS 9?"
date: 2015-10-27 16:12:23 +0700
comments: true
categories: 
---
## With animations:
* Solution 1:

<a href="https://stackoverzone.files.wordpress.com/2015/10/hide-status-bar.gif"><img align='center' src="https://stackoverzone.files.wordpress.com/2015/10/hide-status-bar.gif" /></a>

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

Ref: <a href="http://stackoverflow.com/questions/32808593/setstatusbarhidden-withanimation-deprecated-in-ios-9#answer-32808743">http://stackoverflow.com/questions/32808593/setstatusbarhidden-withanimation-deprecated-in-ios-9#answer-32808743</a>

* Solution 2:

```swift
@IBAction fun buttonTapped(button: UIButton) {
	UIApplication.sharedApplication().setStatusBarHidden(false, withAnimation: .Fade)
}
```
But this was deprecated on iOS 9 and later

## What should I do if: ```"View controller-based status bar appearance" = NO```

```swift
UIApplication.sharedApplication().statusBarHidden = false
```
This solution without animation :(