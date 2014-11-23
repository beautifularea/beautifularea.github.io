---
layout: post
title: LLDB - watchpoint
date: 2014-11-23 11:00:00
---

watchpoint:
用来监视一块内存，当数据发生变化时候，触发！<br/>
比如一下demo，来监视son‘name ，当son释放时候，name发生了变化，触发！

{% highlight Objective-C %}
{
    Son *son = [Son new];
    son.name = @"jerk";
    son.toys = @[@"pig", @"dog"];
	
    int x = 1; //breakpoint - 1
}

int x = 1; //breakpoint - 2
{% endhighlight %}

breakpoint - 1 处， 在console输入：<br />
{% highlight Objective-C %}
watchpoint set variable son->_name
{% endhighlight %}

console显示为：<br />
{% highlight Objective-C %}
Watchpoint created: Watchpoint 1: addr = 0x7bf86e14 size = 4 state = enabled type = w
declare @ '/Users/zhTian/Desktop/Destruction/Destruction/ViewController.m:25'
watchpoint spec = 'son->_name'
new value: 0x000d809c
{% endhighlight %}

运行到breakpoint - 2处，console 显示：<br />
{% highlight Objective-C %}
Watchpoint 1 hit:
old value: 0x000d809c
new value: 0x00000000   //此时，son 释放， name变为nil
{% endhighlight %}