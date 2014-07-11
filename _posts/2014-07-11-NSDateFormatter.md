---
layout: post
title: NSDateFormatter用法
date: 2014-07-11 16:15:00
---

NSDateFormatter用法
{% highlight Objective-C %}NSDateFormatter *formatter = [[NSDateFormatter alloc] init];
[formatter setDateFormat: @"yyyy-MM-dd HH:mm:ss"];
NSString *curFormatter = [formatter stringFromDate:[NSDate date]];
//show
[curFormatter release];{% endhighlight %}