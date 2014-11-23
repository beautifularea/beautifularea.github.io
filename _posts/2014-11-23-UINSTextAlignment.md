---
layout: post
title: 消除textAlignment的警告
date: 2014-11-23 15:00:00
---

{% highlight Objective-C %}
#ifdef __IPHONE_6_0
#define UILineBreakModeWordWrap                          NSLineBreakByWordWrapping
#define UILineBreakModeCharacterWrap                     NSLineBreakByCharWrapping
#define UILineBreakModeClip                              NSLineBreakByClipping
#define UILineBreakModeHeadTruncation                    NSLineBreakByTruncatingHead
#define UILineBreakModeTailTruncation                    NSLineBreakByTruncatingTail
#define UILineBreakModeMiddleTruncation                  NSLineBreakByTruncatingMiddle
#define UITextAlignmentLeft                              NSTextAlignmentLeft
#define UITextAlignmentCenter                            NSTextAlignmentCenter
#define UITextAlignmentRight                             NSTextAlignmentRight
#endif   // #ifdef __IPHONE_6_0
{% endhighlight %}

这段宏巧妙的解决了UILable 的textAlignment 和 lineBreakMode 在不同的os版本中出现的警告问题。