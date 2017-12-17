---
layout: post
title: How to impove performance when scrolling UITableView/UICollectionView?
categories: [ios]
tags: [swift, xcode, ios, performance, scrolling, UITableView, UICollectionView]
fullview: true
---

###### There are multiple ways to impove the scrolling for UITableView/UICollectionView, today I will give my way to solved it, hope it will help you guys a little bit:
###### Now we should think about: Why we got lagging when scroll your table view or collection view

As my experiences, 2 important things I think those will root to lagging when scroll your table view/collection view:

- Configure cells too expensive
- Time to calculate height/size for cell take too long

So 2 methods need to change with `UITableView`:

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {}
```
and

```swift
func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {}
```

###### 1. Now we go with configure cells first:

```swift
func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
  let cell: YourTableViewCell = tableView.dequeueReusableCell(withIdentifier: "YourTableViewCell", for: indexPath) as! MessageTableViewCell

  let model: YourModel = models[indexPath.row]

  DispatchQueue(label: "com.geek-is-stupid.queue.configure-cell").async {
      let image: URL = model.image
      let age: Int = model.age
      let name: String = model.name
      let message: String = model.message
      DispatchQueue.main.async {
          cell.imageView.image = image
          cell.nameLabel.text = name
          cell.ageLabel.text = "\(age)"
          cell.messageLabel.text = message
      }
  }
  
  return cell
}
```

###### 2. Impove calculating height for cells

- Calculate cell’s height:

```swift
class YourTableViewCell: UITableViewCell {
  static let shared: MessageTableViewCell = UINib(nibName: "YourTableViewCell", bundle: nil).instantiate(withOwner: nil, options: nil)[0] as! YourTableViewCell
  
  func height(model: YourModel) -> CGFloat {
      //Depend on your cell's layout, you will have different way to get cell's height.
      // e.g There are 3 vertically labels on my cell
      let messageHeight: CGFloat = model.message.size(font: message.messageFont, width: message.messageWidth).height
      let nameHeight: CGFloat = model.name.size(font: message.messageFont, width: message.messageWidth).height
      let ageHeight: CGFloat = "\(model.age)".size(font: message.messageFont, width: message.messageWidth).height
      return messageHeight + nameHeight + ageHeight
  }
}
```

- Need an array to cache calcuated heights:

Back to your view controller


```swift
fileprivate var cachedHeights: [Int: CGFloat] = [:]

func tableView(_ tableView: UITableView, heightForRowAt indexPath: IndexPath) -> CGFloat {
  if let cellHeight = cachedHeights[indexPath.row] {
      return cellHeight
  }
  let model: YourModel = models[indexPath.row]
  let cell: YourTableViewCell = .shared
  let cellHeight: CGFloat = cell.height(model: model)
  cachedHeights[index] = cellHeight
  return cellHeight
}
```

Method to get size for a string:

```swift
extension String {
  func size(font: UIFont, width: CGFloat) -> CGSize {
        let attrString: NSAttributedString = NSAttributedString(string: self, attributes: [NSFontAttributeName: font])
        let bounds: CGRect = attrString.boundingRect(with: CGSize(width: width, height: 0.0), options: .usesLineFragmentOrigin, context: nil)
        let size: CGSize = CGSize(width: bounds.width + 1, height: bounds.height + 2*(font.lineHeight - font.pointSize))
        return size
    }
}
```

OK, hope this works for you guys!!!