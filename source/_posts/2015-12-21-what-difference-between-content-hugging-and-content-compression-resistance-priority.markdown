---
layout: post
title: "What difference between Content Hugging and Content Compression Resistance priority - Cocoa Autolayout?"
date: 2015-12-21 11:04:00 +0700
comments: true
categories: 
---
### Summary:
* Hugging: Content does not want to grow
* Compression Resistance: Content does not want to shrink

### Example:
I have a button here: [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Click Me&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;]

| # | <500 | >500 |
|---|:----:|:----:|
| Hugging | [&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Click Me&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;] | [Click Me] |
| Compression Resistance | [Cli…] | [Click Me] |