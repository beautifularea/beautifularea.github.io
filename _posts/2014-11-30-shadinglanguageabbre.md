---
layout: post
title: 着色语言基础-笔记
date: 2014-11-30 10:00:00
---

1、数据类型
-标量
{% highlight Objective-C %}
bool / int / float
{% endhighlight %}

-向量
{% highlight Objective-C %}
vec2 / vec3 / vec4  float
ivec2 / ivec3 / ivec4 int
bvec2 / bvec3 / bvec4 bool
{% endhighlight %}

<ul>
<li>向量为颜色：r.g.b.a 四个分量</li>
<li>向量为位置：x.y.z.w 四个分量</li>
<li>向量为纹理坐标：s.t.p.q 四个分量</li>

-矩阵
{% highlight Objective-C %}
mat2 / mat3 / mat4

float * float 
{% endhighlight %}

-采样器
{% highlight Objective-C %}
sample2D / sample3D / sampleCube
{% endhighlight %}

-结构体
{% highlight Objective-C %}
struct Info{
	vec3 color;
	vec3 postion;
	vec2 textCoor;
}
{% endhighlight %}

-数组
{% highlight Objective-C %}
vec3 position[20]
{% endhighlight %}

-空类型
{% highlight Objective-C %}
void
{% endhighlight %}

*** 属性变量、 一致变量、 易变变量 一定不能初始化！！！<br/>


*** 。 混合选择<br/>
必须来自同一个组！！！如：<br/>
rgba 、 stpq。。。。。。<br/>

OpenGL es 使用【列向量】 ， 因此矩阵只能出现在列向量左边进行相乘！！！<br/>


shading language 不支持类型转换，只能在构造函数时候转换：<br/>
float f = 1.0;<br/>
bool b = bool(f); --如此！！！<br/>


四钟限定符：<br/>
attribute 顶点位置、颜色等(只用在顶点着色器中)<br/>
uniform 光源位置等<br/>
varying 从顶点到片元着色器的量<br/>
const 常量<br/>


函数参数序列：<br/>
in / out / inout <br/>

片元着色器中浮点变量经度：<br/>
lowp / mediump / highp <br/>
lowp float color;<br/>

precision lowp float color;!!!!<br/>

内建变量：<br/>
顶点着色器<br/>
gl_Position<br/>
输出！<br/>

片元着色器：<br/>
输入：<br/>
gl_FragCoord;<br/>
gl_FrontFacing;<br/>

输出：<br/>
gl_FragColor;<br/>
gl_FragData;<br/>

内建N多函数：<br/>
略。。。<br/>


举个栗子：<br/>
{% highlight Objective-C %}
/////////////////////////////////////////////////////////////////
// VERTEX ATTRIBUTES
/////////////////////////////////////////////////////////////////
attribute vec3 a_position;
attribute vec3 a_normal;
attribute vec2 a_texCoords0;

/////////////////////////////////////////////////////////////////
// TEXTURE
/////////////////////////////////////////////////////////////////
#define MAX_TEXTURES    1
#define MAX_TEX_COORDS  1

/////////////////////////////////////////////////////////////////
// UNIFORMS
/////////////////////////////////////////////////////////////////
uniform highp mat4      u_mvpMatrix;
uniform highp mat3      u_normalMatrix;
uniform sampler2D       u_units[MAX_TEXTURES];
uniform lowp  vec4      u_globalAmbient;
uniform highp vec3      u_diffuseLightDirection;
uniform highp vec4      u_diffuseLightColor;

/////////////////////////////////////////////////////////////////
// Varyings
/////////////////////////////////////////////////////////////////
varying highp vec2      v_texCoords[MAX_TEX_COORDS];
varying lowp vec4       v_lightColor;

void u_method(){
    
}

void main()
{
   // Texture coords
   v_texCoords[0] = a_texCoords0;
   
   // Lighting
   lowp vec3 normal = normalize(u_normalMatrix * a_normal);
   lowp float nDotL = max(
      dot(normal, normalize(u_diffuseLightDirection)), 0.0);
   v_lightColor = (nDotL * u_diffuseLightColor) + 
      u_globalAmbient;
   
   gl_Position = u_mvpMatrix * vec4(a_position, 1.0); 
}
{% endhighlight %}