---
layout: post
title: How to measure loading time for iOS app?
categories: [ios]
tags: [swift, objective-c, xcode, ios, loading, time, start-up, cold-start]
fullview: true
---

There is a simple trick to show your app loading time included `dylib loading time`, `rebase/binding time`, `ObjC setup time`, `initializer time`, ...

<img width="528" alt="screenshot 2018-10-11 at 7 30 08 pm" src="https://user-images.githubusercontent.com/6329656/46912895-ed285200-cfab-11e8-9b78-ced2ce1a89d9.png">

Here how to setup:

1: Edit scheme

<img width="215" alt="screenshot 2018-10-14 at 12 24 12 pm" src="https://user-images.githubusercontent.com/6329656/46912901-13e68880-cfac-11e8-8b99-b7d5f4703cf0.png">

2: Add new Environment Variables with `DYLD_PRINT_STATISTICS` 

<img width="895" alt="screenshot 2018-10-14 at 12 25 35 pm" src="https://user-images.githubusercontent.com/6329656/46912911-70e23e80-cfac-11e8-95bc-5662de8e94b2.png">

3: Run and view in your console to see the results!