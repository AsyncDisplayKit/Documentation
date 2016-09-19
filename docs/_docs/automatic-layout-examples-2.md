---
title: Layout Examples
layout: docs
permalink: /docs/automatic-layout-examples-2.html
---

Scroll down to see the `layoutSpecThatFits:` code for the following examples. 

<table class="paddingBetweenColsNoColor">
  <tr>
    <td wdith="30%"><img src="/static/images/layout-examples-photo-with-inset-text-overlay.png" width="35%"></td>
    <td>Photo with inset text overlay</td> 
  </tr>
  <tr>
    <td wdith="30%"><img src="/static/images/layout-examples-photo-with-outset-icon-overlay.png" width="35%"></td>
    <td>Photo with outset icon overlay (requires sizing and positioning of each element)</td> 
  </tr>
  <tr>
    <td wdith="30%"><img src="/static/images/layout-examples-simple-header-with-left-right-justified-text.png" width="75%"></td>
    <td>Header with left and right justified text</td> 
  </tr>
  <tr>
    <td wdith="30%"><img src="/static/images/layout-examples-simple-inset-text-cell-closeup.png" width="60%"></td>
    <td>Simple inset text cell</td> 
  </tr>
  <tr>
    <td wdith="30%"><img src="/static/images/layout-examples-top-bottom-separator-line.png" width="60%"></td>
    <td>Top and bottom separator lines that grow with the center content</td> 
  </tr>
</table> 

<br>

## Photo with Inset Text Overlay

<img src="/static/images/layout-examples-photo-with-inset-text-overlay.png">

To create this layout, we will use a:

- `ASStaticLayoutSpec` to size the image node
- `ASInsetLayoutSpec` to inset the text
- `ASOverlayLayoutSpec` to overlay the inset text spec on top of the photo

The diagram below shows the composition of the layoutables (layout specs + nodes). 

<img src="/static/images/layout-examples-photo-with-inset-text-overlay-diagram.png">

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
  _photoNode.preferredFrameSize = CGSizeMake(USER_IMAGE_HEIGHT*2, USER_IMAGE_HEIGHT*2);
  <b>ASStaticLayoutSpec</b> *backgroundImageStaticSpec = [<b>ASStaticLayoutSpec</b> staticLayoutSpecWithChildren:@[_photoNode]];

  UIEdgeInsets insets = UIEdgeInsetsMake(INFINITY, 12, 12, 12);
  <b>ASInsetLayoutSpec</b> *textInsetSpec = [<b>ASInsetLayoutSpec</b> insetLayoutSpecWithInsets:insets child:_titleNode];

  <b>ASOverlayLayoutSpec</b> *textOverlaySpec = [<b>ASOverlayLayoutSpec</b> overlayLayoutSpecWithChild:backgroundImageStaticSpec
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

- `ASStaticLayoutSpec` to place the photo and icon which have been individually sized and positioned using their `ASLayoutable` properties

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
  _iconNode.preferredFrameSize = CGSizeMake(40, 40);
  _photoNode.preferredFrameSize = CGSizeMake(150, 150);

  CGFloat x = _photoNode.preferredFrameSize.width;
  CGFloat y = 0;
  
  _iconNode.layoutPosition = CGPointMake(x, y);
  _photoNode.layoutPosition = CGPointMake(_iconNode.preferredFrameSize.height/2.0, _iconNode.preferredFrameSize.height/2.0);
  
  <b>ASStaticLayoutSpec</b> *staticLayoutSpec = [<b>ASStaticLayoutSpec</b> staticLayoutSpecWithChildren:@[_photoNode, _iconNode]];
  
  return staticLayoutSpec;
}
  </pre>
  <pre lang="swift" class = "swiftCode hidden">
  </pre>
</div>
</div>

## Simple Header with Left and Right Justified Text

<img src="/static/images/layout-examples-simple-header-with-left-right-justified-text.png">

To create this layout, we will use a:

- one horizontal and one vertical `ASStackLayoutSpec`
- `ASLayoutSpec` as a flexible spacer between the left and right sides
- `ASInsetLayoutSpec` to inset the entire header

The diagram below shows the composition of the layoutables (layout specs + nodes). 

<img src="/static/images/layout-examples-simple-header-with-left-right-justified-text-diagram.png">

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
  _usernameNode.flexShrink = YES;
  _postLocationNode.flexShrink = YES;

  <b>ASStackLayoutSpec</b> *verticalStackSpec = [<b>ASStackLayoutSpec</b> verticalStackLayoutSpec];
  verticalStackSpec.flexShrink = YES;
  
  // if fetching post location data from server, check if it is available yet
  if (_postLocationNode.attributedString) {
    [verticalStackSpec setChildren:@[_usernameNode, _postLocationNode]];
  } else {
    [verticalStackSpec setChildren:@[_usernameNode]];
  }
  
  <b>ASLayoutSpec</b> *spacerSpec = [[<b>ASLayoutSpec</b> alloc] init];
  spacerSpec.flexGrow = YES;
  spacerSpec.flexShrink = YES;
  
  // horizontal stack
  <b>ASStackLayoutSpec</b> *horizontalStackSpec = [<b>ASStackLayoutSpec</b> horizontalStackLayoutSpec];
  horizontalStackSpec.alignItems = ASStackLayoutAlignItemsCenter; // center items vertically in horiz stack
  horizontalStackSpec.justifyContent = ASStackLayoutJustifyContentStart; // justify content to left
  horizontalStackSpec.flexShrink = YES;
  horizontalStackSpec.flexGrow = YES;
  [horizontalStackSpec setChildren:@[verticalStackSpec, spacerSpec, _postTimeNode]];
  
  // inset horizontal stack
  UIEdgeInsets insets = UIEdgeInsetsMake(0, 10, 0, 10);
  <b>ASInsetLayoutSpec</b> *headerInsetSpec = [<b>ASInsetLayoutSpec</b> insetLayoutSpecWithInsets:insets child:horizontalStackSpec];
  headerInsetSpec.flexShrink = YES;
  headerInsetSpec.flexGrow = YES;
  
  return headerInsetSpec;
}
  </pre>
  <pre lang="swift" class = "swiftCode hidden">
  </pre>
</div>
</div>

## Simple Inset Text Cell

<img src="/static/images/layout-examples-simple-inset-text-cell.png" width="40%">

To recreate the layout of a <i>single cell</i> from the table above (example is Pinterest's search view), we will use a:

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
    <b>ASInsetLayoutSpec</b> *inset = [<b>ASInsetLayoutSpec</b> insetLayoutSpecWithInsets:insets
                                                                      child:_titleNode];

    <b>ASCenterLayoutSpec</b> *centerSpec = [<b>ASCenterLayoutSpec</b> centerLayoutSpecWithCenteringOptions:ASCenterLayoutSpecCenteringY
                                                                                sizingOptions:ASCenterLayoutSpecSizingOptionMinimumX
                                                                                        child:inset];
    return centerSpec;
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
  _topSeparator.flexGrow = YES;
  _bottomSeparator.flexGrow = YES;

  UIEdgeInsets contentInsets = UIEdgeInsetsMake(10, 10, 10, 10);
  ASInsetLayoutSpec *insetContentSpec = [ASInsetLayoutSpec insetLayoutSpecWithInsets:contentInsets
                                                                               child:_textNode];
  // final vertical stack
  ASStackLayoutSpec *verticalStackSpec = [ASStackLayoutSpec verticalStackLayoutSpec];
  verticalStackSpec.direction = ASStackLayoutDirectionVertical;
  verticalStackSpec.justifyContent = ASStackLayoutJustifyContentCenter;
  verticalStackSpec.alignItems = ASStackLayoutAlignItemsStretch;
  verticalStackSpec.children = @[_topSeparator, insetContentSpec, _bottomSeparator];

  return verticalStackSpec;
}
  </pre>
  <pre lang="swift" class = "swiftCode hidden">
  </pre>
</div>
</div>
