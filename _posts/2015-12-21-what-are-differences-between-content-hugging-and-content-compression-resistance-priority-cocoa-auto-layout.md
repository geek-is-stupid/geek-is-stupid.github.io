---
layout: post
title: What are differences between Content Hugging and Content Compression Resistance Priority - Cocoa Autolayout?
categories: [ios]
tags: [swift, xcode, ios, Content Hugging, Content Compression Resistance Priority, Cocoa Autolayout]
fullview: true
---


###### Summary:

- Hugging: Content does not want to grow
- Compression Resistance: Content does not want to shrink

###### Example:

I have a button look like this:   [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Click Me&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]


| #             |     <500.     | >500  |
| ------------- |:-------------:|:-----:|
| Hugging	      |[&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Click Me&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]	| [Click Me] |
|Compression Resistance	|[Cli…]|[Click Me]|



