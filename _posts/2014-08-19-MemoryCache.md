---
layout: post
title: iOS 内存缓存
date: 2014-08-19 10:00:00
---

{% highlight Objective-C %}
- (void)useCache{
    //使用缓存的目的是为了使用的应用程序能更快速的响应用户输入，是程序高效的运行。有时候我们需要将远程web服务器获取的数据缓存起来，减少对同一个url多次请求。
    //NSURLRequest需要一个缓存参数来说明它请求的url何如缓存数据的，我们先看下它的CachePolicy类型。
    NSString *paramURLAsString= @"http://www.baidu.com/";
    if ([paramURLAsString length] == 0){
        NSLog(@"Nil or empty URL is given");
        return;
    }
    NSURLCache *urlCache = [NSURLCache sharedURLCache];
    /* 设置缓存的大小为1M*/
    [urlCache setMemoryCapacity:4*1024*1024];
    //创建一个nsurl
    NSURL *url = [NSURL URLWithString:paramURLAsString];
    //创建一个请求
    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:url
                                                           cachePolicy:NSURLRequestUseProtocolCachePolicy
                                                       timeoutInterval:60.0f];
	//从请求中获取缓存输出
    NSCachedURLResponse *response = [urlCache cachedResponseForRequest:request];
    //判断是否有缓存
    if (response != nil){
        NSLog(@"如果有缓存输出，从缓存中获取数据");
        [request setCachePolicy:NSURLRequestReturnCacheDataDontLoad];
    }
    self.connection = nil;
    /* 创建NSURLConnection*/
    NSURLConnection *newConnection = [[NSURLConnection alloc] initWithRequest:request
                                                                     delegate:self
                                                             startImmediately:YES];
    self.connection = newConnection;
    [newConnection release];
}
{% endhighlight %}

NSURLConnectionDataDelegate
{% highlight Objective-C %}
- (void)  connection:(NSURLConnection *)connection
  didReceiveResponse:(NSURLResponse *)response{
    NSLog(@"将接收输出");
}
{% endhighlight %}

{% highlight Objective-C %}
- (NSURLRequest *)connection:(NSURLConnection *)connection
             willSendRequest:(NSURLRequest *)request
            redirectResponse:(NSURLResponse *)redirectResponse{
    NSLog(@"即将发送请求");
    return(request);
}
{% endhighlight %}

{% highlight Objective-C %}
- (void)connection:(NSURLConnection *)connection
    didReceiveData:(NSData *)data{
    NSLog(@"接受数据");
    NSLog(@"数据长度为 = %lu", (unsigned long)[data length]);
}
{% endhighlight %}

{% highlight Objective-C %}
/*
 Sent before the connection stores a cached response in the cache, to give the delegate an opportunity to alter it.
 */
- (NSCachedURLResponse *)connection:(NSURLConnection *)connection
                  willCacheResponse:(NSCachedURLResponse *)cachedResponse{    
    NSLog(@"将缓存输出");
    return(cachedResponse);
    /*
     return nil;
     就是不缓存 response!
     */
}
{% endhighlight %}

{% highlight Objective-C %}
- (void)connectionDidFinishLoading:(NSURLConnection *)connection{
    NSLog(@"请求完成");
}
{% endhighlight %}

{% highlight Objective-C %}
- (void)connection:(NSURLConnection *)connection
  didFailWithError:(NSError *)error{
   NSLog(@"请求失败");
}
{% endhighlight %}