---
layout: post
title: Get UIGestureRecognizer info
date: 2014-06-25 16:50:00
---

获取UIGestureRecognizer target 和 action。

{% highlight Objective-C %}
#import <objc/runtime.h>

UITapGestureRecognizer *tap = [[UITapGestureRecognizer alloc] initWithTarget: self action: @selector(tapaction)];
[self.lableView addGestureRecognizer: tap];
[tap release];

for(UIGestureRecognizer *gesture in self.lableView.gestureRecognizers){
		Ivar targetsIvar = class_getInstanceVariable([UIGestureRecognizer class], "_targets");
        id targetActionPairs = object_getIvar(gesture, targetsIvar);
        Class targetActionPairClass = NSClassFromString(@"UIGestureRecognizerTarget");
        Ivar targetIvar = class_getInstanceVariable(targetActionPairClass, "_target");
        Ivar actionIvar = class_getInstanceVariable(targetActionPairClass, "_action");
        for (id targetActionPair in targetActionPairs)
        {
            id target = object_getIvar(targetActionPair, targetIvar);
            SEL action = (__bridge void *)object_getIvar(targetActionPair, actionIvar);
            NSLog(@"target=%@; action=%@", target, NSStringFromSelector(action));
        }
}
{% endhighlight %}

{% highlight Objective-C %}
UIGestureRecognizerTarget.h

__attribute__((visibility("hidden")))
@interface UIGestureRecognizerTarget : NSObject {
@private
	id _target;
	SEL _action;
}
// inherited: -(id)description;
@end
{% endhighlight %}

