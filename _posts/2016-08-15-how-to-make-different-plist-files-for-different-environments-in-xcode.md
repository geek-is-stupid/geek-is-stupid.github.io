---
layout: post
title: Different .plist files for different environments in Xcode?
categories: [ios]
tags: [swift, xcode, ios, plist, enviroments]
fullview: true
---

If you have question like this title of this post, so you are my audience

OK now if I have 2 `.plist` files:

1. SERVICE-ACCOUNT-DEBUG.plist
2. SERVICE-ACCOUNT-RELEASE.plist

When app runs, depend on current environment we will rename one of them to `SERVICE-ACCOUNT.plist`:

If your app in DEBUG mode we will rename `SERVICE-ACCOUNT-DEBUG.plist` to `SERVICE-ACCOUNT.plist`

If your app in RELEASE mode we will rename `SERVICE-ACCOUNT-RELEASE.plist` to `SERVICE-ACCOUNT.plist`

Let's go to your `Build Phares` and creates new Run script:

![https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2016-08-17-different-plist-file-for-different-environment-in-xcode/1.png](https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2016-08-17-different-plist-file-for-different-environment-in-xcode/1.png)

Add this script:

```swift
if [ "${CONFIGURATION}" == "Release" ]; then
cp -r "${PROJECT_DIR}/PATH-TO-FILE/SERVICE-ACCOUNT-RELEASE.plist" "${BUILT_PRODUCTS_DIR}/${PRODUCT_NAME}.app/SERVICE-ACCOUNT.plist"
echo "Production plist copied"
elif [ "${CONFIGURATION}" == "Debug" ]; then
cp -r "${PROJECT_DIR}/PATH-TO-FILE/SERVICE-ACCOUNT-DEBUG.plist" "${BUILT_PRODUCTS_DIR}/${PRODUCT_NAME}.app/SERVICE-ACCOUNT.plist"
echo "Debug plist copied"
fi
```