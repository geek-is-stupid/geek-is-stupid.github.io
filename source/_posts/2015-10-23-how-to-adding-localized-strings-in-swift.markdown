---
layout: post
title: "How to adding localized strings in Swift?"
date: 2015-10-23 14:45:31 +0700
comments: true
categories: 
---
This heard like easy? 

- Yes, but language very complex, with each language we have different way to order of word in a sentence
- So if I have a name of user: <strong>```"Andrew McKay"```</strong>, and he has <strong>```"commented"```</strong> in a page:
The text on app will be: 

```swift
"Andrew McKay also commented"
```

The simple way:

```swift
let text = "\(user.name) " + NSLocalizedString("also commented", comment: "") 
```

<strong>The right way (I think):</strong>

```swift
let text = NSString.localizedStringWithFormat(NSLocalizedString("%@ also commented", comment: "") as NSString, user.name) as String
```