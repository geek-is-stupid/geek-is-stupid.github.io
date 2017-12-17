---
layout: post
title: How to Calculate the Height of the Text Depends on Width and Font by using Swift?
categories: [ios]
tags: [swift, xcode, ios, text height, uilabel, uitextview, string]
fullview: true
---

The height of the text depends on the font, width and especially it has multiple lines.

##### Swift3

```swift
extension String {
  func size(font: UIFont, width: CGFloat) -> CGSize {
        let attrString = NSAttributedString(string: self, attributes: [NSFontAttributeName: font])
        let bounds = attrString.boundingRect(with: CGSize(width: width, height: .greatestFiniteMagnitude), options: .usesLineFragmentOrigin, context: nil)
        let size = CGSize(width: bounds.width, height: bounds.height)
        return size
    }
}
```

Carefully if your text contains emoji:

```swift
extension String {
  func size(font: UIFont, width: CGFloat) -> CGSize {
        let attrString = NSAttributedString(string: self, attributes: [NSFontAttributeName: font])
        let framesetter = CTFramesetterCreateWithAttributedString(attrString)
        let size = CTFramesetterSuggestFrameSizeWithConstraints(framesetter, CFRange(location: 0, length: characters.count), nil, CGSize(width: width, height: .greatestFiniteMagnitude), nil)
        return size
    }
}
```

##### Swift 2

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

Cheer 🤗🤗🤗