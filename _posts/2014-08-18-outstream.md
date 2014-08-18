---
layout: post
title: 使用NSOutputStream将网络请求的数据保存到本地
date: 2014-08-18 14:00:00
---

{% highlight Objective-C %}
@property (nonatomic, retain) NSOutputStream *fileStream;
{% endhighlight %}

{% highlight Objective-C %}
NSString *filePath =[NSHomeDirectory() stringByAppendingPathComponent:@"Documents/stream"];
self.fileStream = [NSOutputStream outputStreamToFileAtPath: filePath append:NO];
[self.fileStream open];
{% endhighlight %}

{% highlight Objective-C %}
- (void)connection:(NSURLConnection *)connection
    didReceiveData:(NSData *)data{
	NSLog(@"接受数据");    
    NSLog(@"数据长度为 = %lu", (unsigned long)[data length]);
    NSInteger       dataLength;
    const uint8_t * dataBytes;
    NSInteger       bytesWritten;
    NSInteger       bytesWrittenSoFar;
    // 接收到的数据长度
    dataLength = [data length];
    dataBytes  = [data bytes];
    bytesWrittenSoFar = 0;
    do {
        bytesWritten = [self.fileStream write:&dataBytes[bytesWrittenSoFar] maxLength:dataLength - bytesWrittenSoFar];
        assert(bytesWritten != 0);
        if (bytesWritten == -1) {
            break;
        } else {
            bytesWrittenSoFar += bytesWritten;
        }
    } while (bytesWrittenSoFar != dataLength);
}
{% endhighlight %}