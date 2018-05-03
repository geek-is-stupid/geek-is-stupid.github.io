---
layout: post
title: How to mock a completion block in OCMock?
categories: [ios]
tags: [swift, xcode, ios, test, C/C++, functions, pointer]
fullview: true
---

### Problems
My dependency has a completion block, and inside the implementation of that block I do some logics, so how my test needs to make sure it calls all the logics?

```
- (void)doSomething {
	[self.myService requestWithCompletion:^void(NSError *error) {
		[self.myEvent handleError:error];
	}];
}
```

### NSInvocation + OCMStub

```
- (void)testDoSomething {
	[OCMStub([self.myServiceMock requestWithCompletion:[OCMArg any]]) andDo:^(NSInvocation *invocation) {
        void (^handler)(NSError *error);
        [invocation getArgument:&handler atIndex:2];
        handler(errorMock);
    }];
    
    //When:
    [self.sut doSomething];
    
    //Then:
    OCMVerify([self.myEventMock handleError:[OCMArg any]]);
}
```


**How to indicate the `index` is `2` for `[invocation getArgument:&handler atIndex:2];`?**

We have `0` for the target (self), `1` for the selector (doSomething), `2` is the index of block in your selector.

If your selector looks like this `- (void)arg1:(int)arg1 block:(void(^)())block` then the `index` is `3`.

