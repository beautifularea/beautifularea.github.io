---
layout: post
title: 消除方法deprecated-warning
date: 2014-08-14 17:40:00
---

{% highlight Objective-C %}
#pragma clang diagnostic push
#pragma clang diagnostic ignored "-Wdeprecated-declarations"
//Available in iOS 2.0 and later. Deprecated in iOS 7.0.
[@"content" sizeWithFont: [UIFont systemFontOfSize: 12.0]];
#pragma clang diagnostic pop
{% endhighlight %}