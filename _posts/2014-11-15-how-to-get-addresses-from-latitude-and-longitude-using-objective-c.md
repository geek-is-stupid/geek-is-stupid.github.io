---
layout: post
title: How to Get Address From Latitude and Longitude Using Objective-C?
categories: [ios]
tags: [objc, objective-c, xcode, ios, address, latitude, longitude]
fullview: true
---

```swift
- (void)getAddressFromLatLon:(CLLocation*)location theAnnotation:(TrafficAnnotation*)annotation {
    CLGeocoder *geocoder = [[CLGeocoder alloc] init];
    [geocoder reverseGeocodeLocation:location completionHandler:^(NSArray *placemarks, NSError *error){
         if (placemarks && placemarks.count > 0) {
             CLPlacemark *placemark= [placemarks objectAtIndex:0];
             NSString *address = [NSString stringWithFormat:@"%@ , %@ , %@, %@", [placemark thoroughfare], [placemark locality], [placemark administrativeArea], [placemark country]];
             NSLog(@"New Address Is:%@", address);
         }
     }];
}
```
