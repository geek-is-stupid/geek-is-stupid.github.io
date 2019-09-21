---
layout: post
title: "How to get an iOS application's Scheme URL?"
categories: ios
tags: swift objective-c objc ios scheme url extract iMazing
fullview: true
---

From time to time we have to implement a feature that we have to open external apps from our apps!

So here are some steps to extract an iOS application's scheme URL

**Step 1:** Download the partner’s iOS app to your iPhone

**Step 2:** On your laptop, download iMazing application https://imazing.com/

**Step 3:** Open iMazing on your laptop
![1](https://user-images.githubusercontent.com/6329656/65371472-0cd0fb80-dc8e-11e9-8232-777d4bf2e68e.png)

**Step 4:** Search for the partner’s app and select download
![2](https://user-images.githubusercontent.com/6329656/65371473-0cd0fb80-dc8e-11e9-9ddd-bfa4e8cc7abd.png)

**Step 5**: After downloaded, right-click to export it
![Screen Shot 2019-08-07 at 10 17 27 AM](https://user-images.githubusercontent.com/6329656/65371474-0d699200-dc8e-11e9-94ec-a5e6a8ffb57f.png)

**Step 6**: Save the *.ipa

**Step 7**: Open your terminal and run: `bash extract-scheme-url.sh AppName.ipa`
```
Unzipping... 
✅ URL Schemes: 
partner_app_scheme_url_here 
```

[extract-scheme-url.sh]([https://gist.github.com/levantAJ/be52cc86dc5d476f23e4be75ae7905cc](https://gist.github.com/levantAJ/be52cc86dc5d476f23e4be75ae7905cc))
<script src="https://gist.github.com/levantAJ/be52cc86dc5d476f23e4be75ae7905cc.js"></script>