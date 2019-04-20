---
layout: post
title: "Way to approach Unit Tests in iOS development"
categories: ios
tags: swift objective-c objc ios unit test TDD dependency injection di xcode BDD FIRST state return interation
fullview: true
---
# 🌡 Unit Tests
 
I and you and many developers are too familiar with this level of testing, someone might know, someone might doing it everyday. But how to do it productivity? How to do it in a saving time? then it gonna need you practice a lot with Unit Tests.

OK I will go with you on the road to become *"master"* of Unit Tests particular in iOS 😉. So some thoughts will share more about the **frame** and **best practice** with Unit Tests.

## Unit Tests are FIRST

What is **FIRST** here, it's not like TDD that do test before do implementation.  This is also **Best Practices** for Testing.

⚡️ <b><span style="font-size: 20pt">F</span>AST</b>: Tests should run quickly, saving time for CI, instantly give feedbacks and developers won't mind to running them!

🧩 <b><span style="font-size: 20pt">I</span>SOLATED</b> (or <b><span style="font-size: 20pt">I</span>NDEPENDENT</b>): Explicitly & separately do setup or teardown for every tests. When tests are interdependent, a change to one test can cause several others to fail in puzzling ways. 

🔁 <b><span style="font-size: 20pt">R</span>EPEATABLE</b>: No **flaky** tests, tests should give the same results after every run!

🤳 <b><span style="font-size: 20pt">S</span>ELF-VERIFYING</b>: A good unit test should automated, and either *fails* or *passes*. And should NOT include bunch of comments to explain what the testing is doing.

🧪 <b><span style="font-size: 20pt">T</span>IMELY</b>: Writing Tests before write your production code.

Working with Unit Tests, let keeps in mind this FIRST principle to your tests more effective.

## System Under Test - `sut`

To start a unit test, make a test has its purpose, you should determine what is the object you want to testing, it's `sut`.

- Clear purpose of a tests
- Tests are more readable

```swift
class YourClassTest: XCTestCase {
    var sut: YourClass!
}
```

> The "system under test". It is short for "whatever thing we are testing" and is always defined from the perspective of the test. When we are writing unit tests the system under test (SUT) is whatever class (a.k.a. CUT), object (a.k.a. OUT) or method(s) (a.k.a. MUT) we are testing; when we are writing customer tests, the SUT is probably the entire application (a.k.a. AUT) or at least a major subsystem of it. The parts of the application that we are not verifying in this particular test may still be involved as a depended-on component (DOC).


## Behavior Driven Development - BDD

I love to add BDD to my Unit Tests because it make my test more clear and **SELF-VERIFYING**. 

A tests should separate 3 parts:

- **given** something
- **when** the thing happens
- **then** getting expectations.

```swift
class YourClassTest: XCTestCase {
    var sut: YourClass!
	
    func testYourMethodShouldDoSomething() {
        //Given:
		
        //When:
        sut.yourMethod()
		
        //Then:
    }
}
```

Alternative naming systems as **triple A (AAA)** are **Arrange**-**Act**-**Assert** and **Assemble**-**Activate**-**Assert**.

## Type of Unit Tests

### Return Value Test
```
func testYourMethod() {
    //When: Setup object
    //When: Call method that return a value
    //Then: Compare against expected value
}
```
![return-value-test](https://user-images.githubusercontent.com/6329656/56451556-769f4f00-6356-11e9-9a57-a18198de9bca.png)

### State Test
```
func testYourMethod() {
    //When: Setup object
    //When: Call method, query object in some way
    //Then: Verify result
}
```
![state-test](https://user-images.githubusercontent.com/6329656/56451842-eeba4480-6357-11e9-9414-6203af055223.png)


### Interaction Test
```
func testYourMethod() {
    //When: Setup object
    //When: Call method, SUT calls dependencies
    //Then: Verify result
}
```
![interaction-test](https://user-images.githubusercontent.com/6329656/56452163-b0258980-6359-11e9-964c-18152549e6c4.png)

➡️**[What next]** To work with **SUT** has dependencies, you might read [Mocking for tests in iOS development](https://geek-is-stupid.github.io/2019-04-03-mocking-for-test-in-ios-development)

**Where to go from here?**

- [https://geek-is-stupid.github.io/2019-04-03-mocking-for-test-in-ios-development/](https://geek-is-stupid.github.io/2019-04-03-mocking-for-test-in-ios-development/)
- [https://pragprog.com/magazines/2012-01/unit-tests-are-first](https://pragprog.com/magazines/2012-01/unit-tests-are-first)
- [http://xunitpatterns.com/SUT.html](http://xunitpatterns.com/SUT.html)

#### 👍 Like & Share, thanks!!!
