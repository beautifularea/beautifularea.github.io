---
layout: post
title: OC 对象序列化和反序列化
date: 2014-11-17 10:00:00
---

面向对象的程序在运行的时候会创建一个复杂的对象图，经常要以二进制的方法序列化这个对象图，这个过程叫做Archiving. 二进制流可以通过网络或写入文件中<br/>

举个栗子：<br />
需要被序列化的类：<br/>
{% highlight Objective-C %}
@interface Car : NSObject<NSCoding>
@property (nonatomic, copy) NSString *name;
@property (nonatomic, copy) NSString *type;
@end

@implementation Car
- (void)encodeWithCoder:(NSCoder *)aCoder{
    [aCoder encodeObject: self.name forKey:@"name"];
    [aCoder encodeObject: self.type forKey:@"type"];
}

- (id)initWithCoder:(NSCoder *)aDecoder{
    self = [super init];
    if (self){
        self.name = [aDecoder decodeObjectForKey:@"name"];
        self.type = [aDecoder decodeObjectForKey:@"type"];
    }
    return self;
}
@end
{% endhighlight %}

实例化Car对象<br/>
{% highlight Objective-C %}
Car *car = [[Car alloc] init];
car.name = @"奔驰";
car.type = @"4";

NSData *archiveCarPriceData = [NSKeyedArchiver archivedDataWithRootObject: car];
[[NSUserDefaults standardUserDefaults] setObject:archiveCarPriceData forKey:@"car"];

Car *newCar = nil;
NSData *myEncodedObject = [[NSUserDefaults standardUserDefaults] objectForKey:@"car"];
newCar = [NSKeyedUnarchiver unarchiveObjectWithData: myEncodedObject];
NSLog(@"new car name = %@", newCar.name);
NSLog(@"new car type = %@", newCar.type);
{% endhighlight %}