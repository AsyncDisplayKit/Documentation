---
title: Upgrading to 2.0 <b><i>(Beta)</i></b>
layout: docs
permalink: /docs/upgrading.html
---

Here's a brief summary of the AsyncDisplayKit 2.0 Beta API breaking changes on `master` as of today:

### Layout

The most significant change in the 2.0 release is a sweeping upgrade to AsyncDisplayKit's Layout API. Please read the separate <a href="layout2-conversion-guide.html"><b>Layout 2.0 Conversion Guide</b></a> for an overview of the upgrades and to see how to convert your existing layout code. 

Other layout updates include:

- The <a href = "layout-transition-api.html">Layout Transition API</a> (`transitionLayoutWithDuration:`) has been moved out of Beta. Significant new functionality is planed for future dot releases. 

- `.usesImplicitHierarchyManagement` has been renamed to `.automaticallyManagesSubnodes` for <a href = "http://asyncdisplaykit.org/docs/implicit-hierarchy-mgmt.html">Automatic Subnode Management</a>. This API has been moved out of Beta, but has a few documented <a href = "">limitations</a>. 

### Collection & Table API

Another significant change in the 2.0 release is an update to the Collection / Table APIs. These APIs have been moved from the view space (`collectionView` / `tableView`) to the node space (`collectionNode` / `tableNode`). 

- Any `indexPath` that is passed to the `collectionView` space references data that has been synced with `ASCollectionNode`'s underlying `UICollectionView`. Conversly, any `indexPath` that is passed to the `collectionNode` space references asynchronous data that *might not yet* have been synced with ASCollectionNode's underlying `UICollectionView`. The same concepts apply to `ASTableNode`.

- An exception to this is `ASTableNode`'s `-didSelectRowAtIndexPath:`, which is called in UIKit space to make sure that `indexPath` indicies reference the data in the onscreen (data that has been synced to the underlying `UICollectionView` `dataSource`).

- While previous versions of the framework required the developer to be aware of the asynchronous interplay between `ASCollectionNode` and its underlying `UICollectionView`, this new API should provide better safegaurds against developer-introduced data source inconsistencies. 

- **Please Note** that not all of the older `collectionNode` and `tableNode` `dataSource` / `delegate` methods will warn that they have been deprecated. So, please `cmd` + `shift` + `f` to search for instances of `tableView` and `collectionView` in your project’s code. Most, if not all, of these methods have new node versions. 

Other table / collection updates include:

- Deprecate `ASTableView`'s -init method. Please use `ASTableNode` instead of `ASTableView`. While this makes adopting the framework marginally more difficult to, the benefits of using ASTableNode / ASCollectionNode over their ASTableView / ASCollectionView counterparts are signficant. 

- Deprecate `-beginUpdates` and `-endUpdatesAnimated:`. Please use the `-performBatchUpdates:` methods instead.

- Deprecate `-reloadDataImmediately`. Please see the header file comments for the deprecation solution.
 
- Moved range tuning to the `tableNode` and `collectionNode` (from the `tableView` / `collectionView`)

- `constrainedSizeForNodeAtIndexPath:` moved from the `.dataSource` to the `.delegate` to be consistent with UIKit definitions of the roles. **Note:** Make sure that you provide a delegate for any `ASTableNode`, `ASCollectionNode` or `ASPagerNodes` that use this method. Your code will silently not call your delegate method, if you do not have a delegate assigned. 

- collection view update validation assertions are now enabled. If you see something like `"Invalid number of items in section 2. The number of items after the update (7) must be equal to the number of items before the update (4) plus or minus the number of items inserted or removed from the section (4 inserted, 0 removed)”`, please check the data source logic. If you have any questions, reach out to us on GitHub. 

Collection / Table API Resources: 

- PR [#2390](https://github.com/facebook/AsyncDisplayKit/pull/2390) and PR [#2381](https://github.com/facebook/AsyncDisplayKit/pull/2381) show how we converted the [example projects](https://github.com/facebook/AsyncDisplayKit/tree/master/examples) to conform to this new API. 

- Video of the `ASCollectioNode` Behind-the-Scenes [talk at Pinterest](https://youtu.be/yuDqvE5n_1g).

### Updated Interface State Callback Methods

The new method names are meant to unify the range update methods to show how they relate to each other & be a bit more self-explanatory:

- `didEnter/ExitPreloadState`
- `didEnter/ExitDisplayState`
- `didEnter/ExitVisibleState`

These new methods replace the following:

- `loadStateDidChange:(BOOL)inLoadState`
- `displayStateDidChange:(BOOL)inDisplayState`
- `visibleStateDidChange:(BOOL)isVisible`
