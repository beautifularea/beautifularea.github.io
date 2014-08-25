---
layout: post
title: autorelease
date: 2014-08-25 10:30:00
---

The NSAutoreleasePool class is used to support Cocoa’s reference-counted memory management system. An autorelease pool stores objects that are sent a release message when the pool itself is drained.
If you use Automatic Reference Counting (ARC), you cannot use autorelease pools directly. Instead, you use @autoreleasepool blocks. For example, in place of:
{% highlight Objective-C %}
NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
// Code benefitting from a local autorelease pool.
[pool release];
{% endhighlight %}
you would write:
{% highlight Objective-C %}
@autoreleasepool {
    // Code benefitting from a local autorelease pool.
}
{% endhighlight %}
@autoreleasepool blocks are more efficient than using an instance of NSAutoreleasePool directly; you can also use them even if you do not use ARC.

In a reference-counted environment (as opposed to one which uses garbage collection), an NSAutoreleasePool object contains objects that have received an autorelease message and when drained it sends a release message to each of those objects. Thus, sending autorelease instead of release to an object extends the lifetime of that object at least until the pool itself is drained (it may be longer if the object is subsequently retained). An object can be put into the same pool several times, in which case it receives a release message for each time it was put into the pool.

Each thread (including the main thread) maintains its own stack of NSAutoreleasePool objects (see “Threads”). As new pools are created, they get added to the top of the stack. When pools are deallocated, they are removed from the stack. Autoreleased objects are placed into the top autorelease pool for the current thread. When a thread terminates, it automatically drains all of the autorelease pools associated with itself.

In a garbage-collected environment, there is no need for autorelease pools. You may, however, write a framework that is designed to work in both a garbage-collected and reference-counted environment. In this case, you can use autorelease pools to hint to the collector that collection may be appropriate. In a garbage-collected environment, sending a drain message to a pool triggers garbage collection if necessary; release, however, is a no-op. In a reference-counted environment, drain has the same effect as release. Typically, therefore, you should use drain instead of release.

If you are making Cocoa calls outside of the Application Kit’s main thread—for example if you create a Foundation-only application or if you detach a thread—you need to create your own autorelease pool.

If your application or thread is long-lived and potentially generates a lot of autoreleased objects, you should periodically drain and create autorelease pools (like the Application Kit does on the main thread); otherwise, autoreleased objects accumulate and your memory footprint grows. If, however, your detached thread does not make Cocoa calls, you do not need to create an autorelease pool.

Autorelease Pool Blocks<br/>
{% highlight Objective-C %}
NSArray *urls = <# An array of file URLs #>;
for (NSURL *url in urls) { 
@autoreleasepool {
    NSError *error;
    NSString *fileContents = [NSString stringWithContentsOfURL:url
                                     encoding:NSUTF8StringEncoding error:&error];
    /* Process the string, creating and autoreleasing more objects. */
}
}
{% endhighlight %}

After an autorelease pool block, you should regard any object that was autoreleased within the block as “disposed of.” Do not send a message to that object or return it to the invoker of your method. If you must use a temporary object beyond an autorelease pool block, you can do so by sending a retain message to the object within the block and then send it autorelease after the block, as illustrated in this example:
{% highlight Objective-C %}
– (id)findMatchingObject:(id)anObject {
    id match;
    while (match == nil) {
        @autoreleasepool {
            /* Do a search that creates a lot of temporary objects. */
            match = [self expensiveSearchForObject:anObject];
            if (match != nil) {
                [match retain]; /* Keep match around. */
            }
        }
    }
    return [match autorelease];   /* Let match go and return it. */
}
{% endhighlight %}

实例方法<br/>
drain vs. release
drain<br/>
In a reference-counted environment, releases and pops the receiver; in a garbage-collected environment, triggers garbage collection if the memory allocated since the last collection is greater than the current threshold.
In a reference-counted environment, this method behaves the same as release. Since an autorelease pool cannot be retained (see retain), this therefore causes the receiver to be deallocated. When an autorelease pool is deallocated, it sends a release message to all its autoreleased objects. If an object is added several times to the same pool, when the pool is deallocated it receives a release message for each time it was added.

release<br/>
Releases and pops the receiver.
In a reference-counted environment, since an autorelease pool cannot be retained (see retain), this method causes the receiver to be deallocated. When an autorelease pool is deallocated, it sends a release message to all its autoreleased objects. If an object is added several times to the same pool, when the pool is deallocated it receives a release message for each time it was added.

- (id)retain<br/>
In a reference-counted environment, this method raises an exception.
Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '*** -[NSAutoreleasePool retain]: Cannot retain an autorelease pool'



<br/>
<a href="https://developer.apple.com/library/ios/documentation/Cocoa/Reference/Foundation/Classes/NSAutoreleasePool_Class/Reference/Reference.html" rel="external nofollow" target="_blank" class="muted">NSAutoreleasePool Class Reference</a>
<br/>
<a href="https://developer.apple.com/library/ios/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmAutoreleasePools.html#//apple_ref/doc/uid/20000047" rel="external nofollow" target="_blank" class="muted">Using Autorelease Pool Blocks</a>