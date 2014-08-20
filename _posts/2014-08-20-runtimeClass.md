---
layout: post
title: Objective-C 动态创建类Class
date: 2014-08-20 17:30:00
---

<ol>
<li>创建一个类Class</li>
{% highlight Objective-C %}
const char * className = "Calculator";
Class kclass = objc_getClass(className);
if (!kclass)
{
    Class superClass = [NSObject class];
    kclass = objc_allocateClassPair(superClass,className,0);
}
{% endhighlight %}

<li>添加一个成员变量</li>
{% highlight Objective-C %}
NSUInteger size;
NSUInteger alignment;
NSGetSizeAndAlignment("*",&size,&alignment);
class_addIvar(kclass,"expression", size,alignment,"*");
{% endhighlight %}

<li>添加成员办法</li>
{% highlight Objective-C %}
/*
 "v＠:＠"，申明v-返回值void类型，＠-self指针id类型，:-SEL指针SEL类型，＠-函数第一个参数为id类型
 "＠＠:"，申明＠-返回值id类型，＠-self指针id类型，:-SEL指针SEL类型
 */
class_addMethod(kclass, @selector(setExpressionFormula:),(IMP)setExpressionFormula,"v＠:＠");
class_addMethod(kclass, @selector(getExpressionFormula), (IMP)getExpressionFormula,"＠＠:");
{% endhighlight %}

<li>注册到运行时景象</li>
{% highlight Objective-C %}
objc_registerClassPair(kclass);
{% endhighlight %}

<li>实例化类</li>
{% highlight Objective-C %}
id instance = [[kclass alloc] init];
{% endhighlight %}

<li>给变量赋值</li>
{% highlight Objective-C %}
object_setInstanceVariable(instance, "expression", "1+1");
{% endhighlight %}

<li>获取变量值</li>
{% highlight Objective-C %}
void *value = NULL;
object_getInstanceVariable(instance, "expression", &value);
{% endhighlight %}

<li>调用函数</li>
{% highlight Objective-C %}
[instance performSelector: @selector(getExpressionFormula)];
{% endhighlight %}

参考：<br/>
<a href="https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/ObjCRuntimeGuide/Articles/ocrtTypeEncodings.html#//apple_ref/doc/uid/TP40008048-CH100-SW1" rel="external nofollow" target="_blank" class="muted">Type Encodings</a>

<a href="https://developer.apple.com/library/ios/documentation/Cocoa/Reference/ObjCRuntimeRef/Reference/reference.html" rel="external nofollow" target="_blank" class="muted">Objective-C Runtime Reference</a>
