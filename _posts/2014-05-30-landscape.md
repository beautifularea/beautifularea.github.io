---
layout: post
title: iOS6.0后的横屏设置
date: 2014-05-30 15:00:00
---

原本的屏幕方向控制的方法
{% highlight Objective-C %}
- (BOOL)shouldAutorotateToInterfaceOrientation:(UIInterfaceOrientation)interfaceOrientation;
{% endhighlight %}
Available in iOS 2.0 and later. Deprecated in iOS 6.0.
所以在iOS6.0后可以使用以下组合方法来实现。

{% highlight Objective-C %}
- (BOOL)shouldAutorotate{
    return YES;
}
- (NSUInteger)supportedInterfaceOrientations{
    return UIInterfaceOrientationMaskLandscape;
}
- (UIInterfaceOrientation)preferredInterfaceOrientationForPresentation{
    return UIInterfaceOrientationLandscapeRight;
}
{% endhighlight %}

在给UIWindow添加root view controller 的时候，使用
{% highlight Objective-C %}
if([self.window respondsToSelector: @selector(setRootViewController:)]){
        self.window.rootViewController = _viewController;
    }
{% endhighlight %}

现在做的小东西就使用了一个UIViewcontroller，其他UINavigationController、UITabBarController等情况没考虑。
横屏后，width和height对调，注意调整坐标！
