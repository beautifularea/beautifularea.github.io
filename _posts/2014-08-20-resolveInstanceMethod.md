---
layout: post
title: resolveInstanceMethod
date: 2014-08-20 10:30:00
---

From Reference:<br/>
Dynamically provides an implementation for a given selector for an instance method.

{% highlight Objective-C %}
+ (BOOL)resolveInstanceMethod:(SEL)name
{% endhighlight %}

Parameters<br/>
name<br/>
The name of a selector to resolve.<br/>
Return Value<br/>
YES if the method was found and added to the receiver, otherwise NO.<br/>

Discussion<br/>
This method and resolveClassMethod: allow you to dynamically provide an implementation for a given selector.<br/>

An Objective-C method is simply a C function that take at least two arguments—self and _cmd. Using the class_addMethod function, you can add a function to a class as a method. Given the following function:<br/>

{% highlight Objective-C %}
void dynamicMethodIMP(id self, SEL _cmd)
{
    // implementation ....
}
{% endhighlight %}

you can use resolveInstanceMethod: to dynamically add it to a class as a method (called resolveThisMethodDynamically) like this:

{% highlight Objective-C %}
+ (BOOL) resolveInstanceMethod:(SEL)aSEL
{
    if (aSEL == @selector(resolveThisMethodDynamically))
    {
          class_addMethod([self class], aSEL, (IMP) dynamicMethodIMP, "v@:");
          return YES;
    }
    return [super resolveInstanceMethod:aSel];
}
{% endhighlight %}

Special Considerations
This method is called before the Objective-C forwarding mechanism is invoked. If respondsToSelector: or instancesRespondToSelector: is invoked, the dynamic method resolver is given the opportunity to provide an IMP for the given selector first.

eg.
{% highlight Objective-C %}
[self resolveThisMethodDynamically];
void dynamicMethodIMP(id self, SEL _cmd) {
    // implementation ....
    NSLog(@"%s", __FUNCTION__);
}
+ (BOOL)resolveInstanceMethod:(SEL)aSEL
{
    if (aSEL == @selector(resolveThisMethodDynamically)) {
        class_addMethod([self class], aSEL, (IMP) dynamicMethodIMP, "v@:");
        return YES;
    }
    return [super resolveInstanceMethod:aSEL];
}
{% endhighlight %}

动态添加类方法:<br/>
{% highlight Objective-C %}
Dynamically provides an implementation for a given selector for a class method.
+ (BOOL)resolveClassMethod:(SEL)name
{% endhighlight %}