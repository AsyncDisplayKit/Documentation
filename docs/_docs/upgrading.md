---
title: Upgrading to 2.0 <b><i>(New)</i></b>
layout: docs
permalink: /docs/upgrading.html
---

Here's a brief summary of the AsyncDisplayKit 2.0 Beta changes on [master](https://github.com/facebook/AsyncDisplayKit) as of today:

### Layout

- The most significant change in the 2.0 release is a **sweeping upgrade to AsyncDisplayKit's Layout API**. Read the <a href="layout2-conversion-guide.html"><b>Conversion Guide</b></a> for an overview of the upgrades and to see examples showing how to convert your existing layout code. 

- The <a href = "layout-transition-api.html">Layout Transition API</a> (`transitionLayoutWithDuration:`) has been moved out of Beta. Significant new functionality is planed for future dot releases. 

- `.usesImplicitHierarchyManagement` has been renamed to `.automaticallyManagesSubnodes` for <a href = "http://asyncdisplaykit.org/docs/implicit-hierarchy-mgmt.html">Automatic Subnode Management</a>. This API has been moved out of Beta, but has a few documented <a href = "">limitations</a>. 

### Updated interface state callback method names

The new method names are meant to unify the range update methods to show how they relate to each other & be a bit more self-explanatory:

- `didEnter/ExitPreloadState`
- `didEnter/ExitDisplayState`
- `didEnter/ExitVisibleState`

These new methods replace the following:

- `loadStateDidChange:(BOOL)inLoadState`
- `displayStateDidChange:(BOOL)inDisplayState`
- `visibleStateDidChange:(BOOL)isVisible`

### Gotchas

- `constrainedSizeForNodeAtIndexPath:` moved from the `.dataSource` to the `.delegate` to be consistent with UIKit definitions of the roles. **Note:** Make sure that you provide a delegate for any `ASTableNode`, `ASCollectionNode` or `ASPagerNodes` that use this method. 

- collection view update validation assertions are now enabled. If you see something like `"Invalid number of items in section 2. The number of items after the update (7) must be equal to the number of items before the update (4) plus or minus the number of items inserted or removed from the section (4 inserted, 0 removed)”`, please check the data source logic. If you have any questions, reach out to us on GitHub. 
