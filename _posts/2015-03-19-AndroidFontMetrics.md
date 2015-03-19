---
layout: post
title: 【初学Android】Android FontMetrics
date: 2015-03-19 10:00:00
---

问题：
{% highlight Objective-C %}
canvas.drawText(needDrawString, 10, 80, mPaint );
{% endhighlight %}
10，80 代表什么意思？待画文字的坐标？NO!

下面看下Android上的FontMetrics.<br/>
{% highlight Objective-C %}
Class that describes the various metrics for a font at a given text size. Remember, Y values increase going down, so those values will be positive, and values that measure distances going up will be negative. <br/>
public float	ascent	The recommended distance above the baseline for singled spaced text.<br/>
public float	bottom	The maximum distance below the baseline for the lowest glyph in the font at a given text size.<br/>
public float	descent	The recommended distance below the baseline for singled spaced text.<br/>
public float	leading	The recommended additional space to add between lines of text.<br/>
public float	top	The maximum distance above the baseline for the tallest glyph in the font at a given text size.<br/>
{% endhighlight %}

通过下面代码画文字：<br/>
{% highlight Objective-C %}
public void onDraw(Canvas canvas){
	    super.onDraw(canvas);
        mPaint.reset();
        mPaint.setColor(Color.WHITE);
        mPaint.setTextSize(80);
        mPaint.setTextAlign(Paint.Align.LEFT); //default
		
// FontMetrics对象
        Paint.FontMetrics fontMetrics = mPaint.getFontMetrics();
        String text = "abcdefg";
// 计算每一个坐标
        float textWidth = mPaint.measureText(text);
        Log.d(TAG, "textWidth = " + textWidth);
        float textHeight = -mPaint.ascent() + mPaint.descent();
        Log.d(TAG, "textHeight = " + textHeight);
        String needDrawString = "TextHeight = " + textHeight; //"textWidth = " + textWidth +
        canvas.drawText(needDrawString, 10, 80, mPaint );
        float baseX = 30;
        float baseY = 700;
        float topY = baseY + fontMetrics.top;
        float ascentY = baseY + fontMetrics.ascent;
        float descentY = baseY + fontMetrics.descent;
        float bottomY = baseY + fontMetrics.bottom;
// 绘制文本
        canvas.drawText(text, baseX, baseY, mPaint);

// BaseLine描画
        mPaint.setColor(Color.RED);
        canvas.drawLine(baseX, baseY, baseX + textWidth, baseY, mPaint);
        mPaint.setTextSize(20);
        canvas.drawText("base", baseX + textWidth, baseY, mPaint);
// Base描画
        canvas.drawCircle(baseX, baseY, 5, mPaint);

// TopLine描画
        mPaint.setColor(Color.LTGRAY);
        canvas.drawLine(baseX, topY, baseX + textWidth, topY, mPaint);
        canvas.drawText("top", baseX + textWidth, topY, mPaint);
// AscentLine描画
        mPaint.setColor(Color.GREEN);
        canvas.drawLine(baseX, ascentY, baseX + textWidth, ascentY, mPaint);
        canvas.drawText("ascent", baseX + textWidth, ascentY + 10, mPaint);
// DescentLine描画
        mPaint.setColor(Color.YELLOW);
        canvas.drawLine(baseX, descentY, baseX + textWidth, descentY, mPaint);
        canvas.drawText("descent", baseX + textWidth, descentY, mPaint);
// ButtomLine描画
        mPaint.setColor(Color.MAGENTA);
        canvas.drawLine(baseX, bottomY, baseX + textWidth, bottomY, mPaint);
        canvas.drawText("buttom", baseX + textWidth, bottomY + 10, mPaint);
}
{% endhighlight %}

ps.<br/>
Remember, Y values increase going down, so those values will be positive, and values that measure distances going up will be negative.
比如在计算mPaint.ascent()的时候，要加-。<br/>
float textHeight = -mPaint.ascent() + mPaint.descent();<br/>

回到上面的问题，pt(10，80)表示baseline的起点.<br/>
如果修改如下：<br/>
mPaint.setTextAlign(Paint.Align.CENTER); 
则pt于文字的中点重合.
