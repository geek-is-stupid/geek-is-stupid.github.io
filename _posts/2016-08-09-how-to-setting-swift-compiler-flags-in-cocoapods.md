---
layout: post
title: How to setting Swift Compiler Flags in CocoaPods?
categories: [ios]
tags: [swift, xcode, ios, CocoaPods, Swift Compiler Flags]
fullview: true
---

Someday your boss ask you to have staging server and production server to make data be pure, so you will find out this code for your api:

```swift
var host: NSURL {
    #if DEBUG
        return NSURL(string: "https://www.google.com.vn")!
    #else
        return NSURL(string: "http://geek-is-stupid.github.io")!
    #endif
}
```

And you go to `Build Settings` and set the `DEBUG` symbol in Swift `Compiler - Custom Flags` section for the `Other Swift Flags` key via a `-D DEBUG` entry. See the following screenshot for an example:

![https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2016-08-17-setting-swift-compiler-flags-in-cocoapods/1.png](https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2016-08-17-setting-swift-compiler-flags-in-cocoapods/1.png)

That works, hummm but what happen if I move the host to a library and I use CocoaPods to manages dependencies, it won’t work any more.

So the way to let it jump :)

Open your Podfile and add this script to the bottom:

```swift
post_install do |installer|
    installer.pods_project.build_configurations.each do |config|
        if config.name == 'Debug'
            config.build_settings['OTHER_SWIFT_FLAGS'] ||= ['$(inherited)', '-D DEBUG']
        elsif config.name == 'Release'
            config.build_settings['OTHER_SWIFT_FLAGS'] ||= ['$(inherited)', '-D RELEASE']
        end
    end
end
```


