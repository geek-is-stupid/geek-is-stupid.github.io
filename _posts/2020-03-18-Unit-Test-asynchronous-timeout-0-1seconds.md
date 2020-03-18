---
layout: post
title: "Unit Test asynchronous time-out - 0.1 seconds"
categories: ios
tags: swift objective-c objc ios Unit test asynchronous time-out expectation
fullview: true
---
## 0.1 over 1 or 3

### UNIT, not Integration
Just remind that we’re writing a UNIT test, which means test within a method, a small piece of code, we’re not trying to make an integration test to test multiple levels of code so it should not take that long.

If your test method is executed more than 0.1 seconds then your implementation has harmed the performance 😛

### [F for Fast](https://pragprog.com/magazines/2012-01/unit-tests-are-first)
The time-out is not making your test run faster, but it **does make your test fail faster**.

Imaging you setup the time-out for 3 seconds, and for some reason, it’s failed in eventually, so the CI needs to wait for 3 seconds, how about if we have the same for 10 test cases, then the CI needs to wait for 30 seconds for the failure. It takes a lot of resources, it blocks other jobs, it make your build takes longer,…

Why don’t we think to make the test to raise the result instantly to save a lot of time for us instead of increasing the time-out?

### [R for Repeatable](https://pragprog.com/magazines/2012-01/unit-tests-are-first)
People start telling me *“If I make it as 3 seconds then the test more stable”*, or *“if the time-out is longer then it will make the percentage of successful is higher, cover 90%”*. 

Then why don’t we make the time-out as 10 or 30 seconds or forever, then the test never fails 🤔

So as your thoughts, it means the time-out decided a test will be succeeded or failed? No, it shouldn't be, a time-out is a way that allow us to use to execute an asynchronous test, it should not decide the result of a unit-test. 

### [Mocking](https://geek-is-stupid.github.io/2019-04-03-mocking-for-test-in-ios-development/)
I have got some ideas like: *“My method integrated with a lot of dependencies so I am afraid of 0.1 is not enough to execute”*.

We're giving wrong way to mock or the dependencies are not fully mocking, then we adding a long time-out to justify that due to a lot of dependencies.

Why don’t we find the root cause to fix it instead of keeps increasing the time-out to fixing a flaky test as last resort. 

Good mocking is **not a real network**, is **not a real database query**, **is not a real queue**, **is not a real object**, …, it just has the same interface of the dependencies but underneath does not process any heavy tasks so it should not take that long to execute. 

A good test is a test that has a good mocking which is introduced by a good injection [DI - Dependency Injection](https://www.objc.io/issues/15-testing/dependency-injection/)

### Machine
Some one also telling *“It due to the CI machine, 0.1 is too short to let the machine finish the test method”*. 

🤔 0.1 or 3 are short for us, short for human, but I don't think it's a short period for a machine nowaday, to execute a “good” unit-test I don't think it takes up to 0.1! 

### Flaky Test
None of us want a flaky test, because it takes a lot of effort to maintain, the short time-out (0.1) can help us to indicate which test can be a flaky test and we can apply DI to mock as much as possible to avoid it! 


### The chicken or the egg?
If someone asking you why it should be 0.1 not 3. Then you ask them back why it's 3 not 0.1 😛

The question be like *"Which came first, the chicken or the egg?"* 😉

---------------

#### 0.1 or 3 does not matter, mocking matters


