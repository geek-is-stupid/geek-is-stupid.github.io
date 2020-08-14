---
layout: post
title: "iOS UITextView for Flutter"
categories: ios
tags: swift ios UITextView Flutter 
fullview: true
---

<img width="499" alt="Screen Shot 2020-05-06 at 17 37 05" src="https://user-images.githubusercontent.com/6329656/90223061-8a1dd580-de37-11ea-8f0b-96636540a6e2.jpg">

Currently there is no TextView for Flutter, so we have to have an injection native TextView from iOS & Android to Flutter project.

## PlaformView & PlaformChannel

[https://flutter.dev/docs/development/platform-integration/platform-channels](https://flutter.dev/docs/development/platform-integration/platform-channels)

- This is an API from Flutter to create Widgets from Native Views!
- And the way how **Dart** & **iOS**/**Android** communicate to each order

<img width="499" alt="Screen Shot 2020-05-06 at 17 37 05" src="https://flutter.dev/images/PlatformChannels.png">

## Implementation
### In Flutter project

#### Create a Widget call TextView in Flutter side

```dart
import 'dart:async';
import 'package:flutter/foundation.dart';
import 'package:flutter/material.dart';
import 'package:flutter/services.dart';

typedef void TextViewCreatedCallback(TextViewController controller);

class TextView extends StatefulWidget {
  const TextView({
    Key key,
    this.onTextViewCreated,
  }) : super(key: key);

  final TextViewCreatedCallback onTextViewCreated;

  @override
  State<StatefulWidget> createState() => _TextViewState();
}

class _TextViewState extends State<TextView> {
  @override
  Widget build(BuildContext context) {
    if (defaultTargetPlatform == TargetPlatform.android) {
      // The native TextView from AndroidView
    }
    if (defaultTargetPlatform == TargetPlatform.iOS) {
      	// viewType `FlutterUITextView` should be matched when register in iOS side
    	return UiKitView(
        viewType: 'FlutterUITextView',
        onPlatformViewCreated: _onPlatformViewCreated,
      );
    }
    return Text('$defaultTargetPlatform is not yet supported by the text_view plugin');
  }

  void _onPlatformViewCreated(int id) {
    if (widget.onTextViewCreated == null) {
      return;
    }
    widget.onTextViewCreated(new TextViewController._(id));
  }
}

class TextViewController {

  // Open a channel with name is `com.my_app.channel.textview_$id`
  // Make sure the iOS/Android observe on same the name of this channel
  TextViewController._(int id) : _channel = new MethodChannel('com.my_app.channel.textview_$id');

  final MethodChannel _channel;

  // Tell the iOS/Android to set the html text to the UITextView (iOS), TextView (Kotlin) 
  Future<void> setHtmlText({String text}) async {
    assert(text != null);
    Map<String, dynamic> arguments = {
      'text': text,
    };
    return _channel.invokeMethod('setHtmlText', arguments);
  }
}
```

We also can having more methods same as `setHtmlText` to setting up more data for the TextView from native side

#### Caller from Flutter

```dart
TextView(onTextViewCreated: (TextViewController controller) {
	controller.setHtmlText(text: "<html><body>Hello World</body></html>");
})
```

### In iOS project

#### Setting preview 

Open the **Info.plist**, adding key `io.flutter.embedded_views_preview` with value is `true` to allow Flutter previewing on the UIView.

```swift
<key>io.flutter.embedded_views_preview</key>
<true/>
```

#### Create the TextView compatible with FlutterPlatformView 

```swift
public class FlutterUITextView: NSObject, FlutterPlatformView {
    let frame: CGRect
    let viewId: Int64
    let arguments: Any?
    let textView: UITextView
    var channel: FlutterMethodChannel?

    public init(withFrame frame: CGRect, viewIdentifier viewId: Int64, arguments args: Any?,
         messenger: FlutterBinaryMessenger) {
        self.frame = frame
        self.viewId = viewId
        self.arguments = args
        self.textView = UITextView()
        self.channel = nil
        super.init()
        
        // This channel name has to be matched with the the name that we defined in `TextViewController` above
        self.channel = FlutterMethodChannel(name: "com.my_app.channel.textview_\(viewId)", binaryMessenger: messenger)
        channel?.setMethodCallHandler({ [weak self] (call: FlutterMethodCall, result: FlutterResult) -> Void in
            switch call.method {
            case "setHtmlText":
                let args = call.arguments as? [String: Any]
                self?.set(htmlText: (args?["text"] as? String) ?? "")
            default:
                break
            }
        })
    }

    public func view() -> UIView {
        return textView
    }

    private func set(htmlText: String) {
        DispatchQueue.main.async {
            //Rendering HTML in in next cycle cuz html from text is expensive task.
            let format = #"<span style="font-size:%.2fpx;font-family:'-apple-system';font-weight:400;color:#40485A;">%@</span>"#
            let html = String(format: format, 14.0, htmlText)
            let data = html.data(using: .utf8)!
            let attributedText = try! NSAttributedString(data: data,
                                          options: [.documentType: NSAttributedString.DocumentType.html,
                                                    .characterEncoding: String.Encoding.utf8.rawValue],
                                          documentAttributes: nil)
            self.textView.attributedText = attributedText
        }
    }
}
```

#### Create an UITextView Factory 

```swift
public class FlutterUITextViewViewFactory: NSObject, FlutterPlatformViewFactory {
    let messenger: FlutterBinaryMessenger

    public init(messenger: FlutterBinaryMessenger) {
        self.messenger = messenger
    }

    public func create(withFrame frame: CGRect, viewIdentifier viewId: Int64, arguments args: Any?) -> FlutterPlatformView {
        return FlutterUITextView(withFrame: frame, viewIdentifier: viewId,
                                 arguments: args, messenger: messenger)
    }

    public func createArgsCodec() -> FlutterMessageCodec & NSObjectProtocol {
        return FlutterStandardMessageCodec.sharedInstance()
    }
}
```

#### Setting Flutter engine

This setup for the iOS project **without** the `FlutterAppDelegate`

1: Go to the swift file that you wanna present the Flutter app

2: Importing 

```swift
import Flutter
import FlutterPluginRegistrant
```

3: Store the Flutter engine, make sure this variable is global variable in your class

```swift
let flutterEngine = FlutterEngine(name: "this_is_my_flutter_app")
```

- name: Just indicated the name for the engine

4: Start & register the engine

```swift
flutterEngine.run()
GeneratedPluginRegistrant.register(with: flutterEngine)
```


More details about the Flutter Engine in iOS ([https://flutter.dev/docs/development/add-to-app/ios/add-flutter-screen?tab=engine-swift-tab](https://flutter.dev/docs/development/add-to-app/ios/add-flutter-screen?tab=engine-swift-tab))


5: Register the FlutterUITextView to the Flutter engine

```swift
let registrar = flutterEngine.registrar(forPlugin: "FlutterUITextView")!
let viewFactory = FlutterUITextViewViewFactory(messenger: registrar.messenger())
registrar.register(viewFactory, withId: "FlutterUITextView")
```

- The param `withId` (`"FlutterUITextView"`) should be matched with the `viewType` that we defined with `UiTextView`'s `viewType` in Flutter code.

6: Present the Flutter app

```swift
let flutterVC = FlutterViewController(engine: flutterEngine, nibName: nil, bundle: nil)
parent?.present(flutterVC, animated: true, completion: nil)
```


![Screen Shot 2020-08-14 at 14 52 35](https://user-images.githubusercontent.com/6329656/90226830-d10ec980-de3d-11ea-858d-6ac4c2241d5d.png)

<img width="1552" alt="Screen Shot 2020-08-14 at 15 00 06" src="https://user-images.githubusercontent.com/6329656/90227541-f223ea00-de3e-11ea-87fd-4eb7057b8ebc.png">

## Ref

- [https://api.flutter.dev/javadoc/io/flutter/plugin/platform/PlatformView.html](https://api.flutter.dev/javadoc/io/flutter/plugin/platform/PlatformView.html)
- [https://flutter.dev/docs/development/add-to-app/ios/add-flutter-screen?tab=engine-swift-tab](https://flutter.dev/docs/development/add-to-app/ios/add-flutter-screen?tab=engine-swift-tab)


