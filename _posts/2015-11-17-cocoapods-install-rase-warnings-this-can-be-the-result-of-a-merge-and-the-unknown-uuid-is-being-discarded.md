---
layout: post
title: CocoaPods Install Rase Warning "This Can Be the Result of a Merge and the Unknown UUID Is Being Discarded."
categories: [ios]
tags: [swift, xcode, ios, cocoapods, install, warnings, merge, unknown, uuid, PBXGroup]
fullview: true
---

Have you ever get some warnings like this?

> [!] <‘PBXGroup name='Filter’ UUID=‘99CE81391BA80D2D00CA7B29’>‘ attempted to initialize an object with an unknown UUID. '995E161E1BAAA6C2002153EC’ for attribute: ‘children’. This can be the result of a merge and the unknown UUID is being discarded.

![https://stackoverzone.files.wordpress.com/2015/10/pod-warning.png](https://stackoverzone.files.wordpress.com/2015/10/pod-warning.png)

Don't worries, I have a solution :]

1. Open any file by Xcode
2. On right side of Xcode, choose Show the File inspector > `Target Membership`
3. Try to uncheck and check the file on any target:

![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-10-03-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-10-03-pm.png)


