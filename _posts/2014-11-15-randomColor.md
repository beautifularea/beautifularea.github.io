---
layout: post
title: 生成随机颜色
date: 2014-11-15 12:00:00
---

{% highlight Objective-C %}
@interface UIColor(RandomColor)
+ (UIColor *)randomColor;
@end
@implementation UIColor(RandomColor)
+ (UIColor *)randomColor{
    CGFloat hue = ( arc4random() % 256 / 256.0 );              //0.0 to 1.0
    CGFloat saturation = ( arc4random() % 128 / 256.0 ) + 0.5; // 0.5 to 1.0,away from white
    CGFloat brightness = ( arc4random() % 128 / 256.0 ) + 0.5; //0.5 to 1.0,away from black
    return [UIColor colorWithHue:hue saturation:saturation brightness:brightness alpha:1];
}
@end
{% endhighlight %}