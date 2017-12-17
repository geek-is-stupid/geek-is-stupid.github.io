---
layout: post
title: How to make an UIColor from HEX by using Swift3?
categories: [ios]
tags: [swift, xcode, ios, UIColor, hex]
fullview: true
---

We create a new construction method for UIColor:

```swift
extension UIColor {
  convenience init?(hex: String, alpha: Float = 1.0) {
        var hex = hex

        if hex.hasPrefix("#") {
            hex = hex.substring(from: hex.characters.index(hex.startIndex, offsetBy: 1))
        }

        if let _ = hex.range(of: "(^[0-9A-Fa-f]{6}$)|(^[0-9A-Fa-f]{3}$)", options: .regularExpression) {

            if hex.characters.count == 3 {
                let redHex   = hex.substring(to: hex.characters.index(hex.startIndex, offsetBy: 1))
                let greenHex = hex.substring(with: hex.characters.index(hex.startIndex, offsetBy: 1)..<hex.characters.index(hex.startIndex, offsetBy: 2))
                let blueHex  = hex.substring(from: hex.characters.index(hex.startIndex, offsetBy: 2))

                hex = redHex + redHex + greenHex + greenHex + blueHex + blueHex
            }

            let redHex = hex.substring(to: hex.characters.index(hex.startIndex, offsetBy: 2))
            let greenHex = hex.substring(with: hex.characters.index(hex.startIndex, offsetBy: 2)..<hex.characters.index(hex.startIndex, offsetBy: 4))
            let blueHex = hex.substring(with: hex.characters.index(hex.startIndex, offsetBy: 4)..<hex.characters.index(hex.startIndex, offsetBy: 6))

            var redInt:   CUnsignedInt = 0
            var greenInt: CUnsignedInt = 0
            var blueInt:  CUnsignedInt = 0

            Scanner(string: redHex).scanHexInt32(&redInt)
            Scanner(string: greenHex).scanHexInt32(&greenInt)
            Scanner(string: blueHex).scanHexInt32(&blueInt)

            self.init(red: CGFloat(redInt) / 255.0, green: CGFloat(greenInt) / 255.0, blue: CGFloat(blueInt) / 255.0, alpha: CGFloat(alpha))
        } else {
            self.init()
            return nil
        }
    }
}
```

OK:

```swift
let color: UIColor = UIColor(hex: "#4286f4")!
```

Question how to get hex string from an UIColor?

```swift
extension UIColor {
  var hex: String {
        var r:CGFloat = 0
        var g:CGFloat = 0
        var b:CGFloat = 0
        var a:CGFloat = 0

        getRed(&r, green: &g, blue: &b, alpha: &a)

        let rgb:Int = (Int)(r*255)<<16 | (Int)(g*255)<<8 | (Int)(b*255)<<0

        return NSString(format:"%06x", rgb) as String
    }
}
```

Show time:

```swift
debugPrint(yourColor.hex)
```

🖍🖍🖍🖍🖍