---
layout: post
title: Remote Control Events
date: 2015-01-08 10:00:00
---

最近要在已有的项目中添加这个功能，锁屏后显示歌曲信息及对歌曲进行操作，在网上能找了不少demo，现在把主要的几个点摘录下来。

1、配置工程的-info.plist文件
{% highlight Objective-C %}
Required background modes , value: App plays audio or streams audio/video using AirPlay
{% endhighlight %}

2、添加Framework
{% highlight Objective-C %}
AVFoundation.framework
MediaPlayer.framework
{% endhighlight %}

3、关键代码
{% highlight Objective-C %}
[[AVAudioSession sharedInstance] setCategory: AVAudioSessionCategoryPlayback error: nil];
[[AVAudioSession sharedInstance] setActive: YES error: nil];
{% endhighlight %}

{% highlight Objective-C %}
[[UIApplication sharedApplication] beginReceivingRemoteControlEvents];
[self becomeFirstResponder];
{% endhighlight %}

{% highlight Objective-C %}
- (BOOL)canBecomeFirstResponder
{
    return YES;
}
{% endhighlight %}

{% highlight Objective-C %}
- (void)remoteControlReceivedWithEvent:(UIEvent *)event{
    NSLog(@"event.subtype = %d", event.subtype);
    
    if (event.type == UIEventTypeRemoteControl) {
        switch (event.subtype) {
            case UIEventSubtypeRemoteControlPlay:
            case UIEventSubtypeRemoteControlPause:
            case UIEventSubtypeRemoteControlStop:
            case UIEventSubtypeRemoteControlTogglePlayPause:
            {
                if(_avPlayer.isPlaying){
                    [_avPlayer pause];
                }
                else{
                    [_avPlayer play];
                }
            }
                break;
                
            default:
                break;
        }
    }
}
{% endhighlight %}

{% highlight Objective-C %}
NSString *string = [[NSBundle mainBundle] pathForResource:@"匆匆那年" ofType:@"mp3"];
NSURL *url = [NSURL fileURLWithPath:string];
_avPlayer = [[AVAudioPlayer alloc] initWithContentsOfURL:url error:nil];
[_avPlayer prepareToPlay];
{% endhighlight %}

4、显示歌曲信息<br/>
_currentTime 为当前播放的时间。点击暂停时候，需要特殊设置一下，否则时间从0开始显示<br/>
{% highlight Objective-C %}
Class playingInfoCenter = NSClassFromString(@"MPNowPlayingInfoCenter");
if (playingInfoCenter) {
    NSMutableDictionary *songInfo = [NSMutableDictionary dictionary];
    MPMediaItemArtwork *albumArt = [[MPMediaItemArtwork alloc] initWithImage: [UIImage imageNamed:@"AlbumArt"]];
    
    [songInfo setObject:@"Audio Title" forKey:MPMediaItemPropertyTitle];
    [songInfo setObject:@"Audio Author" forKey:MPMediaItemPropertyArtist];
    [songInfo setObject:@"Audio Album" forKey:MPMediaItemPropertyAlbumTitle];
    [songInfo setObject:albumArt forKey:MPMediaItemPropertyArtwork];

    CMTime play_second = CMTimeMake(_currentTime,1.0);
    CMTime length = CMTimeMake(song.length, 1.0);
    
    NSLog(@"play_second = %f current time = %f", CMTimeGetSeconds(play_second), _currentTime);
    
    int type = [AL_XIAMIManager shareInstance].playStatus;
    if([AL_XIAMIManager shareInstance].playStatus == XIAMI_Pause){
        [dict setObject:[NSNumber numberWithDouble:CMTimeGetSeconds(CMTimeMakeWithSeconds(_currentTime, 1.0))]
                 forKey:MPNowPlayingInfoPropertyElapsedPlaybackTime];
        
        [dict setObject:[NSNumber numberWithFloat:0.000001] forKey:MPNowPlayingInfoPropertyPlaybackRate];//进度光标的速度 （这个随 自己的播放速率调整，我默认是原速播放）
        [dict setObject:[NSNumber numberWithDouble:CMTimeGetSeconds(length)] forKey:MPMediaItemPropertyPlaybackDuration];//歌曲总时间设置
    }
    else if(type == XIAMI_Playing){

        [dict setObject:[NSNumber numberWithDouble:CMTimeGetSeconds(play_second)]
                 forKey:MPNowPlayingInfoPropertyElapsedPlaybackTime]; //音乐当前已经播放时间
        
        [dict setObject:[NSNumber numberWithFloat:1.0] forKey:MPNowPlayingInfoPropertyPlaybackRate];//进度光标的速度 （这个随 自己的播放速率调整，我默认是原速播放）
        [dict setObject:[NSNumber numberWithDouble:CMTimeGetSeconds(length)] forKey:MPMediaItemPropertyPlaybackDuration];//歌曲总时间设置
    }

    [[MPNowPlayingInfoCenter defaultCenter] setNowPlayingInfo:songInfo];
}
{% endhighlight %}

参考:<br/>
<a href="http://jaysonlane.net/tech-blog/2012/04/lock-screen-now-playing-with-mpnowplayinginfocenter/" rel="external nofollow" target="_blank" class="muted">Lock screen Info</a>
<br/>
<a href="https://developer.apple.com/library/ios/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/Remote-ControlEvents/Remote-ControlEvents.html" rel="external nofollow" target="_blank" class="muted">Remote Control Events</a>

