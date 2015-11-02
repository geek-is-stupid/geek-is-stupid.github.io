---
layout: post
title: "How to calculate the height of the text depend on width and font using Swift2.0?"
date: 2015-10-16 17:19:50 +0700
comments: true
categories: 
---
The height of the text depend on the font, width and especially it has multiple line.

```swift
extension String {
    func sizeWithFont(font: UIFont, forWidth width: CGFloat) -> CGSize {
        let fString = self as NSString
        let paragraphStyle = NSMutableParagraphStyle()
        paragraphStyle.lineBreakMode = NSLineBreakMode.ByWordWrapping
        let attrDict = [
            NSFontAttributeName: font,
            NSParagraphStyleAttributeName: paragraphStyle
        ]
        let maximumSize = CGSize(width: width, height: CGFloat(MAXFLOAT))
        let rect = fString.boundingRectWithSize(maximumSize,
            options: [.TruncatesLastVisibleLine, .UsesLineFragmentOrigin],
            attributes: attrDict,
            context: nil)
        return rect.size
    }
}
```
