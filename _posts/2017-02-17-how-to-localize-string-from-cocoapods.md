---
layout: post
title: How to localise a string from CocoaPods by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, localise, string, CocoaPods]
fullview: true
---

Last time I already introduced to how to localized in a simple project:

[http://geek-is-stupid.github.io/blog/2015/10/23/how-to-manage-all-localized-string-in-your-project/](http://geek-is-stupid.github.io/blog/2015/10/23/how-to-manage-all-localized-string-in-your-project/)

Now I will show you how to localize from your CocoaPods dependences!

The simple way how you get the localized string:

```swift
let string = NSLocalizedString("Geek is stupid", comment: "")
```

This statement will using main bundle to load all `*.lproj` files, but if you load from you CocoaPods it is not main bundle anymore! 😱

Cool, so if I have a library with the name is: `YourCocoaPodsDependenceName`, so we might to do 3 things:

######1. Add a global localized string method:

```swift
static func localizedStringForKey(_ key: String, comment: String) -> String {
  let bundleClass = Bundle(for: YourCocoaPodsDependenceName.self)
  let bundlePath = bundleClass.path(forResource: "YourCocoaPodsDependenceName", ofType: "bundle")!
  let bundle = Bundle(path: bundlePath)!
  let string = bundle.localizedString(forKey: key, value: nil, table: nil)
  return string
}
```

######2. Using `localizedStringForKey(_: comment:)` instead of `NSLocalizedString`:

```swift
let string = localizedStringForKey("Geek is stupid", comment: "")
```

######3. Hold on, we might to update the `*.podspec` to tell `CocoaPods` that we are adding a bundle:

```swift
//...
s.resource_bundle = { "YourCocoaPodsDependenceName" => ["YourCocoaPodsDependenceName/**/*.lproj"] }
//..
```

Time to 🚀🚀🚀!