---
layout: post
title: How to catch the crash's stack trace?
categories: [ios]
tags: [objective-c, xcode, ios, crash, address, log, stack, trace, line, number]
fullview: true
---

### Problems
How to log down the crash trace for investigation?

```
2018-03-20 04:09:03.858000000 410 30735 [2]: Collection <__NSArrayM: 0x13d5493c0> was mutated while being enumerated.
2018-03-20 04:09:03.864000000 410 30735 [2]: Stack Trace: (
	0   CoreFoundation                      0x000000018635b17c <redacted> + 148
	1   libobjc.A.dylib                     0x00000001855a4528 objc_exception_throw + 56
	2   CoreFoundation                      0x000000018635a7f0 <redacted> + 0
	3   YourAppName                         0x00000001014946f4 _ZN12closure_impl15closure_wrapperEPvP10grpc_error + 956452
	4   YourAppName                         0x000000010149419c _ZN12closure_impl15closure_wrapperEPvP10grpc_error + 955084
	5   YourAppName                         0x00000001014ac994 _ZN12closure_impl15closure_wrapperEPvP10grpc_error + 1055428
	6   YourAppName                         0x00000001014734b4 _ZN12closure_impl15closure_wrapperEPvP10grpc_error + 820708
	7   YourAppName                         0x000000010142f928 _ZN12closure_impl15closure_wrapperEPvP10grpc_error + 543320
	8   libdispatch.dylib                   0x0000000185cdaa54 <redacted> + 24
	9   libdispatch.dylib                   0x0000000185cdaa14 <redacted> + 16
	10  libdispatch.dylib                   0x0000000185ce7ea4 <redacted> + 1032
	11  libdispatch.dylib                   0x0000000185ce7a38 <redacted> + 120
	12  libsystem_pthread.dylib             0x0000000185f8306c _pthread_wqthread + 1268
	13  libsystem_pthread.dylib             0x0000000185f82b6c start_wqthread + 4
)
2018-03-20 04:09:03.866000000 410 30735 [2]: Slide: 0x98c000
2018-03-20 04:09:03.867000000 410 30735 [2]: Load address: 0x10098c000
```

### NSSetUncaughtExceptionHandler

```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions { 
	NSSetUncaughtExceptionHandler(&uncaughtExceptionHandler);
}

void uncaughtExceptionHandler(NSException *exception) {
    NSLog(@"CRASH: %@", exception);
    NSLog(@"Stack Trace: %@", [exception callStackSymbols]);
}
```
