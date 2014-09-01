---
layout: post
title: tail recursive
date: 2014-09-01 10:00:00
---

举个栗子<br/>
{% highlight Objective-C %}
/*
 1-n sum
 */
- (int)trail_sum: (int)sum : (int)n{
    if(0 == n){
        return  sum;
    }
    else{
        return [self trail_sum: sum + n : n-1];
    }
}
{% endhighlight %}

{% highlight Objective-C %}
/*
 四叉树节点个数
 */
- (int)trail_recursive: (int)depth sum: (int)sum{
    if(0 == depth){
        return 1 + sum;
    }
    else{
        return [self trail_recursive: depth-1 sum: sum + pow(4, depth)];
    }    
    return 0;
}
{% endhighlight %}

{% highlight Objective-C %}
-module(test).
-export([fac/1]).
fac(0, Sum) -> Sum;
fac(N, Sum) -> fac(N - 1, N * Sum).
fac(N) -> fac(N, 1).
{% endhighlight %}