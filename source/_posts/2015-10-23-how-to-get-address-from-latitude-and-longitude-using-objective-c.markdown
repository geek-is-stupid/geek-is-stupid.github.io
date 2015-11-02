---
layout: post
title: "How to get address from latitude and longitude using Objective C?"
date: 2015-10-23 15:45:49 +0700
comments: true
categories: 
---
```objc
-(void)getAddressFromLatLon:(CLLocation*) location theAnnotation:(TrafficAnnotation*) annotation{
    CLGeocoder *geocoder = [[CLGeocoder alloc] init];
    [geocoder reverseGeocodeLocation:location completionHandler:^(NSArray *placemarks, NSError *error){
         if(placemarks && placemarks.count > 0){
             CLPlacemark *placemark= [placemarks objectAtIndex:0];
             // address defined in .h file
             address = [NSString stringWithFormat:@"%@ , %@ , %@, %@", [placemark thoroughfare], [placemark locality], [placemark administrativeArea], [placemark country]];
             NSLog(@"New Address Is:%@", address);
         }
     }];
}
```