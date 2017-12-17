---
layout: post
title: How to press on a link to open iOS application by using Swift? Universal Links in iOS 9
categories: [ios]
tags: [swift, xcode, ios, universal links, deeplinks, handoff, json]
fullview: true
---

In iOS 9, your app can register to open web links (using https or http) directly, bypassing Safari. Here is the process to add Universal Links:


![https://stackoverzone.files.wordpress.com/2015/10/universal_links.png](https://stackoverzone.files.wordpress.com/2015/10/universal_links.png)

##### A. Generate apple-site-app-association file

1. Create Server-side web credentials file: `handoff.json`

```swift
{
    "applinks": {
        "apps": [],
        "details": {
            "TEAM_ID.App-Bundle-ID": {
                "paths": ["*"]
            }
        }
    }
}
```

2. Download your certificate at [https://developer.apple.com/account/ios/certificate/certificateList.action](https://developer.apple.com/account/ios/certificate/certificateList.action), and when it is downloaded, double click on it to add into KeyChain app
3. Right click on your certificate in KeyChain to export your `Certificates.p12`

![https://stackoverzone.files.wordpress.com/2015/10/111.png](https://stackoverzone.files.wordpress.com/2015/10/111.png)

![https://stackoverzone.files.wordpress.com/2015/10/21.png](https://stackoverzone.files.wordpress.com/2015/10/21.png)

4. Open terminal to create certificates with `openssl` commands:

- Create the certificate:

```swift
openssl pkcs12 -in Certificates.p12 -clcerts -nokeys -out output_crt.pem
```

- Create the secret key:

```swift
openssl pkcs12 -in Certificates.p12 -nocerts -nodes -out output_key.pem
```

- Create the intermediate certificate:

```swift
openssl pkcs12 -in Certificates.p12 -cacerts -nokeys -out sample.ca-bundle
```

5. Sign the handoff.json file with the following command:

```swift
cat handoff.json | openssl smime -sign -inkey output_key.pem -signer output_crt.pem -certfile sample.ca-bundle -noattr -nodetach -outform DER> apple-app-site-association
```

6. Upload your `apple-site-app-association` file to the root of your web-site:

e.g. [https://example.com/apple-app-site-association.](https://example.com/apple-app-site-association.)

Make sure Content-Type of this file is: `application/pkcs7-mime`

##### B. Xcode setting


![https://stackoverzone.files.wordpress.com/2015/10/31.png](https://stackoverzone.files.wordpress.com/2015/10/31.png)

1. Select your project in the Navigator area

2. Select your Target in the Editor area

3. In the Editor area select the `Capabilities` option from the menu bar

4. Open the disclosure button to the left of the Associated Domains

5. Under Associated Domains select the `"+"` button

6. Type the entitlement in the input area:


```swift
applinks:<your-domain>
```

Example:

```swift
applinks:geek-is-stupid.github.io
```

##### C. App implement

In `AppDelegate.swift`:

```swift
func application(application: UIApplication, continueUserActivity userActivity: NSUserActivity, restorationHandler: ([AnyObject]?) -> Void) -> Bool {
    if let webpageURL = userActivity.webpageURL where userActivity.activityType == NSUserActivityTypeBrowsingWeb {
        // Your handle links code here :D
        return true
    }
    return false
}
```

##### D. Test on localhost

The Universal link **NEVER** works with links as: [http://localhost:8080](http://localhost:8080)

1. Launch the plugin at Root:

```swift
atlas-run --context-path ROOT
```

2. Change the local host link to pure link by using **NGROK** **[https://ngrok.com/](https://ngrok.com/)**:

```swift
ngrok <port_number>
```

Example for [http://localhost:8080](http://localhost:8080)

```swift
ngrok 8080
```

3. Your local host link will become: [http://xxxxxxxx.ngrok.com](http://xxxxxxxx.ngrok.com)

4. Re-configuring Associated Domains in Xcode to start testing



##### E. Drawbacks of Universal Links

1. Work only for iOS9+
2. We need to upload the file apple-app-site-association located at: https://domain.com/apple-app-site-association
3. Just support static domains, not dynamic domains, not sub-domains
4. To make a trust relationship between the app and the website, we need add the domains of the websites into Associated Domains entitlement first, and then we can specific which paths from our websites should be handled as universal links, BUT we have a lot of customer’s domains and they are different, so we can not register all of them into Associated Domains entitlement
5. Uptil Beta 4, you need to sign the app-site-association json file before upload, Apple is planning to do away with this in next Beta/GM.