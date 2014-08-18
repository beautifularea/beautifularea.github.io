---
layout: post
title: Erlang八种原始数据类型
date: 2014-08-18 11:00:00
---

<ol>
<li>整数</li>
<li>原子</li>
<li>浮点</li>
<li>引用-引用是全局唯一的符号，只用来比较两个引用是否相等。引用可以通过调用Erlang原语make_ref()来创造</li>
<li>二进制</li>
<li>Pid——Pid是Process Identifier（进程标识符）的缩写，Pid由Erlang的spawn（…）原语创建，Pid是Erlang进程的引用。</li>
<li>端口（port）——端口用于与外界通信，由内置函数（BIF3）open_port来创建。消息可以通过端口进行收发</li>
<li>匿名函数（fun）——匿名函数是函数闭包4，由表达式“fun(…) -> … end.”来创建。</li>
<li>元组（tuple）——元组是一种包含固定个数的Erlang数据的容器。{ D1, D2, …, Dn }表示一个元组</li>
<li>列表（list）——列表是包含可变个数的Erlang数据的容器。[Dh | Dt]表示一个列表它的第1个元素是Dh，余下的元素是一个列表Dt</li>
<li>字符串（string）——字符串记作用双引号引起来的字符系列</li>
ps.字符串“cat”只是是列表[97, 99, 116]的速记法
<li>记录（record）——记录提供了对每个元素都带有标记的元组的一种便利的访问方式。使得我们可以通过名字而不是通过位置来访问元组的元素</li>
</ol>
