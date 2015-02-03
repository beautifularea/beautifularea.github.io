---
layout: post
title: edgesExtendedForLayout
date: 2015-02-03 10:00:00
---

UIViewController 在iOS 7.0后新加的属性，默认显示为UIRectEdgeAll。
{% highlight Objective-C %}
@property(nonatomic,assign) UIRectEdge edgesForExtendedLayout NS_AVAILABLE_IOS(7_0); // Defaults to UIRectEdgeAll
{% endhighlight %}

如果是一个viewController要从状态栏下开始显示的话，我设置如下：
self.edgesForExtendedLayout = UIRectEdgeNone;
结果，依然是从最顶点开始显示，被状态栏覆盖了。

网上一大堆，blahblahblah,说就这么设置没问题。

当然是有问题的！看文档，找这个属性的解释。
【This property is only applied to view controllers that are embedded in containers】, such as UINavigationController or UITabBarController. View controllers set as the root view controller do not react to this property. Default value is UIRectEdgeAll.

就是这个地方造成的，多看文档嘛！