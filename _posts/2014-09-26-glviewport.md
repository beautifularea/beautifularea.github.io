---
layout: post
title: glViewport和glOrtho
date: 2014-09-26 15:00:00
---

glOrtho是创建一个正交平行的视景体。 一般用于物体不会因为离屏幕的远近而产生大小的变换的情况。比如，常用的工程中的制图等。需要比较精确的显示。 而作为它的对立情况,<br/> glFrustum则产生一个透视投影。这是一种模拟真是生活中，人们视野观测物体的真实情况。例如：观察两条平行的火车到，在过了很远之后，这两条铁轨是会相交于一处的。还有，离眼睛近的物体看起来大一些，远的物体看起来小一些。<br/>
<br/>
glOrtho(left, right, bottom, top, near, far)， left表示视景体左面的坐标，right表示右面的坐标，bottom表示下面的，top表示上面的。这个函数简单理解起来，就是一个物体摆在那里，你怎么去截取他。这里，我们先抛开glViewport函数不看。先单独理解glOrtho的功能。<br/> 假设有一个球体，半径为1，圆心在(0, 0, 0)，那么，我们设定glOrtho(-1.5, 1.5, -1.5, 1.5, -10, 10);就表示用一个宽高都是3的框框把这个球体整个都装了进来。  如果设定glOrtho(0.0, 1.5, -1.5, 1.5, -10, 10);就表示用一个宽是1.5，<br/> 高是3的框框把整个球体的右面装进来;如果设定glOrtho(0.0, 1.5, 0.0, 1.5, -10, 10)；就表示用一个宽和高都是1.5的框框把球体的右上角装了进来。<br/>
<br/>
从上述三种情况，我们可以大致了解glOrtho函数的用法。glOrtho函数只是负责使用什么样的视景体来截取图像，并不负责使用某种规则把图像呈现在屏幕上。<br/>
<br/>
glViewport主要完成这样的功能。它负责把视景体截取的图像按照怎样的高和宽显示到屏幕上。