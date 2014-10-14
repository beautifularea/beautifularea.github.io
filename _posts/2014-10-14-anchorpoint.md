---
layout: post
title: CALayer anchor point
date: 2014-10-14 10:00:00
---

CALayer 的 anchorPoint 和 position的关系

如图<br/>
<image src="/images/layer_anchor_point_1.png" />
<br/>

<image src="/images/layer_anchor_point_2.jpg" />
<br/>

anchorPoint的默认值为(0.5,0.5)，也就是anchorPoint默认在layer的中心点。

{% highlight C %}
position.x = frame.origin.x + anchorPoint.x * bounds.size.width；  
position.y = frame.origin.y + anchorPoint.y * bounds.size.height；
{% endhighlight %}