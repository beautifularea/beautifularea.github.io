---
layout: post
title: Radian-Degree 转换
date: 2014-08-04 15:00:00
---

{% highlight Objective-C %}
#include <math.h>
static inline double degreeToRadians (double degrees) {return degrees * M_PI/180;}
#define DegreesToRadians(x) ((x) * M_PI / 180.0)
{% endhighlight %}

<br/>
Radians and Degrees<br/>
The radian is the standard unit of angle measure. If you were to take a circle and cut out a slice with an angle of one radian, the arc length of that slice would be exactly equal to the radius of the circle, no matter what size circle you are measuring. Degrees are another unit of angle measure created by the ancient Babylonians. They conceptualized a full revolution subdivided into 360 individualized parts, each "part" being a single degree.

Radian Formatting<br/>
In radians, one revolution is 2π (pi, approximately 3.14159) radians, because a full circle's arc length--or circumference--is 2π * its radius. As a result, many angle measurements include π as part of the measurement. A "right angle," or the angle formed by two perpendicular lines, is π/2 radians, as it is a quarter of a full revolution.

Conversion Process<br/>
To convert radians to degrees, one must first multiply the number by 180, then divide by π. This is mathematically equivalent to multiplying by the fraction 180/π. As the most common radian measures already contains π as part of the measurement, this just means you can remove the π--dividing it by itself--and multiply the result by 180.

Examples<br/>
π/3 radians = π/3 * (180/π) degrees = 180/3 degrees = 60 degrees.<br/>
2π/9 radians = 2π/9 * (180/π) degrees = 360/9 degrees = 40 degrees.<br/>
5 radians = 5 * (180/π) degrees = 900/π degrees ~= 286.479 degrees.<br/>

<br/>
参考：
<br/>
<li>
  <a href="http://math.rice.edu/~pcmi/sphere/drg_txt.html" rel="external nofollow" target="_blank" class="muted">Degree/Radian Circle</a>
</li>
