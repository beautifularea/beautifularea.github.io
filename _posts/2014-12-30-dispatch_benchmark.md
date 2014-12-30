---
layout: post
title: 使用dispatch_benchmark方法测试运行时间
date: 2014-12-30 10:00:00
---

在做室内地图时候，绘制的时候有耗时比较大的地方，就要看一下部分代码运行时间，把他们找出来。
如下：

{% highlight Objective-C %}
static size_t const count = 1000;
static size_t const iterations = 10000;
id object = @"🐷";
{% endhighlight %}

{% highlight Objective-C %}
CFTimeInterval startTime = CACurrentMediaTime();
{
    for (size_t i = 0; i < iterations; i++) {
        @autoreleasepool {
            NSMutableArray *mutableArray = [NSMutableArray array];
            for (size_t j = 0; j < count; j++) {
                [mutableArray addObject:object];
            }
        }
    }
}
CFTimeInterval endTime = CACurrentMediaTime();
NSLog(@"Total Runtime: %g s", endTime - startTime);
{% endhighlight %}

略麻烦，但是还有更简洁的方法，（为毛那时候不知道😂）
使用：dispatch_benchmark方法,这个方法没有公开声明，使用前，需要声明下：
{% highlight Objective-C %}
extern uint64_t dispatch_benchmark(size_t count, void (^block)(void));
{% endhighlight %}
可以使用man 来查看方法定义。

{% highlight Objective-C %}
uint64_t t = dispatch_benchmark(iterations, ^{
    @autoreleasepool {
        NSMutableArray *mutableArray = [NSMutableArray array];
        for (size_t i = 0; i < count; i++) {
            [mutableArray addObject:object];
        }
    }
});
NSLog(@"[[NSMutableArray array] addObject:] Avg. Runtime: %llu ns", t);
{% endhighlight %}

剩下的就大胆的用吧。

参考：<br/>
<a href="http://libdispatch.macosforge.org/" rel="external nofollow" target="_blank" class="muted">libdispatch</a><br/>
<a href="http://nshipster.cn/benchmarking/" rel="external nofollow" target="_blank" class="muted">benchmarking</a>