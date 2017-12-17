---
layout: post
title: How to make a paged UICollectionView?
categories: [ios]
tags: [swift, xcode, ios, UICollectionView, paging]
fullview: true
---

The layout for our UICollectionView looks like:

![https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2017-02-20-how-to-paging-uicollectionview/layout.png](https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2017-02-20-how-to-paging-uicollectionview/layout.png)

Create new class as subclass for: `UICollectionViewFlowLayout`

```swift
import UIKit

public class PageCollectionViewFlowLayout: UICollectionViewFlowLayout {
    override public func targetContentOffset(forProposedContentOffset proposedContentOffset: CGPoint, withScrollingVelocity velocity: CGPoint) -> CGPoint {
        if let collectionView: UICollectionView = self.collectionView {
            let bounds: CGRect = collectionView.bounds
            let halfWidth = bounds.size.width * 0.5
            let proposedContentOffsetCenterX: CGFloat = proposedContentOffset.x + halfWidth

            if let attributesForVisibleCells = self.layoutAttributesForElements(in: bounds) {
                var candidateAttributes : UICollectionViewLayoutAttributes?
                for attributes in attributesForVisibleCells {
                    if attributes.representedElementCategory != UICollectionElementCategory.cell { continue }

                    if (attributes.center.x == 0) || (attributes.center.x > (collectionView.contentOffset.x + halfWidth) && velocity.x < 0) { continue }

                    if let candAttrs = candidateAttributes {
                        let a = attributes.center.x - proposedContentOffsetCenterX
                        let b = candAttrs.center.x - proposedContentOffsetCenterX

                        if fabsf(Float(a)) < fabsf(Float(b)) {
                            candidateAttributes = attributes;
                        }
                    } else {
                        candidateAttributes = attributes
                        continue
                    }
                }

                if(proposedContentOffset.x == -(collectionView.contentInset.left)) {
                    return proposedContentOffset
                }

                return CGPoint(x: floor(candidateAttributes!.center.x - halfWidth), y: proposedContentOffset.y)
            }
        }
        return super.targetContentOffset(forProposedContentOffset: proposedContentOffset)
    }
}
```

Now go to Xcode to change the class for your `UICollectionView`:

![https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2017-02-20-how-to-paging-uicollectionview/FlowLayout.png](https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2017-02-20-how-to-paging-uicollectionview/FlowLayout.png)

And change the class for your Flow layout:

![https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2017-02-20-how-to-paging-uicollectionview/ChangeClassForFlowLayout.png](https://raw.githubusercontent.com/geek-is-stupid/geek-is-stupid.github.io/master/images/post/2017-02-20-how-to-paging-uicollectionview/ChangeClassForFlowLayout.png)

Now show time!!! 📟
