---
title: Upgrading to 2.0 <b><i>(New)</i></b>
layout: docs
permalink: /docs/upgrading.html
prevPage: installation.html
nextPage: image-modification-block.html
---

AsyncDisplayKit **2.0 Beta** is (almost) here! Here's a brief summary of the changes on [master](https://github.com/facebook/AsyncDisplayKit) as of today:

**API naming improvements (find & replace):**

- `.usesImplicitHierarchyManagement` renamed to `.automaticallyManagesSubnodes` for [Automatic Subnode Management](http://asyncdisplaykit.org/docs/implicit-hierarchy-mgmt.html)
- `ASRelativeDimensionTypePercent` and associated functions renamed to use `Fraction` to be consistent with Apple terminology.

**Updated Interface State callback method names**

The new names are meant to unify the range update methods to show how they relate to each other & hopefully be a bit more self explanatory:

- `didEnter/ExitPreloadState`
- `didEnter/ExitDisplayState`
- `didEnter/ExitVisibleState`

These new methods replace the following:

- `loadStateDidChange:(BOOL)inLoadState`
- `displayStateDidChange:(BOOL)inDisplayState`
- `visibleStateDidChange:(BOOL)isVisible`

**Make sure to check:**

- `constrainedSizeForNodeAtIndexPath:` moved from the `.dataSource` to the `.delegate` to be consistent with UIKit definitions of the roles. **Note:** Make sure that you provide a delegate for any `ASTableNode`, `ASCollectionNode` or `ASPagerNodes` that use this method. 
- collection view update validation assertions are now enabled. If you see something like `"Invalid number of items in section 2. The number of items after the update (7) must be equal to the number of items before the update (4) plus or minus the number of items inserted or removed from the section (4 inserted, 0 removed)”`, please check the data source logic. If you have any questions, reach out to us on GitHub. 

**Layout Changes:**

- [Layout Transition API](http://asyncdisplaykit.org/docs/layout-transition-api.html) (`transitionLayoutWithDuration:`) has been moved out of Beta
- `ASStackLayoutSpec`'s `.alignItems` property now defaults to `ASStackLayoutAlignItemsStretch` instead of `ASStackLayoutAlignItemsStart` to align with the CSS align-items property.

The majority of the remaining (unmerged) changes will be in the Layout API. 
