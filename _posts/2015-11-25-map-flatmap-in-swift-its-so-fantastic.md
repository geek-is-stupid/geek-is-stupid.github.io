---
layout: post
title: map(), flatMap() in Swift, Its So Fantastic 
categories: [ios]
tags: [swift, xcode, ios, map, flatMap, array, collection]
fullview: true
---

##### Array

I have a class:

```swift
class Person {
    var name: String?
    var age = 0

    init(name: String?, age: Int) {
        self.name = name
        self.age = age
    }
}
```

and I have a lot of people and all of them I stored in an array:

```swift
let A = Person(name: "A", age: 20)
let B = Person(name: "B", age: 25)
let C = Person(name: "C", age: 27)
let D = Person(name: "D", age: 32)
let people = [A, B, C, D]
```

###### Question: How to join all names of people in the array?

Normal way:

```swift
var names = ""
for person in people {
  names += person.name! + ","
}
print(names)
```

###### Shorter way:

```swift
let arrayNames = people.map { person in
  return person.name
}
print(arrayNames.joinWithSeparator(","))
```

or shorter :]

```swift
let arrayNames = people.map { $0.name }
print(arrayNames.joinWithSeparator(","))
```

###### So how about if I using .flatMap???

Now I need to change a little bit here:

```swift
A.name = nil
C.name = nil
```

Using .map:

```swift
let arrayNames = people.map { person in
    guard let name = person.name else { return nil }
    return person.name
}
```

Output:

> [nil, "B", nil, "D"]

###### Using .flatMap:

let arrayNames = people.flatMap { person in
    guard let name = person.name else { return nil }
    return name
}

Output:

> ["B", "D"]

###### -> The nil values won’t be added into returned array

Difference:

| `.map` | `.flatMap` |
|---|----|
| Add nil values into returned array	 | Doesn’t add nil values into returned array |

