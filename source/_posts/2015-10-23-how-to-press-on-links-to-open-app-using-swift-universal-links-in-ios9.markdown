---
layout: post
title: "How to press on links to open app using Swift? Universal Links in iOS 9"
date: 2015-10-23 23:51:20 +0700
comments: true
categories: 
---
In iOS 9, your app can register to open web links (using https or http) directly, bypassing Safari. Here is the process to add Universal Links:

<a href="https://stackoverzone.files.wordpress.com/2015/10/universal_links.png"><img align='center' src="https://stackoverzone.files.wordpress.com/2015/10/universal_links.png" alt="universal_links" /></a>

<h1>A. Generate apple-site-app-association file</h1>

**1. Create Server-side web credentials file: ```handoff.json```**

```json
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

**2. Download your certificate at <a href="https://developer.apple.com/account/ios/certificate/certificateList.action" target="_blank">here</a>, and when it is downloaded, double click on it to add into ```KeyChain app```**
 
**3. Right click on your certificate in KeyChain to export your ```Certificates.p12```**

<a href="https://stackoverzone.files.wordpress.com/2015/10/111.png"><img align='center' src="https://stackoverzone.files.wordpress.com/2015/10/111.png" alt="1" /></a>

<a href="https://stackoverzone.files.wordpress.com/2015/10/21.png"><img align='center' src="https://stackoverzone.files.wordpress.com/2015/10/21.png" alt="2" /></a>

**4. Open terminal to create certificates with ```openssl``` commands:**

* Create the certificate:

```swift
openssl pkcs12 -in Certificates.p12 -clcerts -nokeys -out output_crt.pem
```

* Create the secret key:

```swift
openssl pkcs12 -in Certificates.p12 -nocerts -nodes -out output_key.pem
```

* Create the intermediate certificate:

```swift
openssl pkcs12 -in Certificates.p12 -cacerts -nokeys -out sample.ca-bundle
```

**5. Sign the ```handoff.json``` file with the following command:**

```swift
cat handoff.json | openssl smime -sign -inkey output_key.pem -signer output_crt.pem -certfile sample.ca-bundle -noattr -nodetach -outform DER> apple-app-site-association
```

**6. Upload your ```apple-site-app-association``` file to the root of your web-site**

e.g. <a href="https://example.com/apple-app-site-association" target="_blank">https://example.com/apple-app-site-association</a>.

<strong>Make sure Content-Type of this file is:</strong>

```swift
application/pkcs7-mime
```

<h1>B. Xcode setting</h1>

<a href="https://stackoverzone.files.wordpress.com/2015/10/31.png"><img src="https://stackoverzone.files.wordpress.com/2015/10/31.png" alt="3" /></a>

**1. Select your project in the Navigator area**

**2. Select your Target in the Editor area**

**3. In the Editor area select the Capabilities option from the menu bar**

**4. Open the disclosure button to the left of the Associated Domains**

**5. Under Associated Domains select the ```"+"``` button**

**6. Type the entitlement in the input area:**

```swift
applinks:<your-domain>
```

Example:

```swift
applinks:geek-is-stupid.github.io
```

<h1>C. App implement</h1>

In ```AppDelegate.swift```

```swift
func application(application: UIApplication, continueUserActivity userActivity: NSUserActivity, restorationHandler: ([AnyObject]?) -> Void) -> Bool {
    if let webpageURL = userActivity.webpageURL where userActivity.activityType == NSUserActivityTypeBrowsingWeb {
        // Your handle links code here :D
        return true
    }
    return false
}
```

<h1>D. Test on localhost</h1>

The Universal link never works with links as: <a href="http://localhost:8080">http://localhost:8080</a>

**1. Launch the plugin at Root:**

```swift
atlas-run --context-path ROOT
```

**2. Change the local host link to pure link by using <a href="https://ngrok.com/" target="_blank">ngrok</a>:**

```swift
ngrok <port_number>
```

Example for [http://localhost:8080]

```swift
ngrok 8080
```

**3. Your local host link will become: <a href="http://xxxxxxxx.ngrok.com">http://xxxxxxxx.ngrok.com</a>**

**4. Re-configuring Associated Domains in Xcode to start testing**

<h1>Drawbacks of Universal Links</h1>

1. Work only for iOS9+
2. We need to upload the file apple-app-site-association located at: <a href="https://domain.com/apple-app-site-association">https://domain.com/apple-app-site-association</a>
3. Just support static domains, not dynamic domains, not sub-domains
4. To make a trust relationship between the app and the website, we need add the domains of the websites into  Associated Domains entitlement first, and then we can specific which paths from our websites should be handled as universal links, BUT we have a lot of customer's domains and they are different, so we can not register all of them into Associated Domains entitlement
5. Uptil Beta 4, you need to sign the ```app-site-association``` json file before upload, Apple is planning to do away with this in next Beta/GM.