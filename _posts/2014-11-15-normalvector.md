---
layout: post
title: 求平面法向量
date: 2014-11-15 10:00:00
---

平面法向量的具体步骤：（待定系数法）
<ol>
<li>建立恰当的直角坐标系</li>
<li>在平面内找出两个不共线的向量，记为a=（a1,a2, a3） b=（b1,b2,b3）</li>
<li>计算a×b=(a2b3-a3b2,a3b1-a1b3,a1b2-a2b1)=n，则因为叉乘（也叫向量积）的结果n垂直于a和b所在平面，因此n是平面的法向量。</li>
</ol>

小栗子：<br />
{% highlight Objective-C %}
/////////////////////////////////////////////////////////////////
// This function returns the face normal vector for triangle.
//求面的法向量
static GLKVector3 SceneTriangleFaceNormal(
   const SceneTriangle triangle){
    //不共线向量A
    GLKVector3 vectorA = GLKVector3Subtract(
                                            triangle.vertices[1].position,
                                            triangle.vertices[0].position);
    //不共线向量B
    GLKVector3 vectorB = GLKVector3Subtract(
                                            triangle.vertices[2].position,
                                            triangle.vertices[0].position);
    //求面的法向量
    return SceneVector3UnitNormal(
                                  vectorA,
                                  vectorB);
}

/////////////////////////////////////////////////////////////////
// Returns a unit vector in the same direction as the cross
// product of vectorA and VectorB
GLKVector3 SceneVector3UnitNormal(
                                  const GLKVector3 vectorA,
                                  const GLKVector3 vectorB){
    GLKVector3 crossProduct = GLKVector3CrossProduct(vectorA, vectorB);//计算法向量
    return GLKVector3Normalize(crossProduct);//计算单位向量
}
{% endhighlight %}

补充资料：<br/>
设向量A=(x1,y1,z1),B=(x2, y2, z2)

(一)向量的点积
向量的点积定义：
{% highlight Objective-C %}
AB=|A||B|cos<A,B>
{% endhighlight %}

其运算结果是一个常量。将其转换成矩阵乘法是
{% highlight Objective-C %}
AB = matrix(A)*matrix(B)=(x1,y1,z1)*(x2,y2,z2)=x1*x2 + y1*y2 + z1*z2 ;
{% endhighlight %}

(二)向量的叉积

向量的叉积的模定义：
{% highlight Objective-C %}
|A×B|=|A||B|sin<A,B>
{% endhighlight %}
其几何意义是以|A|，|B|为边长的平行四边形的面积。

向量的叉积定义：
{% highlight Objective-C %}
A×B=( y1z2 - y2z1 , x1z2 - x2z1 , x1y2 - x2y1)
{% endhighlight %}
利用向量的叉乘性质可以求取一个平面的法向量。
