---
layout: post
title: iOS本地化
date: 2014-11-24 10:00:00
---

{% highlight Objective-C %}
+ (NSString*)localizeStringWithKey:(NSString *)key table:(NSString *)tableName{
	NSString *path_ = [[NSBundle mainBundle] pathForResource:@"zh-Hans" ofType:@"lproj"];
	NSBundle *bundle = [NSBundle bundleWithPath:path];
	NSString *string = [bundle localizedStringForKey:key value:@"" table:tableName];
	if (string==nil) {
	    return key;
	}
	return string;
}
{% endhighlight %}