---
layout: post
title: NSCParameterAssert
date: 2014-09-29 10:30:00
---

{% highlight Objective-C %}
#import <Foundation/NSException.h>
#define NSCParameterAssert(condition) NSCAssert((condition), @"Invalid parameter not satisfying: %s", #condition)
static NSData *AGLKDataWithResizedCGImageBytes(
   CGImageRef cgImage,
   size_t *widthPtr,
   size_t *heightPtr)
{
   NSCParameterAssert(NULL != cgImage);
   NSCParameterAssert(NULL != widthPtr);
   NSCParameterAssert(NULL != heightPtr);
}
{% endhighlight %}

{% highlight Objective-C %}
NSCAssert(0 < originalWidth, @"Invalid image width");
NSCAssert(0 < originalHeight, @"Invalid image width");
{% endhighlight %}