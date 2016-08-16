---
layout: post
title: "Adjust custom font verical alignment - xcode"
date: 2016-08-16 10:05:47 +0700
comments: true
categories: 
---
Some time when you custom font in your project, and the font display not correctly, so how to fix that:

Step 1. Download [Apple Font Tool Suite](https://developer.apple.com/fonts/) command line utilities.

Step 2. Go to the folder which contains your font and run command:
`ftxdumperfuser -t hhea -A d Bold.ttf`

Step 3. It will generate Bold.hhea.xml automatically, open it and edit `ascender` value. In my case I changed it from `750` to `1200`

Step 4. Run: `ftxdumperfuser -t hhea -A f Bold.ttf`

And now your font is updated!!!