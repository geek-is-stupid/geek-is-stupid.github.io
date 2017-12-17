---
layout: post
title: How to adjust custom fonts vertical alignment - Xcode
categories: [ios]
tags: [swift, xcode, ios, uifont, alignment]
fullview: true
---

Some time when you custom font in your project, and the font display not correctly, so how to fix that:

Step 1. Download [Apple Font Tool](https://developer.apple.com/fonts/) Suite command line utilities.

Step 2. Go to the folder which contains your font and run command to get font's details: 

```swift
ftxdumperfuser -t hhea -A d Bold.ttf
```

Step 3. It will generate `Bold.hhea.xml` automatically, open it and edit ascender value. In my case I changed it from 750 to 1200

Step 4. Change vetical aligment for your font:

```swift
ftxdumperfuser -t hhea -A f Bold.ttf
```

And now your font is updated!!!