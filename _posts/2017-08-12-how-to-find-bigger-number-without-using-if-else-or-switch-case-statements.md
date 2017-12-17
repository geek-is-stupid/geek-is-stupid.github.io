---
layout: post
title: How to Find Bigger Number Without Using If/else or Switch/case Statements?
categories: [ios]
tags: [swift, xcode, ios, number, bigger]
fullview: true
---


Suppose we have 2 positive integer numbers: `a` and `b` Now print out which number is the bigger? Note that without using `if/else`, `switch/case` or comparing operations! 😎

For example: `a = 5`, `b = 7` The bigger is: `b`

It’s:

```
((a + b) + abs(a - b)) / 2
```