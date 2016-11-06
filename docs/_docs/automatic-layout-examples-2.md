---
title: Layout Examples
layout: docs
permalink: /docs/automatic-layout-examples-2.html
---

The following code can be found in the layout [example project](https://github.com/facebook/AsyncDisplayKit/tree/master/examples/LayoutSpecExamples).

## Simple Header with Left and Right Justified Text

<img src="/static/images/layout-examples-simple-header-with-left-right-justified-text.png">

To create this layout, we will use a:

- a vertical `ASStackLayoutSpec`
- a horizontal `ASStackLayoutSpec`
- `ASInsetLayoutSpec` to inset the entire header

The diagram below shows the composition of the layout elements (nodes + layout specs). 

<img src="/static/images/layout-examples-simple-header-with-left-right-justified-text-diagram.png">

<div class = "highlight-group">
<span class="language-toggle">
  <a data-lang="swift" class="swiftButton">Swift</a>
  <a data-lang="objective-c" class = "active objcButton">Objective-C</a>
</span>
<div class = "code">
  <pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  // when the username / location text is too long, 
  // shrink the stack to fit onscreen rather than push content to the right, offscreen
  ASStackLayoutSpec *nameLocationStack = [ASStackLayoutSpec verticalStackLayoutSpec];
  nameLocationStack.style.flexShrink = YES;
  nameLocationStack.style.flexGrow = YES;
  
  // if fetching post location data from server, 
  // check if it is available yet and include it if so
  if (_postLocationNode.attributedText) {
    nameLocationStack.children = @[_usernameNode, _postLocationNode];
  } else {
    nameLocationStack.children = @[_usernameNode];
  }
  
  // horizontal stack
  ASStackLayoutSpec *headerStackSpec = [ASStackLayoutSpec stackLayoutSpecWithDirection:ASStackLayoutDirectionHorizontal
                                                                               spacing:40
                                                                        justifyContent:ASStackLayoutJustifyContentStart
                                                                            alignItems:ASStackLayoutAlignItemsCenter
                                                                              children:@[nameLocationStack, _postTimeNode]];
  
  // inset the horizontal stack
  return [ASInsetLayoutSpec insetLayoutSpecWithInsets:UIEdgeInsetsMake(0, 10, 0, 10) child:headerStackSpec];
}
  </pre>
  <pre lang="swift" class = "swiftCode hidden">
  </pre>
</div>
</div>

Rotate the example project from portrait to landscape to see how the spacer grows and shrinks.

## Photo with Inset Text Overlay

<img src="/static/images/layout-examples-photo-with-inset-text-overlay.png">

To create this layout, we will use a:

- `ASInsetLayoutSpec` to inset the text
- `ASOverlayLayoutSpec` to overlay the inset text spec on top of the photo

<div class = "highlight-group">
<span class="language-toggle">
  <a data-lang="swift" class="swiftButton">Swift</a>
  <a data-lang="objective-c" class = "active objcButton">Objective-C</a>
</span>
<div class = "code">
  <pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  _photoNode.style.preferredSize = CGSizeMake(USER_IMAGE_HEIGHT*2, USER_IMAGE_HEIGHT*2);

  // INIFINITY is used to make the inset unbounded
  UIEdgeInsets insets = UIEdgeInsetsMake(INFINITY, 12, 12, 12);
  ASInsetLayoutSpec *textInsetSpec = [ASInsetLayoutSpec insetLayoutSpecWithInsets:insets child:_titleNode];

  ASOverlayLayoutSpec *textOverlaySpec = [ASOverlayLayoutSpec overlayLayoutSpecWithChild:_photoNode
                                                                                 overlay:textInsetSpec];
  
  return textOverlaySpec;
}
  </pre>
  <pre lang="swift" class = "swiftCode hidden">
  </pre>
</div>
</div>

## Photo with Outset Icon Overlay

<img src="/static/images/layout-examples-photo-with-outset-icon-overlay.png">

To create this layout, we will use a:

- `ASAbsoluteLayoutSpec` to place the photo and icon which have been individually sized and positioned using their `ASLayoutable` properties

<div class = "highlight-group">
<span class="language-toggle">
  <a data-lang="swift" class="swiftButton">Swift</a>
  <a data-lang="objective-c" class = "active objcButton">Objective-C</a>
</span>
<div class = "code">
  <pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  _iconNode.style.preferredSize = CGSizeMake(40, 40);
  _iconNode.style.layoutPosition = CGPointMake(150, 0);
  
  _photoNode.style.preferredSize = CGSizeMake(150, 150);
  _photoNode.style.layoutPosition = CGPointMake(40 / 2.0, 40 / 2.0);
  
  return [ASAbsoluteLayoutSpec absoluteLayoutSpecWithSizing:ASAbsoluteLayoutSpecSizingSizeToFit
                                                   children:@[_photoNode, _iconNode]];
}
  </pre>
  <pre lang="swift" class = "swiftCode hidden">
  </pre>
</div>
</div>



## Simple Inset Text Cell

<img src="/static/images/layout-examples-simple-inset-text-cell.png" width="40%">

To recreate the layout of a <i>single cell</i> as is used in Pinterest's search view above, we will use a:

- `ASInsetLayoutSpec` to inset the text
- `ASCenterLayoutSpec` to center the text according to the specified properties

<div class = "highlight-group">
<span class="language-toggle">
  <a data-lang="swift" class="swiftButton">Swift</a>
  <a data-lang="objective-c" class = "active objcButton">Objective-C</a>
</span>
<div class = "code">
  <pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
    UIEdgeInsets insets = UIEdgeInsetsMake(0, 12, 4, 4);
    ASInsetLayoutSpec *inset = [ASInsetLayoutSpec insetLayoutSpecWithInsets:insets
                                                                      child:_titleNode];

    return [ASCenterLayoutSpec centerLayoutSpecWithCenteringOptions:ASCenterLayoutSpecCenteringY
                                                      sizingOptions:ASCenterLayoutSpecSizingOptionMinimumX
                                                              child:inset];
}
  </pre>
  <pre lang="swift" class = "swiftCode hidden">
  </pre>
</div>
</div>

## Top and Bottom Separator Lines

<img src="/static/images/layout-examples-top-bottom-separator-line.png">

To create the layout above, we will use a:

- a `ASInsetLayoutSpec` to inset the text
- a vertical `ASStackLayoutSpec` to stack the two separator lines on the top and bottom of the text

The diagram below shows the composition of the layoutables (layout specs + nodes). 

<img src="/static/images/layout-examples-top-bottom-separator-line-diagram.png">

The following code can also be found in the `ASLayoutSpecPlayground` [example project]().

<div class = "highlight-group">
<span class="language-toggle">
  <a data-lang="swift" class="swiftButton">Swift</a>
  <a data-lang="objective-c" class = "active objcButton">Objective-C</a>
</span>
<div class = "code">
  <pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  _topSeparator.style.flexGrow = YES;
  _bottomSeparator.style.flexGrow = YES;

  ASInsetLayoutSpec *insetContentSpec = [ASInsetLayoutSpec insetLayoutSpecWithInsets:UIEdgeInsetsMake(20, 20, 20, 20)
                                                                               child:_textNode];
  return [ASStackLayoutSpec stackLayoutSpecWithDirection:ASStackLayoutDirectionVertical
                                                 spacing:0
                                          justifyContent:ASStackLayoutJustifyContentCenter
                                              alignItems:ASStackLayoutAlignItemsStretch
                                                children:@[_topSeparator, insetContentSpec, _bottomSeparator]];
}
  </pre>
  <pre lang="swift" class = "swiftCode hidden">
  </pre>
</div>
</div>
