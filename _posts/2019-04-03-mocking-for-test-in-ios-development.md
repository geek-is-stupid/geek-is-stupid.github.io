---
layout: post
title: "Mocking for tests in iOS development"
categories: ios
tags: swift objective-c objc ios unit test TDD mock protocol inheritance dependency injection di xcode
fullview: true
---
# 🧪 Mocking

We might have some best practices from [Way to approach Unit Tests in iOS development](https://geek-is-stupid.github.io/2019-04-20-way-to-approach-unit-tests-in-iOS-developmemt/). Now we go to the details of mocking 🥳

## What is Mocking?

**Mocking is creating objects that simulate the behavior of real objects!**

The mocking object is used to keep your dependencies out of your unit-tests.

## Why I need Mock?

- **Performance**: Unit Tests should be fast, testing with network, database, or any resources could lead to expensive about time and memory. So using mock objects, it quickly gives us an "expected" response or dataset,… and we no need to wait so long.
- **Isolation**: Unit testing with real object as dependencies for sure that will change the state of the dependencies after any tests and it could lead to flaky tests in the other tests. Mocking will give you "fresh" dependencies.

## How to Mock?

The question should be: *"How to write the code is testable?"*, then **Dependency Injection**

### Dependency Injection 💉 (DI)

![di](https://user-images.githubusercontent.com/6329656/56467821-08d64e80-644e-11e9-8260-712ff324eb1d.png)

***Dependency Injection*** is passing dependency to other objects or framework and it makes testing easier.

Let's get started with normal a implementation like below:

```swift
func increaseCounting() {
    let key = "counting-key"
    let count = UserDefaults.standard.integer(forKey: key)
    UserDefaults.standard.set(count + 1, forKey: key)
}
```

We can see that our code is interacting with an object that we're not control is `UserDefaults`!
So how do we keep our tests [fast & repeatable](https://pragprog.com/magazines/2012-01/unit-tests-are-first)?

#### Constructor Injection

We can pass the dependency object through the constuctor and save that object as a property then we can use it in the method which we want to test!

```swift
class YourClass {
    let userDefaults: UserDefaults
    
    init(userDefaults: UserDefaults) {
        self.userDefaults = userDefaults
    }
}
```

From now we need to update the method:

```swift
func increaseCounting() {
    let key = "counting-key"
    let count = userDefaults.integer(forKey: key)
    userDefaults.set(count + 1, forKey: key)
}
```
<br/>
#### Property Injection

As the same idea with **Constructor Injection**, we still extract the `UserDefaults.standard` as a property with its **standard** value by default and use it in the `increaseCounting`, and we no longer to pass it through the constructor.

```swift
class YourClass {
    var userDefaults: UserDefaults = .standard
}
```
<br/>
#### Method Injection

We directly pass `NSUserDefault` as a parameter in the method `increaseCounting`. And in tests we can pass the mock object as a parameter.

```swift
func increaseCounting(userDefaults: UserDefaults) {
    let key = "counting-key"
    let count = userDefaults.integer(forKey: key)
    userDefaults.set(count + 1, forKey: key)
}
```
<br/>
#### Extract and Override Call

In some cases you have to deal with legacy code the this is not a bad idea to test the legacy code and do minimize the changes before go with refactoring.

We'll create a getter property, and override this method with a **testable** subclass 🤔

```swift
class YourClass {
    var userDefaults: UserDefaults {
        return .standard
    }
}

class TestableYourClass: YourClass {
    override var userDefaults: UserDefaults {
        return .standard //Change this to mock object
    }
}
```

⚠️ I'm not recommend this injection due to in the future the `YourClass` can changes, and possible lead to some unexpected behaviors during the tests!

<br/>
#### Ambient Context
For some methods, we do call some **class methods** in it:
-  We could extract that class method an a getter property.
-  Or do **swizzle** 😅

<br/>
### Protocol Mocks

So we already knew how to faliciate mocking by **Dependency Injection** as above, so now we go to the details that how we mock our dependency (as `UserDefaults`) with a protocol.
Let's following the steps below:

1: Extract to protocol: We should extract all the properties and methods of the dependency which are used in the production code.

```swift
protocol UserDefaultsProtocol {
    func integer(forKey defaultName: String) -> Int
    func set(_ value: Int, forKey defaultName: String)
}
```

2: Make sure the `UserDefaults` confirms to the protocol `UserDefaultsProtocol` 

```swift
extension UserDefaults: UserDefaultsProtocol {}
```

3: Update the production code to use `UserDefaultsProtocol` instead of `UserDefaults`

We'll use **Property Injection** for this e.g.

```swift
class YourClass {
    var userDefaults: UserDefaultsProtocol = UserDefaults.standard

    func increaseCounting() {
        let key = "counting-key"
        let count = userDefaults.integer(forKey: key)
        userDefaults.set(count + 1, forKey: key)
    }
}
```

4: Create a mock in your test target:

```swift
class MockUserDefaultsProtocol: UserDefaultsProtocol {
    var integerKey: String?
    var integerValue: Int?
    
    func integer(forKey defaultName: String) -> Int {
        integerKey = defaultName
        return integerValue ?? 0
    }
	
    func set(_ value: Int, forKey defaultName: String) {
        integerKey = defaultName
        integerValue = value
    }
}
```

5: Now you're able to write your tests, and use `MockUserDefaultsProtocol` as the dependency for the tests.

```swift
import XCTest
@testable import YourTarget

class TestYourClass: XCTestCase {
    var sut: YourClass!
    var userDefaults: MockUserDefaultsProtocol!
	
    func testIncreaseCountingShouldIncreaseValueBy1() {
        //Given:
        sut = YourClass()
        userDefaults = MockUserDefaultsProtocol()
        userDefaults.integerValue = 2
        sut.userDefaults = userDefaults
		
        //When:
        sut.increaseCounting()
        
        //Then:
        XCTAssertEqual(userDefaults.integerKey, "counting-key")
        XCTAssertEqual(userDefaults.integerValue, 3)
    }
}
```

✅ **Recommended!!!**

-  Production code in dependency's object won't execute during test!
-  [Program to an interface, not an implementation
](https://www.artima.com/lejava/articles/designprinciples.html). We're decoupled from the implementation with a lighter-weight mock implementation.

<br/>
### Inheritance Mocks

This approach pretty similar with Protocol Mocks' ideas, except we'll not extract a dependency as a protocol and confirmation it.

What we do is only need to directly create a Mock object which is inherited from the dependency 🙌 `UserDefaults`

Just change the way how we create the mock:

```swift
class MockUserDefaults: UserDefaults {
    var integerKey: String?
    var integerValue: Int?

    override func integer(forKey defaultName: String) -> Int {
        integerKey = defaultName
        return integerValue ?? 0
    }

    override func set(_ value: Int, forKey defaultName: String) {
        integerKey = defaultName
        integerValue = value
    }
}
```

And the test almost the same:

```swift
import XCTest
@testable import YourTarget

class TestYourClass: XCTestCase {
    var sut: YourClass!
    var userDefaults: MockUserDefaults!
	
    func testIncreaseCountingShouldIncreaseValueBy1() {
        //Given:
        sut = YourClass()
        userDefaults = MockUserDefaults()
        userDefaults.integerValue = 2
        sut.userDefaults = userDefaults
		
        //When:
        sut.increaseCounting()
		
        //Then:
        XCTAssertEqual(userDefaults.integerKey, "counting-key")
        XCTAssertEqual(userDefaults.integerValue, 3)
    }
}
```

⚠️ **Not recommended!!!**

- The mock still has a real class underneath, it means this possible triggers the real implementation during the test as we won't expect 😬 
- This mocks might lead to **Partial Mocks 🤯** which is the **`sut`** and the **dependency** are the same. Half of real, half of fake seems it's not a good idea (expect we have to deal with legacy code).

<br/>
**Where to go from here?**

- [https://pragprog.com/magazines/2012-01/unit-tests-are-first](https://pragprog.com/magazines/2012-01/unit-tests-are-first)
- [https://www.objc.io/issues/15-testing/dependency-injection/](https://www.objc.io/issues/15-testing/dependency-injection/)
- [https://www.artima.com/lejava/articles/designprinciples.html](https://www.artima.com/lejava/articles/designprinciples.html)

#### 👍 Like & Share, thanks!!!
