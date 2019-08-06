---
layout: post
title: "How to reduce loading time by using staticlib with CocoaPods"
categories: ios
tags: swift objective-c objc ios dynamic loading time staticlib cocoapods DYLD_PRINT_STATISTICS 
fullview: true
---
In many of us, we're iOS developers who are desire to reduce your app's loading time.

Many and many articles were written to gave you some tricks & tips to improve it, you can check it here [Slow App Startup Times](https://useyourloaf.com/blog/slow-app-startup-times/), 

Someday I go through this WWDC video [Optimizing App Startup Time](https://developer.apple.com/videos/play/wwdc2016/406/) and I try to apply to use `MACH-O` with `staticlib` instead of dynamic frameworks for all my 3rd-party.

My project is using CocoaPods to manages all of libraries. And luckily it supports us to use `staticlib` for any frameworks 👍 [http://blog.cocoapods.org/CocoaPods-1.5.0](http://blog.cocoapods.org/CocoaPods-1.5.0/)


# Pre-main Time - `DYLD_PRINT_STATISTICS`

To start improve your app, we might to have some metrics to compare before and after the improvements.

Apple has added some logs [Logging Dynamic Loader Events](https://developer.apple.com/library/archive/documentation/DeveloperTools/Conceptual/DynamicLibraries/100-Articles/LoggingDynamicLoaderEvents.html) to record the timing when user starts launch your app!

We can use one of them: `DYLD_PRINT_STATISTICS`
> Logs statistical information on an application’s launch process, such as how many images were loaded, when the application finishes launching.

Let's add it into your app's scheme:

<img width="880" alt="Screen Shot 2019-08-06 at 1 29 16 PM" src="https://user-images.githubusercontent.com/6329656/62516115-55ebfe00-b84e-11e9-9f82-6f618dcb0e61.png">

The output afer run the project:

```
Total pre-main time: 2.0 seconds (100.0%)
         dylib loading time: 1.4 seconds (72.8%)
        rebase/binding time:  84.22 milliseconds (4.1%)
            ObjC setup time: 172.29 milliseconds (8.5%)
           initializer time: 291.42 milliseconds (14.4%)
           slowest intializers :
             libSystem.B.dylib :  14.72 milliseconds (0.7%)
    libMainThreadChecker.dylib :  53.90 milliseconds (2.6%)
                     Braintree :  47.92 milliseconds (2.3%)
                  FBSDKCoreKit :  46.91 milliseconds (2.3%)
             GTMSessionFetcher : 124.46 milliseconds (6.1%)
                   YourAppName :  46.73 milliseconds (2.3%)
```

The `Total pre-main time` shows how slow app has to take before system executes `main()`
There are some explaination for the other metrics you can check it here [Slow App Startup Times](https://useyourloaf.com/blog/slow-app-startup-times/)


# CocoaPods with `staticlib`

After watch the WWDC video [Optimizing App Startup Time](https://developer.apple.com/videos/play/wwdc2016/406/) there is a trick that I'm using is to use `staticlib` for all libraries from Pods target.

<img width="997" alt="Screen Shot 2019-08-06 at 1 44 13 PM" src="https://user-images.githubusercontent.com/6329656/62516904-6ef5ae80-b850-11e9-9d83-113cf8b3a143.png">

And the result was surprising me, that the `pre-main time` was reduced almost **1 second**, this is a lot for my project.

```
Total pre-main time: 993.25 milliseconds (100.0%)
         dylib loading time: 580.23 milliseconds (58.4%)
        rebase/binding time:  93.87 milliseconds (9.4%)
            ObjC setup time:  89.93 milliseconds (9.0%)
           initializer time: 229.08 milliseconds (23.0%)
           slowest intializers :
             libSystem.B.dylib :  12.26 milliseconds (1.2%)
    libMainThreadChecker.dylib :  50.70 milliseconds (5.1%)
                     Braintree :  31.65 milliseconds (3.1%)
                         Realm :  31.76 milliseconds (3.1%)
                   YourAppName : 192.47 milliseconds (19.3%)
```


To here you are almost done the improvements, but there is a problem when you do **Archive**

<img width="251" alt="Screen Shot 2019-08-06 at 1 51 52 PM" src="https://user-images.githubusercontent.com/6329656/62517327-65b91180-b851-11e9-9aeb-cb1bc4d3de50.png">


# Found an unexpected Mach-O header code: 0x72613c21

When **Archived** and you'll get this message `Found an unexpected Mach-O header code: 0x72613c21` when try to **Validate** the build from **Organizer**

<img width="734" alt="Screen Shot 2019-08-06 at 2 02 48 PM" src="https://user-images.githubusercontent.com/6329656/62518077-e4627e80-b852-11e9-9d69-3861d22e8cc0.png">

I go to check the logs in `IDEDistribution.standard.log`:

```
2019-08-02 09:43:41 +0000  [OPTIONAL] Didn't find archived user entitlements for <DVTFilePath:0x7f9702d2f5d0:'/Users/username/Library/Developer/Xcode/Archives/2019-08-02/YourAppName 8-2-19, 4.43 PM.xcarchive/Products/Applications/YourAppName.app/Frameworks/Cartography.framework'>: Error Domain=NSCocoaErrorDomain Code=4 "Item at "/Users/username/Library/Developer/Xcode/Archives/2019-08-02/YourAppName 8-2-19, 4.43 PM.xcarchive/Products/Applications/YourAppName.app/Frameworks/Cartography.framework" did not contain a "archived-expanded-entitlements.xcent" resource." UserInfo={NSLocalizedDescription=Item at "/Users/username/Library/Developer/Xcode/Archives/2019-08-02/YourAppName 8-2-19, 4.43 PM.xcarchive/Products/Applications/YourAppName.app/Frameworks/Cartography.framework" did not contain a "archived-expanded-entitlements.xcent" resource.}
2019-08-06 04:04:59 +0000 [MT] Canceled distribution assistant
```

So I guess this due to CocoaPods is treated all those converted `staticlib` libraries like dynamic libraries.


# [CP] Embed Pods Frameworks

`"${PODS_ROOT}/Target Support Files/Pods-ShopBack/Pods-ShopBack-frameworks.sh"`

So I digged into this `Script Phase`, and find out that after you run `pod install`, it will automatically added that script to your app's `Build Phases`.

Let's check what is inside the script:

```
install_framework "${BUILT_PRODUCTS_DIR}/Cartography/Cartography.framework"
```

It will trigger `install_framework` for every targets when you do build your project.

So my idea is to prevent this install for all converted libraries from `Podfile`.


# Podfile

I wrote this [gist](https://gist.github.com/levantAJ/b8eef8121573085d130fde46442e9658), you can download it and use it from `post_install` within your **Podfile**:

```
post_install do |installer|  
    # Improve Pre-main Time
    require File.expand_path(File.dirname(__FILE__) + "/improve-pre-main-time-loading.rb")
    supported_staticlib_pods = ['Cartography']
    improve_pre_main_time_loading(installer, supported_staticlib_pods, "YourAppName")
end
```

With the example above, I sepecific which library will use as `staticlib`:
`supported_staticlib_pods = ['Cartography']` means I only converted `Cartography` to `staticlib`, you can add more libs here

After this step, you can do `pod install` again and do **Archive** ✅


# Results

- iPhone6s
- iOS 12.0.1
- There are 65 dependencies from the Podfile and 93 total pods installed.
- Converted 50 dependencies to use `staticlib`

<img width="758" alt="Screen Shot 2019-08-06 at 2 25 40 PM" src="https://user-images.githubusercontent.com/6329656/62519789-7c159c00-b856-11e9-9210-18ef31a01663.png">


