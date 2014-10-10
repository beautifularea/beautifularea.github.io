---
layout: post
title: makeObjectsPerformSelector
date: 2014-10-10 11:00:00
---

NSArray
{% highlight Objective-C %}
- (void)makeObjectsPerformSelector:(SEL)aSelector withObject:(id)anObject
{% endhighlight %}
Description	<br/>
Sends the aSelector message to each object in the array, starting with the first object and continuing through the array to the last object.<br/>
This method raises an NSInvalidArgumentException if aSelector is NULL.<br/>
Parameters	<br/>
aSelector	<br/>
A selector that identifies the message to send to the objects in the array. The method must take a single argument of type id, and must not have the side effect of modifying the receiving array.<br/>
anObject	<br/>
The object to send as the argument to each invocation of the aSelector method.<br/>
<br/>
{% highlight Objective-C %}
// Draw the cars
[cars makeObjectsPerformSelector:@selector(drawWithBaseEffect:)
                      withObject:self.baseEffect];
{% endhighlight %}