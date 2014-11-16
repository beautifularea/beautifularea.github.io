---
layout: post
title: 判断NSString是否包含中文字符
date: 2014-10-24 10:00:00
---

{% highlight Objective-C %}
- (BOOL)isIncludeChineseInString:(NSString*)str
{
    for (int i=0; i<str.length; i++)
    {
        unichar ch = [str characterAtIndex:i];
        if (0x4e00 < ch  && ch < 0x9fff)
        {
            return true;
        }
    }
    return false;
}
{% endhighlight %}
<br />
最常用的范围是 U+4E00～U+9FA5，即名为：CJK Unified Ideographs 的区块，但 U+9FA6～U+9FFF 之间的字符还属于空码，