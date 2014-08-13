---
layout: post
title: NSArray排序，二分查找
date: 2014-08-13 17:30:00
---

<li>排序一：
{% highlight Objective-C %}
NSArray *unsortedArray = [NSArray arrayWithObjects: @"1",@"3",@"2",@"9" , @"0",nil];
    //compare
NSArray *sortedArray = [unsortedArray sortedArrayUsingSelector:@selector(compare:)];
//    NSLog(@"sortedArray = %@", sortedArray);
{
    NSArray *sortedArray = [unsortedArray sortedArrayUsingComparator:^NSComparisonResult(id obj1, id obj2) {
        NSComparisonResult result = [obj1 compare: obj2];
        return result == NSOrderedDescending; // 升序
        return result == NSOrderedAscending;  // 降序
    }];
    NSLog(@"sortedArray = %@", sortedArray);
}
{% endhighlight %}

<li>排序二：
{% highlight Objective-C %}
NSString *searchObject = @"9";
NSRange searchRange = NSMakeRange(0, [sortedArray count]);
NSUInteger findIndex = [sortedArray indexOfObject:searchObject
                                    inSortedRange:searchRange
                                          options:NSBinarySearchingFirstEqual
                                  usingComparator:^(id obj1, id obj2){
                            return [obj1 compare:obj2];
                        }];
NSLog(@"findIndex = %d", findIndex);
{% endhighlight %}

<li>二分查找一：
{% highlight Objective-C %}
NSString *searchObject = @"9";
NSRange searchRange = NSMakeRange(0, [sortedArray count]);
NSUInteger findIndex = [sortedArray indexOfObject:searchObject
                                    inSortedRange:searchRange
                                          options:NSBinarySearchingFirstEqual
                                  usingComparator:^(id obj1, id obj2){
                            return [obj1 compare:obj2];
                        }];
NSLog(@"findIndex = %d", findIndex);
{% endhighlight %}

<li>二分查找二：
{% highlight Objective-C %}
NSMutableArray *sortedArray1 = [NSMutableArray arrayWithObjects: @"Alice", @"Beth", @"Carol",@"Ellen",nil];
//Where is "Beth"?
unsigned index = (unsigned)CFArrayBSearchValues((CFArrayRef)sortedArray1,
                                                CFRangeMake(0, CFArrayGetCount((CFArrayRef)sortedArray1)),
                                                (CFStringRef)@"Beth",
                                                (CFComparatorFunction)CFStringCompare,
                                                NULL);
if (index < [sortedArray1 count] && [@"Beth" isEqualToString:sortedArray1[index]])
{
    NSLog(@"Beth was found at index %u", index);
} else {
    NSLog(@"Beth was not found (index is beyond the bounds of sortedArray)");
}
//Where should we insert "Debra"?
unsigned insertIndex = (unsigned)CFArrayBSearchValues((CFArrayRef)sortedArray1,
                                                      CFRangeMake(0, CFArrayGetCount((CFArrayRef)sortedArray1)),
                                                      (CFStringRef)@"Debra",
                                                      (CFComparatorFunction)CFStringCompare,
                                                      NULL);
[sortedArray1 insertObject:@"Debra" atIndex:insertIndex];
NSLog(@"sortedArray1 = %@",sortedArray1);
{% endhighlight %}

<li>二分查找三：
{% highlight Objective-C %}
NSArray *numberArray = @[@1, @3, @27, @36, @42, @70, @82];
NSNumber *searchNum = @70;

NSUInteger mid;
NSUInteger min = 0;
NSUInteger max = [numberArray count] - 1;

BOOL found = NO;

while (min <= max) {
    mid = (min + max)/2;
    if ([searchNum intValue] == [numberArray[mid] intValue]) {
        NSLog(@"We found the number! It is at index %lu", (unsigned long)mid);
        found = YES;
        break;
    } else if ([searchNum intValue] < [numberArray[mid] intValue]) {
        max = mid - 1;
    } else if ([searchNum intValue] > [numberArray[mid] intValue]) {
        min = mid + 1;
    }
}
if (!found) {
    NSLog(@"The number was not found.");
}
{% endhighlight %}

参考：
<a href="http://oleb.net/blog/2013/07/nsarray-binary-search/" rel="external nofollow" target="_blank" class="muted">NSArray Binary Search</a>
<a href="http://www.getappninja.com/blog/implementing-a-binary-search-in-ios" rel="external nofollow" target="_blank" class="muted">How to Implement A Binary Search Algorithm in iOS</a>

ps.<br/>
/**
 * Returns the index of the first object found in the receiver
 * which is equal to anObject (using anObject's [NSObject-isEqual:] method).
 * Returns NSNotFound on failure.
 */

//GUNSTEP NSArray indexOfObject: 方法的实现
{% highlight Objective-C %}
- (NSUInteger) indexOfObject: (id)anObject
{
  unsigned	c = [self count];

if (c > 0 && anObject != nil)
{
  unsigned	i;
  IMP	get = [self methodForSelector: oaiSel];
  BOOL	(*eq)(id, SEL, id)
= (BOOL (*)(id, SEL, id))[anObject methodForSelector: eqSel];

  for (i = 0; i < c; i++)
if ((*eq)(anObject, eqSel, (*get)(self, oaiSel, i)) == YES)
  return i;
}
  return NSNotFound;
}
{% endhighlight %}