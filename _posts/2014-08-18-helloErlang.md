---
layout: post
title: Erlang的hello world
date: 2014-08-18 12:30:00
---

Helloworld.erl
<br/>
{% highlight Objective-C %}
-module(Helloworld).    %声明模块名称，必须与文件名相同
-export([run/1]).   %指定向外界导出的函数列表
run(Name)->
    io:format("Hello World ~w~n",[Name]).
%~w可以理解成占位符，将被实际Name取代，~n就是换行了。
{% endhighlight %}

ps.其他占位符<br/>
{% highlight Objective-C %}
~c   anscii码
~f   浮点数
~s   字符串
~w   Erlang term
~p   与~w类似，不过当多行时将自动换行
~n   显然，换行符
{% endhighlight %}