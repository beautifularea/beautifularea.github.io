---
layout: post
title: __attribute__
date: 2014-09-28 15:00:00
---
{% highlight Objective-C %}
// This data type is used to store information for each vertex
typedef struct {
   GLKVector3  positionCoords;
   GLKVector2  textureCoords;
}
SceneVertex;
{% endhighlight %}

{% highlight Objective-C %}
NSLog(@"offsetof1 = %lu", offsetof(SceneVertex, textureCoords));
{% endhighlight %}

打印数值为：16！

{% highlight Objective-C %}
union _GLKVector2
{
    struct { float x, y; };
    struct { float s, t; };
    float v[2];
} __attribute__((aligned(8)));
typedef union _GLKVector2 GLKVector2;
{% endhighlight %}

主要是__attribute__((aligned(8)))的使用；
aligned (alignment)
该属性设定一个指定大小的对齐格式（以字节 为单位）
该声明将强制编译器确保（尽它所能）变量类 型为struct S 或者int32_t 的变量在分配空间时采用8 字节对齐方式。

参考：<br/>
<a href="https://gcc.gnu.org/onlinedocs/gcc/Type-Attributes.html#Type-Attributes" rel="external nofollow" target="_blank" class="muted">Type-Attributes</a>