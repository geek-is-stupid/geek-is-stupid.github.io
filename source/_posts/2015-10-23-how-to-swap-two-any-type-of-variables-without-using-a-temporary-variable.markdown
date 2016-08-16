---
layout: post
title: "How to swap two any type of variables without using a temporary variable?"
date: 2015-10-23 15:48:03 +0700
comments: true
categories: 
---
This method using for any numeric type of variables:

```
a = a + b;
b = a - b;
a = a - b;
```
