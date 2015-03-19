---
layout: post
title: 【初学Android】自定义View
date: 2015-03-19 10:30:00
---

跟iOS类似。<br/>
1、创建java class,name it : DrawView.<br/>
2、修改继承基类为View.<br/>
3、重写两个构造方法:<br/>
{% highlight Objective-C %}
//used when creating the view in code
public DrawingView(Context context){
    super(context);
}
//used when inflating the view from XML
public DrawingView(Context context, AttributeSet attributeSet){
    super(context, attributeSet);
}
{% endhighlight %}

4、最后，实现onDraw()函数<br/>
{% highlight Objective-C %}
public void onDraw(Canvas canvas){
    super.onDraw(canvas);
	//todo
}
{% endhighlight %}

5、显示<br/>
mDrawingView = new DrawingView(this);
setContentView(mDrawingView);
