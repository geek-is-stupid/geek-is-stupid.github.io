---
layout: post
title: "App crashes 'NSInternalInconsistencyException' while calling 'reloadRows(at&#58;with&#58;)'"
categories: ios
tags: swift xcode ios UITableView, NSInternalInconsistencyException reloadData reloadRows crashes dataSource
fullview: true
---

### ⚡️ Problems
Sometime I got a lot of crashes related to `reloadRows(at:with:)` due to the unsync between current data in `UITableView` and the data from `dataSource`

```
Fatal Exception: NSInternalInconsistencyException
Invalid update: invalid number of rows in section 0. The number of rows contained in an existing section after the update (12) must be equal to the number of rows contained in that section before the update (9), plus or minus the number of rows inserted or deleted from that section (1 inserted, 1 deleted) and plus or minus the number of rows moved into or out of that section (0 moved in, 0 moved out).
```

### 💡 Ideas 
Why don't we compare current data in `tableView` and the data from `dataSource`?

### ✅ Solutions:

```swift
extension UITableView {
    var dataHasChanged: Bool {
        guard let dataSource = dataSource else { return false }
        let sections = dataSource.numberOfSections?(in: self) ?? 0
        if numberOfSections != sections {
            return true
        }
        for section in 0..<sections {
            if numberOfRows(inSection: section) != dataSource.tableView(self, numberOfRowsInSection: section) {
                return true
            }
        }
        return false
    }
}
```

And how to use it 🤔?

```swift
if tableView.dataHasChanged {
	tableView.reloadData()
} else {
	tableView.reloadRowsSafe(at: [indexPath], with: .automatic)
}
```

⚠️ You might be check the `indexPath` should appear in `tableView.indexPathsForVisibleRows` 


#### 👍 Like if you want => 🙏
