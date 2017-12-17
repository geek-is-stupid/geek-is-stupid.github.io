---
layout: post
title: How can I use 'guard', 'where' statements in Swift2.0? Is it powerful?
categories: [ios]
tags: [swift, xcode, ios, guard, where, let, var]
fullview: true
---

###### Good way to optimize your code :P

I think `guard` statement is useful for make a precondition in your method when you will do some stuffs, it’s great to unwrapping optional values.

Getting started with a simple example:

```swift
func doSomething() {
  if let person = person {
      if person.age >= 32 {
          // Do a lots of things here
      }
  }
}
```

Update with `where`:

```swift
func doSomething() {
  if let person = person where person.age >= 32 {
      // Do a lots of things here
  }
}
```

But if you have to implement a lot of things and you need to use a lots of nested conditioned statements, it makes your code not flat anymore and very hard to debugging and not easy to read, so the way that I wanna use for this case is `guard`:

```swift
func doSomething() {
  guard let person = person where person.age >= 32 else {
      return
  }
  //...
}
```

By using Swift4:

```swift
func doSomething() {
  guard let person = person, person.age >= 32 else {
      return
  }
  //...
}
```

##### Bonus:

What happen if you have a lots of conditions, something like this:

```swift 
if let person = person, job = job, company = company where person.age > 32 && job == 'iOS' {
  // Do a lots of things here
}
```

But when you debugging your app and it doesn’t run into the condition, how do you know which part made the condition failed???

Tips here: **Make one line with one condition and then you will easy to detect which condition is wrong.**

```swift
if let person = person,
  job = job,
  company = company
  where person.age > 32
  && job == 'iOS' {
  // Do a lots of things here
}
```

By using Swift4

```swift
if let person = person,
  job = job,
  company = company,
  person.age > 32,
  job == 'iOS' {
  // Do a lots of things here
}
```

