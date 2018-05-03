---
layout: post
title: Translating 'hidden' symbol names back to their original names by using atos + dSYM?
categories: [ios]
tags: [swift, objective-c, xcode, ios, crash, address, memory, atos, dSYM, log, stack, trace, line, number]
fullview: true
---

### Problems
As we know that sometimes we got a crash with its stack trace which contains only the address on memories of some objects or some methods, without any line number of your implementation, then how we can indicate which part in your project raises the crash?

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

### Atos + dSYM

Finally we have a great tool from Apple, it called **atos** which is able to convert back the crash's stack trace with properly `dSYM` file, here are the steps.

Step 1. Make sure you have `dSYM` file. Then we can de-obfuscate your symbols:

```
xcrun dsymutil -symbol-map ~/Desktop/YourAppName.xcarchive/BCSymbolMaps ~/Desktop/YourAppName.dSYM
```

Step 2. Symbolicating crash reports with atos following this command:

```
atos -arch <Binary Architecture> -o <Path to dSYM file>/Contents/Resources/DWARF/<binary image name> -l <load address> <address to symbolicate>
```

For e.g in my stack trace I have load address is `0x10098c000` because it'sappeared in:

```
2018-03-20 04:09:03.867000000 410 30735 [2]: Load address: 0x10098c000
```

And I have the address I want to symbolicate is `0x00000001014946f4` because it'sappeared in:


```
    3   YourAppName                         0x00000001014946f4 _ZN12closure_impl15closure_wrapperEPvP10grpc_error + 956452
```

Then I need to run this command line to find my implementation:

```
atos -arch arm64 -o YourAppName.dSYM/Contents/Resources/DWARF/YourAppName -l 0x10098c000 0x00000001014946f4
```

The result looks like this:

```
-[NSMutableDictionary(NAM) setObject:forKey:defaultValue:] (in YourAppName) + 4
```

Have fun! 👨‍💻

Ref: [https://developer.apple.com/library/content/technotes/tn2151/_index.html]()
