---
layout: post
title: BASingleton
date: 2014-12-27 11:00:00
---

兼顾了ARC / 非ARC 的宏定义。

{% highlight Objective-C %}
#ifndef Trial_BASingleton_h
#define Trial_BASingleton_h

// ## : connect string and param
#define singleton_h(name) + (instancetype)shared##name;

#if __has_feature(obj_arc)  //ARC
#define singleton_m(name) \
static id _instance; \
+ (id)allocWithZone:(struct _NSZone *)zone \
{ \
static dispatch_once_t onceToken; \
dispatch_once(&onceToken, ^{ \
_instance = [super allocWithZone:zone]; \
}); \
return _instance; \
} \
\
+ (instancetype)shared##name \
{ \
static dispatch_once_t onceToken; \
dispatch_once(&onceToken, ^{ \
_instance = [[self alloc] init]; \
}); \
return _instance; \
} \
\
+ (id)copyWithZone:(struct _NSZone *)zone \
{ \
return _instance; \
}

#else //NON-ARC
#define singleton_m(name) \
static id _instance; \
+ (id)allocWithZone:(struct _NSZone *)zone \
{ \
static dispatch_once_t onceToken; \
dispatch_once(&onceToken, ^{ \
_instance = [super allocWithZone:zone]; \
}); \
return _instance; \
} \
\
+ (instancetype)shared##name \
{ \
static dispatch_once_t onceToken; \
dispatch_once(&onceToken, ^{ \
_instance = [[self alloc] init]; \
}); \
return _instance; \
} \
\
- (oneway void)release \
{ \
\
} \
\
- (id)autorelease \
{ \
return _instance; \
} \
\
- (id)retain \
{ \
return _instance; \
} \
\
- (NSUInteger)retainCount \
{ \
return 1; \
} \
\
+ (id)copyWithZone:(struct _NSZone *)zone \
{ \
return _instance; \
}
#endif

#endif
{% endhighlight %}

使用:<br/>

{% highlight Objective-C %}
#import "BASingleton.h"

@interface BADataManager : NSObject
singleton_h(BADataManager)
@end

@implementation BADataManager

singleton_m(BADataManager)

-(id)init
{
    static id obj=nil;
    static dispatch_once_t onceToken;
    dispatch_once(&onceToken, ^{
    if ((obj=[super init]) != nil) {
    }
    }); //end block
    return self;
}
@end
{% endhighlight %}


