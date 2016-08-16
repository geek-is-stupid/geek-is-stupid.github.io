---
layout: post
title: "How to capture an image from MKMapView and MKPolyline  using Objective C?"
date: 2015-10-23 15:25:25 +0700
comments: true
categories: 
---

```objc
[MKMapSnapshotter takeSnapShotOfTargetSize:CGSizeMake(320, 150) withPolyline:(MKGeodesicPolyline *)polyline completion:^(UIImage * image, CGFloat height) {
    self.polylineImageView.image = image;
}];
```