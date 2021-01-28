---
layout: post
title: "iOS Unit-Test Tips & Tricks"
categories: ios
tags: swift ios unit-test tips tricks 
fullview: true
---

<img width="499" alt="Screen Shot 2020-05-06 at 17 37 05" src="https://user-images.githubusercontent.com/6329656/105624444-5a4a1380-5e54-11eb-8070-87aab63994e8.jpg">

After few years working with iOS Unit-Test I conducted some tips & tricks to help to write a reliable & effectively unit-test.

## Time-out should be 0.1 rather than 1 or 3

```swift
waitForExpectations(timeout: 0.1) { _ in }
```
```swift
wait(for: [expectation], timeout: 0.1)
```

- Apple does provide this in **XCTest** to sever all kind of Testing: Unit-Test, Integration Test, UITest
- Write a **unit-test** that depend on the timing that not a good test, will your test failed if the time-out is zero? If it failed means you're not doing DI & Mocking well.
- It not make your test-case passes faters, but if it's failed, do make failed faster. This will help your CI/CD reduce a lot of time!
- At the first glance to see a long time-out, it will make you feeling that something underneath is realy complicated.
- 0.1 or 3 does not matter, mocking matters!
- I have some more details on it [https://geek-is-stupid.github.io/2020-03-18-Unit-Test-asynchronous-timeout-0-1-seconds/](https://geek-is-stupid.github.io/2020-03-18-Unit-Test-asynchronous-timeout-0-1-seconds/)

## Not to stubbing in `setUp` method if not needed

- Redundant code, some test case don’t need to use that stub, so you're adding a un-needed implementation.
- It won’t protect your code! Imagine that in the future your teammate make another call to existing dependency, your test won't failed, because all is stubbed in setUp method.
- When the test case is removed, will you/your teammate remember to remove that stub?
- If you think it’s reusable code, then you can separate to a method, and calling that method from your test method.
- Don’t be lazy! 💪


## Always using `setUp` & `tearDown`

- In my experiences with Unit-Test on Objective-C & Swift, using **OCMock** and **Cuckoo**, we run into a problem that when the amount of test cases reach almost 10k, your Test Target gonna be "laggy", some test classes are leaked 😱
- To avoid the problem above, we do add testSetUp & tearDown to clean up the memory were allocated during testing, all mocks & sut should be set to `nil`.

```swift
var sut: YourClass!
var yourMock: MockYour!
    
override func setUp() {
    super.setUp()
    sut = YourClass()
    yourMock = YourMock()
    sut.yourMock = yourMock
}

override func tearDown() {
    sut = nil
    yourMock = nil
    super.tearDown()
}
    
func testMethod() {...}
```


## Dispose your RxSwift subscriptions

To be safe, to not making any leak, or release immediately your memory, please do dispose your subscription! 

```swift
func testFetchPartnershipProgramDetailByCode() {
    // Given:
    let expected = expectation(description: #function)

    // When:
    let subscription = sut.fetch()
        .subscribe(onNext: { _ in
            expected.fulfill()
        })

    // Then:
    waitForExpectations(timeout: 0.1) { _ in
        subscription.dispose()
    }
}
```

## Combine with BBD - Given - When - Then

- In my experiences that writing unit-tests take a lot of time, but when maintain someone unit-tests even take a lot of more time & effort
- If you and you team, set some rules to make sure the anyone can understand & maintainable 

```swift
func testMethodName() {
	// Given:
	/// Stubbing, mocking, preparing data for test
	
	// When:
	sut.methodName()
	
	// Then:
	// Assertion places, verify
}
``` 

## Handling Test Case Failure

- Settings [executionTimeAllowance](https://developer.apple.com/documentation/xctest/xctestcase/3526064-executiontimeallowance) for limit number of minutes to run a test 
- Turn [continueAfterFailure](https://developer.apple.com/documentation/xctest/xctestcase/1496260-continueafterfailure) to `false` to end execution of that method as soon as a failure occurs.

## Understanding Setup and Teardown for Test Methods

![rendered2x-1596820930](https://user-images.githubusercontent.com/6329656/105961992-cf665480-60b1-11eb-9746-e1b04ef30ece.png)

[https://developer.apple.com/documentation/xctest/xctestcase/understanding_setup_and_teardown_for_test_methods](https://developer.apple.com/documentation/xctest/xctestcase/understanding_setup_and_teardown_for_test_methods)