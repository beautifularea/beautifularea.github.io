---
layout: post
title: 对中文数组排序
date: 2014-12-27 16:00:00
---

项目中要对中文城市名进行排序显示。实现如下：<br/>
{% highlight Objective-C %}
NSInteger sortCitys(id user1, id user2, void *context){
    MWO_ADAreaInfo *info1;
    MWO_ADAreaInfo *info2;
    info1 = (MWO_ADAreaInfo *)user1;
    info2 = (MWO_ADAreaInfo *)user2;
    return  [info1.szName localizedCompare: info2.szName];
    NSDictionary *u1,*u2;
    //类型转换
    u1 = (NSDictionary*)user1;
    u2 = (NSDictionary*)user2;
    return  [[u1 valueForKey: @"szName"] localizedCompare:[u2 valueForKey: @"szName"]];
}
{% endhighlight %}

{% highlight Objective-C %}
//对搜索进行排序
NSMutableArray *sortedArray = [unsortedArray sortedArrayUsingFunction: sortCitys context:NULL];
{% endhighlight %}