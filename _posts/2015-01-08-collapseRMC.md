---
layout: post
title: 锁屏后播放按钮状态错误
date: 2015-01-08 11:00:00
---

接上一篇文章，在配置后，在锁屏状态就可以显示歌曲信息并且进行切换等操作，但是在实际的项目中遇到个问题。
信息显示没有问题，声音键，切换按钮，播放键功能都正常，唯一就是暂停或者播放歌曲的按钮，一直显示为【播放】状态，
查看
{% highlight Objective-C %}
remoteControlReceivedWithEvent
{% endhighlight %}

回调，event.subtype 一直显示为：101. UIEventSubtypeRemoteControlPause 状态。

刚开始以为是
{% highlight Objective-C %}
[[AVAudioSession sharedInstance] setCategory: AVAudioSessionCategoryPlayback error: nil];
[[AVAudioSession sharedInstance] setActive: YES error: nil];
{% endhighlight %}

问题，后来发现没问题，排除掉。

就开始找初始化声音的单例类，发现有个地方使用到了OpenAL,就进行了初始化，然后就没用了。[俗话说，只要是我不认识的，就是有问题的！]
注掉这个方法，然后发现可以了！

在同时使用AVAudioPlayer 和 OpenAL 就会出现这样的冲突吗？！不是太了解，感觉这个问题也够奇葩的，下一步研究下OpenAL，可能到时候就能捋顺了。
敬请期待！