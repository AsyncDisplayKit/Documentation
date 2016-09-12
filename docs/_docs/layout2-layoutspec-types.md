---
title: Layout Spec Types
layout: docs
permalink: /docs/layout2-layoutspec-types.html
---

AsyncDisplayKit offers the following ASLayoutSpec subclasses which can be used to compose simple and very complex layouts. 

- <a href = "layout2-layoutspec-types.html#asinsetlayoutspec"><code>ASInsetLayoutSpec</code></a>
- `ASOverlayLayoutSpec`
- `ASBackgroundLayoutSpec`
- `ASCenterLayoutSpec`
- `ASRatioLayoutSpec`
- `ASRelativeLayoutSpec`
- `ASStackLayoutSpec`
- `ASStaticLayoutSpec`

You may also subclass `ASLayoutSpec` in order to make your own, custom layout specs. 

### `ASInsetLayoutSpec`
`ASInsetLayoutSpec` applies an inset margin around its child layoutable. **Note:** the child layoutable must have an instrinsic size or explicitly set its size.

<img src="/static/images/layoutSpec-types/ASInsetLayoutSpec-diagram.png" width="75%">

<div class = "highlight-group">
<span class="language-toggle"><a data-lang="swift" class="swiftButton">Swift</a><a data-lang="objective-c" class = "active objcButton">Objective-C</a></span>

<div class = "code">
<pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  ...
  UIEdgeInsets *insets = UIEdgeInsetsMake(0, HORIZONTAL_BUFFER, 0, HORIZONTAL_BUFFER);
  ASInsetLayoutSpec *headerWithInset = [ASInsetLayoutSpec alloc] initWithInsets:insets child:textNode];
  ...
}
</pre>

<pre lang="swift" class = "swiftCode hidden">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  ...
  let insets = UIEdgeInsetsMake(0, HORIZONTAL_BUFFER, 0, HORIZONTAL_BUFFER);
  let headerWithInset = ASInsetLayoutSpec(insets: insets, child: textNode)
  ...
}
</pre>
</div>
</div>

### ASOverlayLayoutSpec
Lays out a component, stretching another component on top of it as an overlay. **Note:** the underlay layoutable must have an intrinsic size or explictly set a size to it.

<img src="/static/images/layoutSpec-types/ASOverlayLayouSpec-diagram.png" width="65%">

<div class = "highlight-group">
<span class="language-toggle"><a data-lang="swift" class="swiftButton">Swift</a><a data-lang="objective-c" class = "active objcButton">Objective-C</a></span>

<div class = "code">
<pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  ASDisplayNode *backgroundNode = ASDisplayNodeWithBackgroundColor([UIColor blueColor]);
  ASDisplayNode *foregroundNode = ASDisplayNodeWithBackgroundColor([UIColor redColor]
  return [ASOverlayLayoutSpec overlayLayoutSpecWithChild:backgroundNode overlay:foregroundNode]];
}
</pre>

<pre lang="swift" class = "swiftCode hidden">

</pre>
</div>
</div>

Note: A current limitation is that the order in which subnodes are added matters for this layout spec. The overlay layoutable must be added as a subnode to the parent node after the underlaying layoutable. Using ASM does not currently guarantee this order!

### ASBackgroundLayoutSpec
Lays out a component, stretching another component behind it as a backdrop. **Note:** the foreground layoutable must have an intrinsic size.

<img src="/static/images/layoutSpec-types/ASBackgroundLayoutSpec-diagram.png" width="65%">

<div class = "highlight-group">
<span class="language-toggle"><a data-lang="swift" class="swiftButton">Swift</a><a data-lang="objective-c" class = "active objcButton">Objective-C</a></span>

<div class = "code">
<pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  ASDisplayNode *backgroundNode = ASDisplayNodeWithBackgroundColor([UIColor redColor]);
  ASDisplayNode *foregroundNode = ASDisplayNodeWithBackgroundColor([UIColor blueColor]);

  return [ASBackgroundLayoutSpec backgroundLayoutSpecWithChild:foregroundNode background:backgroundNode]];
}
</pre>

<pre lang="swift" class = "swiftCode hidden">

</pre>
</div>
</div>

Note: The order in which subnodes are added matters for this layout spec; the background object must be added as a subnode to the parent node before the foreground object. Using ASM does not currently guarantee this order!

### ASCenterLayoutSpec
Centers a component in the available space. **Note:** The ASCenterLayoutSpec must have an intrinsic size.

<img src="/static/images/layoutSpec-types/ASCenterLayoutSpec-diagram.png" width="65%">

<div class = "highlight-group">
<span class="language-toggle"><a data-lang="swift" class="swiftButton">Swift</a><a data-lang="objective-c" class = "active objcButton">Objective-C</a></span>

<div class = "code">
<pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  ASStaticSizeDisplayNode *subnode = ASDisplayNodeWithBackgroundColor([UIColor greenColor], CGSizeMake(70, 100));
  return [ASCenterLayoutSpec
          centerLayoutSpecWithCenteringOptions:ASCenterLayoutSpecCenteringXY
          sizingOptions:ASRelativeLayoutSpecSizingOptionDefault
          child:subnode]
}
</pre>

<pre lang="swift" class = "swiftCode hidden">

</pre>
</div>
</div>

### ASRatioLayoutSpec
Lays out a component at a fixed aspect ratio (which can be scaled). **Note:** This spec is great for objects that do not have an intrinsic size, such as ASNetworkImageNodes and ASVideoNodes.

<img src="/static/images/layoutSpec-types/ASRatioLayoutSpec-diagram.png" width="65%">

<div class = "highlight-group">
<span class="language-toggle"><a data-lang="swift" class="swiftButton">Swift</a><a data-lang="objective-c" class = "active objcButton">Objective-C</a></span>

<div class = "code">
<pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  // Half Ratio
  ASStaticSizeDisplayNode *subnode = ASDisplayNodeWithBackgroundColor([UIColor greenColor], CGSizeMake(100, 100));
  return [ASRatioLayoutSpec ratioLayoutSpecWithRatio:0.5 child:subnode];
}
</pre>

<pre lang="swift" class = "swiftCode hidden">

</pre>
</div>
</div>

### ASRelativeLayoutSpec
Lays out a component and positions it within the layout bounds according to vertical and horizontal positional specifiers. Similar to the “9-part” image areas, a child can be positioned at any of the 4 corners, or the middle of any of the 4 edges, as well as the center.

<div class = "highlight-group">
<span class="language-toggle"><a data-lang="swift" class="swiftButton">Swift</a><a data-lang="objective-c" class = "active objcButton">Objective-C</a></span>

<div class = "code">
<pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  ASDisplayNode *backgroundNode = ASDisplayNodeWithBackgroundColor([UIColor redColor]);
  ASStaticSizeDisplayNode *foregroundNode = ASDisplayNodeWithBackgroundColor([UIColor greenColor], CGSizeMake(70, 100));

  ASLayoutSpec *layoutSpec =
  [ASBackgroundLayoutSpec
   backgroundLayoutSpecWithChild:
   [ASRelativeLayoutSpec
    relativePositionLayoutSpecWithHorizontalPosition:ASRelativeLayoutSpecPositionStart
    verticalPosition:ASRelativeLayoutSpecPositionStart
    sizingOption:ASRelativeLayoutSpecSizingOptionDefault
    child:foregroundNode]
   background:backgroundNode];

   return layoutSpec;
}
</pre>

<pre lang="swift" class = "swiftCode hidden">

</pre>
</div>
</div>

### ASStackLayoutSpec: Flexbox Container
Of all the layoutSpecs in ASDK,  ASStackLayoutSpec is probably the most useful and the most powerful. `ASStackLayoutSpec` can specify the layout of its children using the flexbox algorithm. Flexbox is designed to provide a consistent layout on different screen sizes. In a stack layout you align items in either a vertical or horizontal stack. A stack layout can be a child of another stack layout, which makes it possible to create almost any layout using a stack layout spec. You will normally use a combination of direction, alignItems, and justifyContent to achieve the right layout.

<div class = "highlight-group">
<span class="language-toggle"><a data-lang="swift" class="swiftButton">Swift</a><a data-lang="objective-c" class = "active objcButton">Objective-C</a></span>

<div class = "code">
<pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  ASStackLayoutSpec *mainStack =
  [ASStackLayoutSpec
   stackLayoutSpecWithDirection:ASStackLayoutDirectionHorizontal
   spacing:6.0
   justifyContent:ASStackLayoutJustifyContentStart
   alignItems:ASStackLayoutAlignItemsCenter
   children:@[_iconNode, _countNode]];

  // Set some constrained size to the stack
  mainStack.minWidth = ASDimensionMakeWithPoints(60.0);
  mainStack.maxHeight = ASDimensionMakeWithPoints(40.0);

  return mainStack;
}
</pre>

<pre lang="swift" class = "swiftCode hidden">

</pre>
</div>
</div>

Flexbox works the same way in AsyncDisplayKit as it does in CSS on the web, with a few exceptions. The defaults are different, there is no `flex` parameter and `flexGrow` and `flexShrink` only supports a boolean value.

### ASStaticLayoutSpec: Absolute Container
Within `ASStaticLayoutSpec` you can specify exact locations (x/y coordinates) of its layoutable children by setting the `layoutPosition` property. Absolute layouts are less flexible and harder to maintain than other types of layouts without absolute positioning.

<div class = "highlight-group">
<span class="language-toggle"><a data-lang="swift" class="swiftButton">Swift</a><a data-lang="objective-c" class = "active objcButton">Objective-C</a></span>

<div class = "code">
<pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  CGSize maxConstrainedSize = constrainedSize.max;

  // Layout all nodes absolute in a static layout spec
  guitarVideoNode.layoutPosition = CGPointMake(0, 0);
  guitarVideoNode.size = ASSizeMakeFromCGSize(CGSizeMake(maxConstrainedSize.width, maxConstrainedSize.height / 3.0));

  nicCageVideoNode.layoutPosition = CGPointMake(maxConstrainedSize.width / 2.0, maxConstrainedSize.height / 3.0);
  nicCageVideoNode.size = ASSizeMakeFromCGSize(CGSizeMake(maxConstrainedSize.width / 2.0, maxConstrainedSize.height / 3.0));

  simonVideoNode.layoutPosition = CGPointMake(0.0, maxConstrainedSize.height - (maxConstrainedSize.height / 3.0));
  simonVideoNode.size = ASSizeMakeFromCGSize(CGSizeMake(maxConstrainedSize.width/2, maxConstrainedSize.height / 3.0));

  hlsVideoNode.layoutPosition = CGPointMake(0.0, maxConstrainedSize.height / 3.0);
  hlsVideoNode.size = ASSizeMakeFromCGSize(CGSizeMake(maxConstrainedSize.width / 2.0, maxConstrainedSize.height / 3.0));

  return [ASStaticLayoutSpec staticLayoutSpecWithChildren:@[guitarVideoNode, nicCageVideoNode, simonVideoNode, hlsVideoNode]];
}
</pre>

<pre lang="swift" class = "swiftCode hidden">

</pre>
</div>
</div>

### ASLayoutSpec
`ASLayoutSpec` is the main class from that all layout spec's are subclassed. It's main job is to handle all the children management, but it also can be used to create custom layout specs. There should be not really a lot of situations where you have to create a custom subclasses of `ASLayoutSpec` though. Instead try to use provided layout specs and compose them together to create more advanced layouts. For examples how to create custom layout spec's look into the already provided layout specs for more details.

Furthermore `ASLayoutSpec` objects can be used as a spacer in a `ASStackLayoutSpec` with other children, when `.flexGrow` and/or `.flexShrink` is applied.

<div class = "highlight-group">
<span class="language-toggle"><a data-lang="swift" class="swiftButton">Swift</a><a data-lang="objective-c" class = "active objcButton">Objective-C</a></span>

<div class = "code">
<pre lang="objc" class="objcCode">
- (ASLayoutSpec *)layoutSpecThatFits:(ASSizeRange)constrainedSize
{
  ...
  // ASLayoutSpec as spacer
  let spacer = ASLayoutSpec()
  spacer.flexGrow = true

  stack.children = [imageNode, spacer, textNode]
  ...
}
</pre>

<pre lang="swift" class = "swiftCode hidden">

</pre>
</div>
</div>