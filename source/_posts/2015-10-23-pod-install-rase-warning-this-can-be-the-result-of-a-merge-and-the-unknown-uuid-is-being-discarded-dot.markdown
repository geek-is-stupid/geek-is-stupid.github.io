---
layout: post
title: "Pod install rase warning \"This can be the result of a merge and  the unknown UUID is being discarded.\""
date: 2015-10-23 20:58:21 +0700
comments: true
categories: 
---
<span style="color:orange; font-family:Menlo;">
[!] <'PBXGroup name='Filter' UUID='99CE81391BA80D2D00CA7B29'>' attempted to initialize an object with an unknown UUID. '995E161E1BAAA6C2002153EC' for attribute: 'children'. This can be the result of a merge and  the unknown UUID is being discarded.
</span>

<a href="https://stackoverzone.files.wordpress.com/2015/10/pod-warning.png"><img class="alignnone size-medium wp-image-227" src="https://stackoverzone.files.wordpress.com/2015/10/pod-warning.png" /></a>

##Resolve:

1. Open any file by Xcode
2. On right side of Xcode, choose ```Show the File inspector``` > ```Target Membership```
3. Try to uncheck and check the file on any ```target```:


<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-10-03-pm.png"><img class="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-10-03-pm.png" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-23-at-9-10-03-pm.png" /></a>