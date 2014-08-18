---
layout: post
title: 3个Erlang list的函数
date: 2014-08-18 13:00:00
---

1、<br/>
{% highlight Objective-C %}
seq(From, To) -> Seq
{% endhighlight %}
其中From和To都是整型，这个函数返回一个从From到To的一个整型列表。
例子
{% highlight Objective-C %}
lists:seq(1,10).
{% endhighlight %}
结果
{% highlight Objective-C %}
[1,2,3,4,5,6,7,8,9,10]
{% endhighlight %}

2、<br/>
{% highlight Objective-C %}
foldl(Fun, Acc0, List) -> Acc1
{% endhighlight %}
Fun这个函数有两个参数
第一个参数是List中的元素，第二个参数是Fun函数执行完后的返回值，这个参数第一次执行时
就是Acc0
例子：对[1,2,3,4,5]求和
{% highlight Objective-C %}
lists:foldl(fun(X, Sum) -> X + Sum end, 0, [1,2,3,4,5]).
{% endhighlight %}
结果：15
执行过程：首先，Fun第一次执行时，X的值取列表List的第一个元素1，Sum取0,
  Fun第二次执行时，X的值取列表List的第二个元素2，Sum取Fun第一次的返回值
  依次轮推，直到List中每个元素执行完，最后foldl返回最后一次的结果。

3、<br/>
{% highlight Objective-C %}
foldr(Fun, Acc0, List) -> Acc1
{% endhighlight %}
foldr这个函数和foldl比较相似
不过是Fun执行时，X的值先取List的最后一个，然后取倒数第二个。



