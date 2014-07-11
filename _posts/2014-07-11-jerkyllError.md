---
layout: post
title: jekyll 2.0.3 问题汇总
date: 2014-07-11 16:30:00
---

1、【error】代码高亮部分，如果出现空行，运行命令 jekyll serve
报错：
{% highlight Objective-C %}
  Liquid Exception: highlight tag was never closed in _posts/XXXX.md/#excerpt
{% endhighlight %}

删除空行，运行成功。

2、【warning】执行命令 jekyll serve

{% highlight Objective-C %}
warning：
       Deprecation: The 'pygments' configuration option has been renamed to 'highlighter'. Please update your config file accordingly. The allowed values are 'rouge', 'pygments' or null.
{% endhighlight %}

修改__config.yml文件：
{% highlight Objective-C %}
# pygments:         true 修改为：highlighter:      pygments
{% endhighlight %}