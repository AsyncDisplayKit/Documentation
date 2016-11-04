---
title: Quickstart
layout: docs
permalink: /docs/layout2-quickstart.html
---

## Motivation & Benefits

The Layout API was created as a performant alternative to UIKit's Auto Layout, which becomes exponentially expensive for complicated view hierarchies. AsyncDisplayKit's Layout API has many benefits over using UIKit's Auto Layout:

- **Fast**: As fast as manual layout code and significantly faster than Auto Layout
- **Asynchronous & Concurrent:** Layouts can be computed on background threads so user interactions are not interrupted. 
- **Declarative**: Layouts are declared with immutable data structures. This makes layout code easier to develop, document, code review, test, debug, profile, and maintain. 
- **Cacheable**: Layout results are immutable data structures so they can be precomputed in the background and cached to increase user perceived performance.
- **Extensible**: Easy to share code between classes. 

## Inspired by CSS Flexbox 

Those who are familiar with Flexbox will notice many similarities in the two systems. However, AsyncDisplayKit's Layout API <a href = "layout2-web-flexbox-differences.html">does not re-implement all of CSS</a>.

## Basic Concepts

AsyncDisplayKit's layout system is centered around two basic concepts: 

1. `ASLayoutSpec`
2. `ASLayoutElements`

### Layout Specs 

A layout spec, short for "layout specification", has no physical presence. Instead, layout specs act as containers for other layout elements by understanding how these children layout elments relate to each other.

AsyncDisplayKit <a hfref = "layout2-layoutspec-types.html">provides several subclasses</a> of `ASLayoutSpec`, from a simple layout specification that insets a single child, to a more complex layout specification that arranges multiple children in varying stack configurations.

### Layout Elements 

Layout specs contain and arrange layout elements. 

All `ASDisplayNode`s and `ASLayoutSpec`s conform to the `<ASLayoutElement>` protocol. This means that you can compose layout specs from both nodes and other layout specs. Cool!

The `ASLayoutElement` protocol has several properties that can be used to create very complex layouts. In addition, layout specs have their own set of properties that can be used to adjust the arrangment of the layout elements. 

### Layout Element Size

Some elements have an "intrinsic size" based on their immediately available content. For example, ASTextNode can calculate its size based on its attributed string. Other nodes that have an intrinsic size include 

- `ASImageNode`
- `ASTextNode`
- `ASButtonNode`
- `ASTextNode` 

All other elmements either do not have an intrinsic size or lack an intrinsic size until their external resource is loaded. For example, an `ASNetworkImageNode` does not know its size until the image is downloaded from the URL. These sorts of elments inlcude 

- `ASVideoNode`
- `ASVideoPlayerNode`
- `ASNetworkImageNode`
- `ASEditableTextNode`

These elements that lack an initial intrinsic size must have an initial size set for them using an `ASRatioLayoutSpec`, an `ASAbsoluteLayoutSpec` or the size properties on the style object. 