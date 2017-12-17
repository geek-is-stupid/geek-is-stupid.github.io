---
layout: post
title: How to handle a response with `success` and `error` cases in only one completion block handler by using Swift?
categories: [ios]
tags: [swift, xcode, ios, response, enum]
fullview: true
---

Nowaday, there are a lot of solution to handle response which has multiple states.


```swift
func getUserDetail(completion: (User?, NSError?) -> Void) {...}
```

or 

```swift
func getUserDetail(success: User -> Void, failure: NSError -> Void) {...}
```

I hate this, when ever I call that method I have to check error != nil

```swift
getUserDetail { user, error in
  if let error = error {
      //Do when failed
  } else if let user = user {
      //Do when success
  } else {
      //What should I do here
  }
}
```

or kind of prolix :) but simple right

```swift
getUserDetail({ user in
  //Do when success
}, failure: { error in
  //Do when failed
})
```

OK now I will do some think more complicated, but I think good for me (not recomended for everybody)

First, create an enum Response:

```swift
enum Response<Value> {
    case success(Value)
    case failure(NSError)
}
```

Time to for `enum`:

```swift
func getUserDetail(completion: Response<User> -> Void) {
  // if success
  completion(.success(user))
  //..
  // if failed
  completion(.failure(error))
}
```

Call your method:

```swift
getUserDetail { response in
  switch response {
  case .Success(let user):
      //Do when success
  case .Failure(let error):
      //Do when failed
  }
}
```

Let's swifty! 🛀🛀🛀

