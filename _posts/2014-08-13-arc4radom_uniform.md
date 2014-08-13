---
layout: post
title: arc4random_uniform
date: 2014-08-13 17:00:00
---

{% highlight Objective-C %}
u_int32_t
arc4random_uniform(u_int32_t /*upper_bound*/) __OSX_AVAILABLE_STARTING(__MAC_10_7, __IPHONE_4_3);
{% endhighlight %}

/*
 arc4random_uniform() will return a uniformly distributed random number less than upper_bound.
 arc4random_uniform() is recommended over constructions like ``arc4random() % upper_bound'' as it avoids "modulo
 bias" when the upper bound is not a power of two.
 */
{% highlight Objective-C %}
int rd = arc4random_uniform(10);
NSLog(@"rd = %d", rd);
{% endhighlight %}
<br/>
查看命令<br/>
{% highlight Objective-C %}
man arc4random_uniform
{% endhighlight %}