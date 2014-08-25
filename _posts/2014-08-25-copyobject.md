---
layout: post
title: Object copying
date: 2014-08-25 11:00:00
---

Copying an object creates a new object with the same class and properties as the original object. You copy an object when you want your own version of the data that the object contains. If you receive an object from elsewhere in an application but do not copy it, you share the object with its owner (and perhaps others), who might change the encapsulated contents. If you are creating a subclass, you might consider making it possible for others to copy instances of your class. Generally, an object should be “copyable” when it is a value object—an object whose main purpose is to encapsulate some data.

Requirements for Object Copying
An object can be copied if its class adopts the NSCopying protocol and implements its single method, copyWithZone:. If a class has mutable and immutable variants, the mutable class should adopt the NSMutableCopying protocol (instead of NSCopying) and implement the mutableCopyWithZone: method to ensure that copied objects remain mutable. You make a duplicate of an object by sending it a copy or mutableCopy message. These messages result in the invocation of the appropriate NSCopying or NSMutableCopying method.

Copies of objects can be shallow or deep. Both shallow- and deep-copy approaches directly duplicate scalar properties but differ on how they handle pointer references, particularly references to objects (for example, NSString *str). A deep copy duplicates the objects referenced while a shallow copy duplicates only the references to those objects. So if object A is shallow-copied to object B, object B refers to the same instance variable (or property) that object A refers to. Deep-copying objects is preferred to shallow-copying, especially with value objects.

//illustrate<br/>
<img src="/images/object_copying.jpg" />


Memory-Management Implications
Like object creation, object copying returns an object with a retain count of 1. In memory-managed code, the client copying the object is responsible for releasing the copied object. Copying an object is similar in purpose to retaining an object in that both express an ownership in the object. However, a copied object belongs exclusively to the new owner, who can mutate it freely, while a retained object is shared between the owners of the original object and clients who have retained the object.