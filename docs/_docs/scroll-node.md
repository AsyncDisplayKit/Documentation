---
title: ASScrollNode
layout: docs
permalink: /docs/scroll-node.html
prevPage: control-node.html
nextPage: editable-text-node.html
---

`ASScrollNode` is an `ASDisplayNode` whose underlying view is an `UIScrollView`. This class offers the ability to automatically adopt its `ASLayoutSpec`'s size as the scrollable `contentSize`. 

### automaticallyManagesContentSize

When enabled, the size calculated by the `ASScrolNode`'s layout spec is used as the `.contentSize` of the scroll view, instead of the bounds size.  The bounds is instead allowed to match the parent's size (whenever it is finite - otherwise, the bounds size also grows to the full `contentSize`).  

`automaticallyManagesContentSize` works well either for a blank `ASScrollNode` with `.layoutSpecBlock` set on it, or a subclass of `ASScrollNode` with a more
traditional `layoutSpecThatFits:` implementation.  

With this approach there is no need to capture the layout size, use an absolute layout spec as a wrapper, or set `contentSize` anywhere in the code and it will update as the layout changes!

There is currently no automatic management of `contentInset`, but it would make sense to add this with keyboard listeners in the future.

### scrollableDirections 

When using `automaticallyManagesContentSize`, you will want to set the `ASScrollNode`'s <code><b>scrollableDirections</b></code> as well. This property controls how the `constrainedSize` is interpreted when sizing the content. If you are using `automaticallyManagesContentSize`, it can be crucial to ensure that the sizing is done how you expect. 

Options include:

<table style="width:100%" class = "paddingBetweenCols">
  <tr>
    <td><b>Vertical</b></td>
    <td>The `constrainedSize` is interpreted as having unbounded `.height` (`CGFLOAT_MAX`), allowing stacks and other content in the layout spec to expand and result in scrollable content.</td> 
  </tr>
  <tr>
    <td><b>Horizontal</b></td>
    <td>The `constrainedSize` is interpreted as having unbounded `.width` (`CGFLOAT_MAX`).</td> 
  </tr>
  <tr>
    <td><b>Vertical & Horizontal</b></td>
    <td>The `constrainedSize` is interpreted as unbounded in both directions.</td>
  </tr>
</table>

### Basic Usage

In case you're not familiar with scroll views, they are basically windows into content that would take up more space than can fit in that area.

Say you have a giant image, but you only want to take up 200x200 pts on the screen.

<div class = "highlight-group">
<span class="language-toggle"><a data-lang="swift" class="swiftButton">Swift</a><a data-lang="objective-c" class = "active objcButton">Objective-C</a></span>

<div class = "code">
<pre lang="objc" class="objcCode">
UIImage *scrollNodeImage = [UIImage imageNamed:@"image"];
ASScrollNode *scrollNode = [[ASScrollNode alloc] init];

scrollNode.style.preferredSize = CGSizeMake(200.0, 200.0);

UIScrollView *scrollNodeView = scrollNode.view;
[scrollNodeView addSubview:[[UIImageView alloc] initWithImage:scrollNodeImage]];
scrollNodeView.contentSize = scrollNodeImage.size;
</pre>
<pre lang="swift" class = "swiftCode hidden">
let scrollNodeImage = UIImage(named: "image")
let scrollNode = ASScrollNode()

scrollNode.style.preferredSize = CGSize(width: 200.0, height: 200.0)

let scrollNodeView = scrollNode.view
scrollNodeView.addSubview(UIImageView(image: scrollNodeImage))
scrollNodeView.contentSize = scrollNodeImage.size
</pre>
</div>
</div>

As you can see, the `scrollNode`'s underlying view is a `ASScrollNode`.

