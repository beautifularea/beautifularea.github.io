---
layout: post
title: 删除数组中的Object
date: 2014-12-30 12:00:00
---

在某些条件下，需要删除数组中符合条件的对象，如果遍历删除，有可能出现问题，可以考虑下面这个方法:<br/>
{% highlight Objective-C %}
NSMutableArray *sortedArray = [[NSMutableArray alloc] initWithObjects: @"1" , @"1",@"2",nil];
[sortedArray enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
    NSString *str = (NSString *)obj;
    if([str isEqualToString: @"1"]){
        [sortedArray removeObject: obj];
    }
}];
{% endhighlight %}