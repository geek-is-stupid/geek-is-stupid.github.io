---
layout: post
title: How to test C/C++ functions by using Objective-C?
categories: [ios]
tags: [swift, xcode, ios, test, C/C++, functions, pointer]
fullview: true
---

### Problem
I guess that you already saw some C/C++ functions like these get called in Objective-C :]

```
int status;
change_network_status(status)
```

or even GCD:

```
dispatch_async(queue, ^{
	//...
});
```

The problem here is how you can make sure that the C/C++ functions or the block of `dispatch_async ` get called? How we can verify it by using `XCTestCase`?

### Objective-C

What you need to do is **passing a pointer to a function**!

Let's get started with a simple class call `Service`, and inside this class I need to call a C/C++ function to start my services via a library.

My C/C++ function need an input is the state of the app (active, inactive, background), and it will return a flag which indicate whether start my services successfully or not.

```
int start_platform_service(int state);
```

In the `Service.m` file I have something like this:

```objc
@interface Service: NSObject
@end

@implementation Service 

- (void)startService {
	int state = 1; 
	int result = start_platform_service(state);
	if (result == 1) {
		NSLog("Success");
		//...
	} else {
		NSLog("Failure");
		//...
	}
}

@end
```

And the `ServiceTest.m` file will be:

```objc
@interface ServiceTest: XCTestCase
@end

@implementation ServiceTest 

- (void)testStartService {
	//... how to verify this method gets trigger `start_platform_service` here?
}

@end
```

The question now is how we can verify that the `start_platform_service` get called when we run `startService`?

We need to **passing a pointer to method `start_platform_service `** to treat it like a dependency and then we easily to mock it.

Back to my `Service.m`, I will declare a property as a pointer and named it as: `startPlatformService`

```objc
@interface Service: NSObject
@property (nonatomic, assign) int (*startPlatformService)(int state);
@end
```

Then we need to update our implementation to use this pointer:

- In initialise method we need to assign which C/C++ function we want to point to our property.
- Then we call our pointer property instead of C/C++ function.

We'll have something like this:

```objc
@implementation Service 

- (instancetype)init {
    self = [super init];
    if (self) {
        _startPlatformService = &start_platform_service;
    }
    return self;
}

- (void)startService {
	int state = 1; 
	int result = self.startPlatformService(state);
	if (result == 1) {
		NSLog("Success");
		//...
	} else {
		NSLog("Failure");
		//...
	}
}

@end

```

### Testing:

My aming to mock the C/C++ function, now let's create a mock method for it, and point our pointer property to that mocking method.

```objc
@implementation ServiceTest 

int start_platform_service_mock(int state) {
	//...
	return 0;
}

@end 
```

To make sure our mock method get called, we need to create static variable to verify this method:

```objc
@implementation ServiceTest 

static BOOL start_platform_service_mock_get_called = NO;
int start_platform_service_mock(int state) {
	start_platform_service_mock_get_called = YES;
	return 0;
}

@end
```

Now we're able to test our method `startService`:

```objc
@interface ServiceTest: XCTestCase
@end

@implementation ServiceTest 

static int start_platform_service_result_mock = -1;
int start_platform_service_mock(int state) {
	return start_platform_service_result_mock;
}

- (void)testStartService {
	//Given:
	start_platform_service_mock_get_called = NO;
	Service *service = [[Service alloc] init];
	service.startPlatformService = &start_platform_service_mock;
	
	//When:
	[service startService];
	
	//Then:
	XCTAssertTrue(start_platform_service_mock_get_called);
}

@end

```

Hope it will help you guys a bit in your `objc` developmemt life!
