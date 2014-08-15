---
layout: post
title: NSStringFromCGPoint
date: 2014-08-15 17:30:00
---

打印坐标点时候不用每次都 point.x, point.y,使用方法：
{% highlight Objective-C %}
NSString *pointofstring = NSStringFromCGPoint(CGPoint point);
{% endhighlight %}

同理还有：
{% highlight Objective-C %}
NSString *rectofstring  = NSStringFromCGRect(CGRect rect);
NSString *sizeofstring = NSStringFromCGSize(CGSize size);
{% endhighlight %}