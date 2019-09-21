---
layout: post
title: "Open deeplink in iOS UI Test"
categories: ios
tags: swift objective-c objc ios ui test open deeplink url universal link
fullview: true
---

UI testing is the best way to verify how your iOS application working in Production environments and how it looks like when integrating with other services!

There is an important feature that every e-commerce mobile applications need to included to do campaign marketing, it's a deep link!

So today we will add some UI tests to make sure the feature work properly:

How to open your iOS app via an URL while you're in UI testing on a simulator?**
All you need is [XCUIApplication](https://developer.apple.com/documentation/xctest/xcuiapplication), it allows you to launch an application based on its bundle identifier.

Let's think about which iOS applications that we have on a simulator, all of them are from Apple, and here are list of their bundle id:

| App name | Bundle ID |
|----------|-----------|
| Activity | com.apple.Fitness |
| App Store | com.apple.AppStore |
| Books | com.apple.iBooks |
| Calculator | com.apple.calculator |
| Calendar | com.apple.mobilecal |
| Camera | com.apple.camera |
| Clips | com.apple.clips |
| Clock | com.apple.mobiletimer |
| Compass | com.apple.compass |
| Contacts | com.apple.MobileAddressBook |
| FaceTime | com.apple.facetime |
| Files | com.apple.DocumentsApp |
| Find Friends| com.apple.mobileme.fmf1 |
| Find iPhone | com.apple.mobileme.fmip1 |
| GarageBand | com.apple.mobilegarageband |
| Health | com.apple.Health |
| Home | com.apple.Home |
| iCloud Drive| com.apple.iCloudDriveApp |
| iMovie | com.apple.iMovie |
| iTunes Store| com.apple.MobileStore |
| iTunes U | com.apple.itunesu |
| Mail | com.apple.mobilemail |
| Maps | com.apple.Maps |
| Messages | com.apple.MobileSMS |
| Measure | com.apple.measure |
| Music | com.apple.Music |
| News | com.apple.news |
| Notes | com.apple.mobilenotes |
| Phone | com.apple.mobilephone | 
| Photos | com.apple.mobileslideshow |
| Photo Booth | com.apple.Photo-Booth |
| Podcasts | com.apple.podcasts |
| Reminders | com.apple.reminders |
| Safari | com.apple.mobilesafari |
| Settings | com.apple.Preferences |
| Shortcuts | is.workflow.my.app |
| Stocks | com.apple.stocks |
| Tips | com.apple.tips |
| TV | com.apple.tv |
| Videos | com.apple.videos |
| Voice Memos | com.apple.VoiceMemos |
| Wallet | com.apple.Passbook |
| Watch | com.apple.Bridge  |
| Weather | com.apple.weather |

From those bundle id, and there is default application on the simulator that allows us to open an URL, it's **Contacts** with bundle id is: `com.apple.MobileAddressBook`

Let's started:

```swift
extension XCTestCase {
  func openDeepLink(urlString: String, assertion: (XCUIApplication) -> Void) {
    //1. Create Contacts via its bundle id
    let contactsApp = XCUIApplication(bundleIdentifier: "com.apple.MobileAddressBook")

    // 2. Launch it in the simulator
    contactsApp.launch()

    // 3. Get default contact with named: `Kate Bell`
    let kateBellCell = contactsApp.cells.staticTexts["Kate Bell"]

    // 4. Open the contact
    if kateBellCell.waitForExistence(timeout: ShortTimeout) {
      kateBellCell.tap()
    }

    // 5. Check wheather the URL is added or not
    let urlCell = contactsApp.tables.cells.element(contains: urlString)
    if urlCell.waitForExistence(timeout: ShortTimeout) {
      // 6. If it's added, open the URL instead of add to the contact
      urlCell.tap() 
    } else {
      // 7. If the URL is not added, try to edit the contact
      contactsApp.buttons["Edit"].tap()

      // 8. Add the URL to contact
      let editTableView = contactsApp.tables.firstMatch
      XCTAssert(editTableView.waitForExistence(timeout: 3), "No 'Edit' table view found!")
      editTableView.swipeUp()
      let addURLCell = editTableView.cells.element(contains: "add url")
      XCTAssert(addURLCell.waitForExistence(timeout: 3), "No 'add url' cell found!")
      addURLCell.tap()
      let urlTextField = editTableView.cells.textFields["URL"]
      XCTAssert(urlTextField.waitForExistence(timeout: 3), "No 'URL' text field found!")
      urlTextField.tap()
      urlTextField.typeText(urlString)
      editTableView.swipeDown()

      // 9. Save the editing
      contactsApp.buttons["Done"].tap()

      // 10. Try to open the URL by press on a cell which is added the URL 
      let urlCell = contactsApp.tables.cells.element(contains: urlString)
      XCTAssert(urlCell.waitForExistence(timeout: 3), "No '\(urlString)' cell found!")
      urlCell.tap()
    }

    // 11. Handle an alert that iOS presented to ask user wheather allows to Open our app or not
    let alertHandler = addUIInterruptionMonitor(withDescription: "\"Contacts\" wants to open \"YourApp\"") { alert -> Bool in
      alert.buttons["Open"].tap()
      return true
    }
    XCUIApplication().tap()

    // 12. Waiting for iOS launches our app
    sleep(3)

    // 13. Call assertion block for testing purpose
    assertion(XCUIApplication())

    // 14. Clear up the URL was added to **Contacts**
    contactsApp.activate()
    contactsApp.buttonTap(name: "Edit")
    let editTableView = contactsApp.tables.firstMatch
    XCTAssert(editTableView.waitForExistence(timeout: 3), "No 'Edit' table view found!")
    editTableView.swipeUp()
    let urlTextField = editTableView.cells.textFields[urlString].firstMatch
    XCTAssert(urlTextField.waitForExistence(timeout: 3), "No '\(urlString)' text field found!")
    urlTextField.tap()
    urlTextField.buttons["Clear text"].tap()
    contactsApp.buttons["Done"].tap()
    contactsApp.terminate()

    // 15. Remove the alert monitor
    removeUIInterruptionMonitor(alertHandler)
  }
}
```


**Add it into the test method**

```swift
func testOpenDeepLink() {
  let urls = ["https://geek-is-stupid.github.io"]
  for url in urls {
    openDeepLink(urlString: url) { app in
    // Your assertions here
    }
  }
}
```

**Now you can runt the test!**