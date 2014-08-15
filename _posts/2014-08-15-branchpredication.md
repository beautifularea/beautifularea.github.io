---
layout: post
title: Branch predication
date: 2014-08-15 15:00:00
---

stackoverflow 上有一个问题：<a href="http://stackoverflow.com/questions/11227809/why-is-processing-a-sorted-array-faster-than-an-unsorted-array" rel="external nofollow" target="_blank" class="muted">Why is processing a sorted array faster than an unsorted array?</a>
先摘录他代码如下：
{% highlight Objective-C %}
#include <algorithm>
#include <ctime>
#include <iostream>
int main()
{
    // Generate data
    const unsigned arraySize = 32768;
    int data[arraySize];
    for (unsigned c = 0; c < arraySize; ++c)
        data[c] = std::rand() % 256;
    // !!! With this, the next loop runs faster
    std::sort(data, data + arraySize);    
    // Test
    clock_t start = clock();
    long long sum = 0;
    for (unsigned i = 0; i < 100000; ++i)
    {
        // Primary loop
        for (unsigned c = 0; c < arraySize; ++c)
        {
            if (data[c] >= 128)
                sum += data[c];
//            int t = (data[c] - 128) >> 31;
//            sum += ~t & data[c];
        }
    }
    double elapsedTime = static_cast<double>(clock() - start) / CLOCKS_PER_SEC;
    std::cout << elapsedTime << std::endl;
    std::cout << "sum = " << sum << std::endl;
}
{% endhighlight %}

在我Mac pro上运行时间如下：<br/>
排序前：24.7035<br/>
排序后：8.10423<br/>
为什么排序前和排序后，运行的时间差别那么大！<br/>

这就涉及到了一个概念Branch predication。<br/>
Branch predication is a strategy in computer architecture design for mitigating the costs usually associated with conditional branches, particularly branches to short sections of code. It does this by allowing each instruction to conditionally either perform an operation or do nothing.

ps.<br/>
后边答案还提到，如果改用位操作的话，排序与否是没有影响的。

更多参考：<br/>
<a href="http://stackoverflow.com/questions/11227809/why-is-processing-a-sorted-array-faster-than-an-unsorted-array" rel="external nofollow" target="_blank" class="muted">Why is processing a sorted array faster than an unsorted array?</a>
<br/>
<a href="http://en.wikipedia.org/wiki/Branch_predication" rel="external nofollow" target="_blank" class="muted">Branch predication</a>
<br/>
<a href="http://users.cis.fiu.edu/~downeyt/cop3402/prediction.html" rel="external nofollow" target="_blank" class="muted">Branch Prediction</a>