---
title: Node Subclasses
layout: docs
permalink: /docs/node-overview.html
prevPage: containers-overview.html
nextPage: subclassing.html
---

AsyncDisplayKit offers the following nodes.  

A key advantage of using nodes over UIKit components is that **all nodes preform layout and display off of the main thread**, so that the main thread is available to immediately respond to user interaction events.  

<table style="width:100%" class = "paddingBetweenCols">
  <tr>
    <th>ASDK Node</th>
    <th>UIKit Equivalent</th> 
  </tr>
  <tr>
    <td><a href = "display-node.html">`ASDisplayNode`</a></td>
    <td>in place of UIKit's `UIView`<br> 
        <i>The root AsyncDisplayKit node, from which all other nodes inherit.</i></td> 
  </tr>
  <tr>
    <td><a href = "cell-node.html">`ASCellNode`</a></td>
    <td>in place of UIKit's `UITableViewCell` & `UICollectionViewCell`<br>
        <i>`ASCellNode`s are used in `ASTableNode`, `ASCollectionNode` and `ASPagerNode`.</i></td> 
  </tr>
  <tr>
    <td><a href = "scroll-node.html">`ASScrollNode`</a></td>
    <td>in place of UIKit's `UIScrollView`
        <p><i>This node is useful for creating a customized scrollable region that contains other nodes.</i></p></td> 
  </tr>
  <tr>
    <td><a href = "editable-text-node.html">`ASEditableTextNode`</a><br>
        <a href = "text-node.html">`ASTextNode`</a></td>
    <td>in place of UIKit's `UITextView`<br>
        in place of UIKit's `UILabel`</td> 
  </tr>
  <tr>
    <td><a href = "image-node.html">`ASImageNode`</a><br>
        <a href = "network-image-node.html">`ASNetworkImageNode`</a><br>
        <a href = "multiplex-image-node.html">`ASMultiplexImageNode`</a></td>
    <td>in place of UIKit's `UIImage`</td> 
  </tr>
  <tr>
    <td><a href = "video-node.html">`ASVideoNode`</a><br>
        `ASVideoPlayerNode`</td>
    <td>in place of UIKit's `AVPlayerLayer`<br>
        in place of UIKit's `UIMoviePlayer`</td> 
  </tr>
  <tr>
    <td><a href = "control-node.html">`ASControlNode`</a></td>
    <td>in place of UIKit's `UIControl`</td>
  </tr>
  <tr>
    <td><a href = "button-node.html">`ASButtonNode`</a></td>
    <td>in place of UIKit's `UIButton`</td>
  </tr>
  <tr>
    <td><a href = "map-node.html">`ASMapNode`</a></td>
    <td>in place of UIKit's `MKMapView`</td>
  </tr>
</table>

<br>
Despite having rough equivalencies to UIKit components, in general, AsyncDisplayKit nodes offer more advanced features and conveniences. For example, an `ASNetworkImageNode` does automatic loading and cache management, and even supports progressive jpeg and animated gifs. 

The <a href = "https://github.com/facebook/AsyncDisplayKit/tree/master/examples/AsyncDisplayKitOverview">`AsyncDisplayKitOverview`</a> example app gives basic implementations of each of the nodes listed above. 
 

# Node Inheritance Hierarchy 

All AsyncDisplayKit nodes inherit from `ASDisplayNode`. 

<img src="/static/images/node-hierarchy.png" alt="node inheritance flowchart">

The nodes highlighted in blue are synchronous wrappers of UIKit elements.  For example, `ASScrollNode` wraps a `UIScrollView`, and `ASCollectionNode` wraps a `UICollectionView`.  An `ASMapNode` in `liveMapMode` is a synchronous wrapper of `UIMapView`.


 
 
