---
layout: post
title: "How to localized string with enum in Swift?"
date: 2015-10-23 15:04:27 +0700
comments: true
categories: 
---
1. Fist thing, please don't use localized string with dynamic text, it doesn't work
2. How it works:

```swift 
enum Status: String {
	case Verified = "Verified"
	case Unverified = "Unverified"

	func localizedString() -> String {
		switch self {
		case .Verified:
			return NSLocalizedString("Verified", comment: "")
		case .Unverified:
			return NSLocalizedString("Unverified", comment: "")
		}
	}
}
```
