---
layout: post
title: "How to add iOS UI test in Xcode 7?"
date: 2015-10-23 14:52:13 +0700
comments: true
categories: 
---
### 1. Create simple project: Click on a button and then get the text in textfield for the label</h3>

<a href="https://stackoverzone.files.wordpress.com/2015/10/1.png"><img class="alignnone size-medium wp-image-227" src="https://stackoverzone.files.wordpress.com/2015/10/1.png" /></a>

<h5>1.1 Add a text field, a button, and a label:</h5>
<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-4-58-26-pm.png"><img class="alignnone size-medium wp-image-229" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-4-58-26-pm.png" alt="Screen Shot 2015-10-06 at 4.58.26 PM" /></a>

<h5>1.2 Process the behavior when user tap on the button</h5>

```objc

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

### 2. Create new iOS UI Testing Bundle
<h5>2.1: Click on your project</h5>
<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-16-38-pm.png"><img class="alignnone size-medium wp-image-232" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-16-38-pm.png" alt="Screen Shot 2015-10-06 at 5.16.38 PM" /></a>

<h5>2.2: Click + button to add new iOS UI Testing Bundle</h5>
<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-4-56-03-pm.png"><img class="alignnone size-medium wp-image-231" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-4-56-03-pm.png" alt="Screen Shot 2015-10-06 at 4.56.03 PM" /></a>

<h5>2.3: Select the iOS UI Testing Bundle</h5>
<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-18-07-pm.png"><img class="alignnone size-medium wp-image-233" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-18-07-pm.png" alt="Screen Shot 2015-10-06 at 5.18.07 PM" /></a>

<h5>2.4 New bundle will be here:</h5>
<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-18-50-pm.png"><img class="alignnone size-medium wp-image-234" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-18-50-pm.png" alt="Screen Shot 2015-10-06 at 5.18.50 PM" /></a>

###3. Recording your behaviors in simulator

<h5>3.1 Open the file: <code>"UITestExampleUITests.swift"</code></h5>

<h5>3.2 Let your cursor within <code>testExample()</code> method, you will see the circle red button at the bottom of Xcode, press on it</h5>
<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-22-07-pm.png"><img class="alignnone size-medium wp-image-236" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-22-07-pm.png" alt="Screen Shot 2015-10-06 at 5.22.07 PM" /></a>

<h5>3.3 The simulator will be show and fill the text "testing" into the textfield and then click button:</h5>
<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-26-38-pm.png"><img class="alignnone size-medium wp-image-237" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-26-38-pm.png" alt="Screen Shot 2015-10-06 at 5.26.38 PM" /></a>

<h5>3.4 The <strong><code>testExample()</code></strong> method in <strong>UITestExampleUITests.swift</strong> will be filled as something like this:</h5>

```swift
func testExample() {
     let app = XCUIApplication()
     app.otherElements.containingType(.Button, identifier:"Button").childrenMatchingType(.TextField).element.typeText("testing")
     app.buttons["Button"].tap()
}
```

<h5>3.5 Turn of recording: Pressed on the circle has the red square inside</h5>
<a href="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-32-00-pm1.png"><img class="alignnone size-full wp-image-252" src="https://stackoverzone.files.wordpress.com/2015/10/screen-shot-2015-10-06-at-5-32-00-pm1.png" alt="Screen Shot 2015-10-06 at 5.32.00 PM" /></a>

###4. Add specific assert - [Test Here]
<h5>4.1 Add this statement above of <code>testExample()</code> method</h5>

```swift
XCTAssert(app.staticTexts["testing"].exists)
```

<h4>This line mean:<strong> With the behavior we recorded (Item 3), the UI must be contain the text <code>"testing"</code></strong></h4>
<h5>4.2 The<strong> <code>testExample()</code></strong> method will be:</h5>

```swift
func testExample() {
     let app = XCUIApplication()
     app.otherElements.containingType(.Button, identifier:"Button").childrenMatchingType(.TextField).element.typeText("testing")
     app.buttons["Button"].tap()
     XCTAssert(app.staticTexts["testing"].exists)
}
```

###5. Run test: <code> Command + U </code>

###6. Basic statements:
- Waiting: Sometime you must to waiting for the animation done, or the results from network

```
func waitForElement(elementType: XCUIElement, timeout: NSTimeInterval ) {
        let elementExist = NSPredicate(format: "exists == 1")
        expectationForPredicate(elementExist, evaluatedWithObject: elementType, handler: nil)
        waitForExpectationsWithTimeout(timeout, handler: nil)
    }
```
- Get UITableViewCell

```
let app = XCUIApplication()
let cell = app.tables.staticTexts["About"] // This is a cell has content text: `About`
```