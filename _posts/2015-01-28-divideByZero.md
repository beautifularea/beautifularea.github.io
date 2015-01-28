---
layout: post
title: 关于被0除
date: 2015-01-28 10:00:00
---

项目中有一个要计算进度条的需求，必然的要用到了除法，而在除的时候必须要考虑除数为0的情况。延伸一下，如果在计算的过程中需要多个除数，怎么办？每个都去判断是否为0吗，这个显然太繁琐了。
怎么办？看文档！

IEEE floating-point(IEEE 754） 中定义了关于float 除数为0 的情况, 详细的内容可以去找官方的文档，下面就摘录下测试代码。

代码如下：
{% highlight Objective-C %}
    {
//#import <float.h>
//#import <math.h>
        float b = 1.0;
        float c = 1.0;
        float divide = 0.0;
        float a = 1 / (b / divide) + (c / divide);
        NSLog(@"a = %f", a);
        //inf -inf NaN
        BOOL fl = isnan(a);
        if(fl){
            NSLog(@"isnan");
        }
        BOOL fg2 = isinf(a);
        if(fg2){
            NSLog(@"isinf");
        }
        BOOL fg3 = isfinite(a);
        if(fg3){
            NSLog(@"isfinite");
        }
    }
{% endhighlight %}

说到底就是float可以除0，不需要判断所有的除数，只需要对结果进行判断就可以！
以上！

参考:<br/>
<a href="http://en.wikipedia.org/wiki/IEEE_floating_point" rel="external nofollow" target="_blank" class="muted">IEEE-float-point</a><br/>
<a href="http://en.wikipedia.org/wiki/NaN" rel="external nofollow" target="_blank" class="muted">NaN</a>
