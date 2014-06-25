---
layout: post
title: sizeof vs.strlen
date: 2014-06-25 17:20:00
---

{% highlight Objective-C %}
char arr[10] = "What?";
int len_one = strlen(arr);
int len_two = sizeof(arr);
NSLog(@"one = %d, two = %d", len_one, len_two);
{% endhighlight %}

输出结果为：5 and 10

sizeof返回定义arr数组时，编译器为其分配的数组空间大小，不关心里面存了多少数据。
strlen只关心存储的数据内容，不关心空间的大小和类型。