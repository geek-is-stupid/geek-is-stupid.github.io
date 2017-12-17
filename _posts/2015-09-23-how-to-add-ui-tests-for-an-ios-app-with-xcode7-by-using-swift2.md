---
layout: post
title: How to Add iOS UI Test in Xcode 7?
categories: [ios]
tags: [swift, xcode, ios, uitest, xctestcase]
fullview: true
---

##### 1. Create simple project: Click on a button and then get the text in textfield for the label

![https://stackoverzone.files.wordpress.com/2015/10/1.png](https://stackoverzone.files.wordpress.com/2015/10/1.png)

- Add a text field, a button, and a label:


![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-4-58-26-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-4-58-26-pm.png)

- Process the behavior when user tap on the button

```swift
import UIKit

class ViewController: UIViewController {

    @IBOutlet weak var textField: UITextField!

    @IBOutlet weak var label: UILabel!

    @IBOutlet weak var button: UIButton!

    override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
    }

    @IBAction func buttonTapped(sender: AnyObject) {
        label.text = textField.text
        textField.resignFirstResponder()
    }
}
```

##### 2. Create new iOS UI Testing Bundle


- Click on your project


![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-16-38-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-16-38-pm.png)

- Click + button to add new iOS UI Testing Bundle


![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-4-56-03-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-4-56-03-pm.png)

- Select the iOS UI Testing Bundle

![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-18-07-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-18-07-pm.png)

- New bundle will be here:


![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-18-50-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-18-50-pm.png)

##### 3. Recording your behaviors in simulator


- Open the file: "UITestExampleUITests.swift"

- Let your cursor within testExample() method, you will see the circle red button at the bottom of Xcode, press on it


![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-22-07-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-22-07-pm.png)

- The simulator will be show and fill the text "testing" into the textfield and then click button:


![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-26-38-pm.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-26-38-pm.png)

- The `testExample()` method in UITestExampleUITests.swift will be filled as something like this:

```swift
func testExample() {
     let app = XCUIApplication()
     app.otherElements.containingType(.Button, identifier:"Button").childrenMatchingType(.TextField).element.typeText("testing")
     app.buttons["Button"].tap()
}
```

- Turn of recording: Pressed on the circle has the red square inside

![https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-32-00-pm1.png](https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-32-00-pm1.png)

##### 4. Add specific assert - [Test Here]


- Add this statement above of testExample() method

```swift
XCTAssert(app.staticTexts["testing"].exists)
```

This line mean: With the behavior we recorded (Item 3), the UI must be contain the text "testing"


- The testExample() method will be:


```swift
func testExample() {
     let app = XCUIApplication()
     app.otherElements.containingType(.Button, identifier:"Button").childrenMatchingType(.TextField).element.typeText("testing")
     app.buttons["Button"].tap()
     XCTAssert(app.staticTexts["testing"].exists)
}
```

##### 5. Run test: Command + U

##### 6. Basic statements:

- Waiting: Sometime you must to waiting for the animation done, or the results from network

```swift
func waitForElement(elementType: XCUIElement, timeout: NSTimeInterval ) {
	let elementExist = NSPredicate(format: "exists == 1")
	expectationForPredicate(elementExist, evaluatedWithObject: elementType, handler: nil)
	waitForExpectationsWithTimeout(timeout, handler: nil)
}
```

- Get UITableViewCell

```swift
let app = XCUIApplication()
let cell = app.tables.staticTexts["About"] // This is a cell has content text: `About`
```