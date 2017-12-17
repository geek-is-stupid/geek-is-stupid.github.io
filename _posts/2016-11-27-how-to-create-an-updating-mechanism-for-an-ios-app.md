---
layout: post
title: How to create an updating mechanism for an iOS application by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, updating, mechanism, version]
fullview: true
---

The idea is we checking the current version on AppStore and the version on your app, if they are not equal then make an updating!!!

But hold on, what happen if AppStore’s review team test your app, and they will use the lastest version, and it’s version absolutely difference with your app’s version on AppStore?

Solution is: `LastReleasedDate`

```swift
extension UIApplication {
	class var needsUpdate: (isNeeded: Bool, version: String?) {
        do {
            let url: URL = URL(string: "http://itunes.apple.com/lookup?bundleId=\(appBundle)")!
            let data: Data = try Data(contentsOf: url)
            guard let json: [String: Any] = try JSONSerialization.jsonObject(with: data, options: [.allowFragments]) as? [String: Any] else { return (false, nil) }
            if let result: [String: Any] = (json["results"] as? [Any])?.first as? [String: Any],
                let appStoreVersion: String = result["version"] as? String,
                let releaseDate: Date = (result["releaseDate"] as? String)?.date(dateFormat: "yyyy-MM-dd'T'hh:mm:ssZ") {
                let flag: Bool
                if let lastReleasedTimeString: String = UserDefaults.standard.string(forKey: "com.levantAJ.geek-is-stupid.last-released-date") {
                    let lastReleasedTime: Double = Double(lastReleasedTimeString)!
                    flag = version.short != appStoreVersion && lastReleasedTime < releaseDate.timeIntervalSince1970
                    if flag {
                        UserDefaults.standard.set(releaseDate.timeIntervalSince1970, forKey: "com.levantAJ.geek-is-stupid.last-released-date")
                    }
                } else {
                    UserDefaults.standard.set(releaseDate.timeIntervalSince1970, forKey: "com.levantAJ.geek-is-stupid.last-released-date")
                    flag = false
                }
                return (flag, appStoreVersion)
            }
        }
        catch let error {
            debugPrint(error.localizedDescription)
            return (false, nil)
        }
        return (false, nil)
    }
}
```

This variable will give you an flag to indicated that should be update your application, and current app’s version on AppStore:

```swift
let (isNeededToUpdate, appVersionOnAppStore) = UIApplication.needsUpdate
debugPrint(isNeededToUpdate)
debugPrint(appVersionOnAppStore)
```