---
layout: post
title: How to Capture an Image From MKMapView and MKPolyline Using Objective C?
categories: [ios]
tags: [swift, xcode, ios, uiimage, mkmapview, mkpolyline]
fullview: true
---

```swift
[MKMapSnapshotter takeSnapShotOfTargetSize:CGSizeMake(320, 150) withPolyline:(MKGeodesicPolyline *)polyline completion:^(UIImage * image, CGFloat height) {
    self.polylineImageView.image = image;
}];
```