---
layout: post
title: "How to manage the errors in your iOS app?"
date: 2015-10-24 00:06:32 +0700
comments: true
categories: 
---
I used to got huge mistaken with handle a lot of errors, and almost the errors from after you call REST API or networks, I just ```print(error)```

###What should I do now?

Keep calm and create ```Error``` class to handle all of them.

## I will show you guys the simple way to handle API errors, something like this:

Step 1. Create enum ```APIError```:

```swift
enum APIError: Int, ErrorType, CustomStringConvertible {
    case Unauthorized = 401
    case Forbidden = 403
    case PageNotFound = 404
    case TimeOut = 408
    case InternalServerError = 500
    case Unknown
    
    var description: String {
        switch self {
        case .PageNotFound:
            return NSLocalizedString("Page Not Found", comment: "")
        case .Unauthorized:
            return NSLocalizedString("Authorization Required", comment: "")
        case .Forbidden:
            return NSLocalizedString("Forbidden", comment: "")
        case .TimeOut:
            return NSLocalizedString("Time Out", comment: "")
        case .InternalServerError:
            return NSLocalizedString("Internal Server Error", comment: "")
        }
    }
    
    func foundationError() -> NSError {
        return NSError(domain: errorDomain, code: self.rawValue, userInfo: [
            NSLocalizedDescriptionKey: description
            ])
    }
}
```

Step 2. Handle when send a request to REST API

```swift
class PersonAPIService {
	class func retrieveInformation(success: (Person) -> Void, failure: (NSError?) -> Void) {
	// Your request here
	// Success
		success(person)
	// Failure with 'headerResponse: NSHTTPURLResponse'
		failure(APIError(rawValue: headerResponse.statusCode))
	}
}
```

Step 3. Now handle in your view controller:

```swift
class ViewController: UIViewController {
	override func viewDidLoad() {
		super.viewDidLoad()
		PersonAPIService.retrieveInformation(success: { (person) in
			// Fetch data
		}, failure: { (error) in
			print(error.localizedDescription)
		})
	}
}
```


