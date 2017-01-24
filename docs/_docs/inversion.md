---
title: Inversion
layout: docs
permalink: /docs/inversion.html
prevPage: placeholder-fade-duration.html
nextPage: layer-backing.html
---

`ASTableNode` and `ASCollectionNode` have a `inverted` property of type `BOOL` that when set to YES, will automatically invert the content so that it's layed out bottom to top, that is the 'first' (indexPath 0, 0) node  is at the bottom rather than the top as usual. This is extremely covenient for chat/messaging apps, and with AsyncDisplayKit it only take one property.

When this is enabled, developers only have to take one more step to have full inversion support and that is to adjust the `contentInset` of their `ASTableNode`/`ASCollectionNode` like so:

```
 CGFloat inset = [self topBarsHeight];
 self.tableNode.view.contentInset = UIEdgeInsetsMake(0, 0, inset, 0);
 self.tableNode.view.scrollIndicatorInsets = UIEdgeInsetsMake(0, 0, inset, 0);
```

See the SocialAppLayout-Inverted example for more details.
