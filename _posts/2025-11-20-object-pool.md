---
title: WORK IN PROGRESS – Object Pool Design Pattern
description: TBD
unlisted: true
---

# Introduction
The Gang of Four (GoF) omitted __Object Pool__ as a design pattern from their catalog. I'm not sure why, since I'm sure that the concept existed at the time of the publication. For example,  [Thread Pools](https://en.wikipedia.org/wiki/Thread_pool) should have been 

# Intent
Object Pool allows multiple clients to access a set resource intensive objects one at a time without having to instantiate them for each use. By resource intensive, I mean that the objects are expensive to instantiate or they are coupled to a limited resources, such as a hardware constraint.

An Object Pool is filled with a fixed number of resource intenstive objects at start up. Then when a client requests one, it acquires it from the pool and then returns it when done so that it's available for another client.

There are plenty of real world examples of shared [Resource Pools](https://en.wikipedia.org/wiki/Pooling_(resource_management)) including:
<img src="https://heute-at-prod-images.imgix.net/2022/04/21/0d770b26-f233-4039-9a25-4cce88dcc9ec.jpeg?rect=0%2C283%2C3600%2C2025&auto=format" alt="Mad Men" title="Image Source: https://www.heute.at/i/mad-men-star-im-alter-von-90-jahren-gestorben-100202791/doc-1g16gc2ur0" width = "45%" align="right" style="padding: 35px;">
* [Secretarial Pools](https://en.wikipedia.org/wiki/Secretarial_pool) - Though mostly a thing of the past, executives would acquire a secretary from the pool, who could take dictation, type a letter, do filing or perform other secretarial skills. Once done with a task, secretaries would return to the pool for the next executive's task. Secretarial pools are not so much a thing anymore, but they were common before the advent of office computers. Women in the secretarial pool were featured on the TV Show: [Mad Men](https://en.wikipedia.org/wiki/Mad_Men).
* [Libraries](https://en.wikipedia.org/wiki/Library) - Libraries contain a finite number of books. Patrons checkout books and then return them several days or weeks later. Libraries are pool-like, but they are not a perfect fit, since libraries are filled with different books. To be a pure pool, the library would contain multiple copies of only one book.
* [Bowling Ball Shoe Rentals](https://en.wikipedia.org/wiki/Bowling#Shoes) - Customers rent a pair of shoes while at the bowling lanes and then return them when done. They are pool-ish too, since the shoes some in different sizes.
* [Car Rental Agencies](https://en.wikipedia.org/wiki/Car_rental) - Customers rent a car and then return it when done. They aren't pure pools either, but I suspect a car rental of [Model T Fords](https://en.wikipedia.org/wiki/Ford_Model_T) could be a pure pool, since they were mostly identical.

# Flyweight vs Object Pool
[Flyweight](https://jhumelsine.github.io/2025/11/14/flyweight.html) and Object Pool have a few similarities:
* Each maintains a repository of objects
* Each tends to return an existing object rather than instantiate a new one upon demand
* Each has concurrency concerns

However, they have a few differences:
* Acquired Flyweight objects are shared by multiple clients, whereas acquired Object Pool objects are used by one client at a time
* The number of Flyweight objects can grow as requested, whereas the number of Object Pool objects are fixed
* Flyweight always returns an object when acquired, whereas an Object Pool might not have any available objects when one is requested
* Flyweight objects are initialized via [Lazy Initialization](https://en.wikipedia.org/wiki/Lazy_initialization), whereas Object Pool objects are initialized at start up
* Flyweight object reclamation is optional, whereas Object Pool reclamation is required

# Object Pool Design and Implementation

# Summary

# References
* [Wikipedia Object Pool Design Pattern](https://en.wikipedia.org/wiki/Object_pool_pattern)
* [Source Making Object Pool Design Pattern](https://sourcemaking.com/design_patterns/object_pool)
* [Object Pool](https://gameprogrammingpatterns.com/object-pool.html) - Blog by GameProgrammingPatterns.com
* [Object Pool Pattern in Java: Enhancing Performance with Reusable Object Management](https://java-design-patterns.com/patterns/object-pool/) - Blog by Java-Design-Patterns.com
* [Introduction to Object Pooling](https://learn.unity.com/tutorial/introduction-to-object-pooling) - Blog by unity.com
* [Create an object pool by using a ConcurrentBag](https://learn.microsoft.com/en-us/dotnet/standard/collections/thread-safe/how-to-create-an-object-pool) - Microsoft article
* [ObjectPools - Performance Engineering in Java](https://www.youtube.com/watch?v=9WaQ3J3u3jg) - Video by kevgol0
* [Level up your code with game programming design patterns: Object pool; Tutorial](https://www.youtube.com/watch?v=U08ScgT3RVM) - Video by Unity
* [The Object Pool - Programming Design Patterns - Ep 4 - C++ Coding](https://www.youtube.com/watch?v=lNTYs72Hi_0) - Video by Code, Tech, and Tutorials
* and for more, Google: [Object Pool Design Pattern](https://www.google.com/search?q=object+pool+design+pattern)

# Complete Demo Code
Here’s the entire implementation up to this point as one file. Copy and paste it into a Java environment and execute it. If you don’t have Java, try this [Online Java Environment](https://www.programiz.com/java-programming/online-compiler/). Play with the implementation.


+++++++++++++++++++++++++++++++++++++
# NOTES

I consider it a creational pattern even if the objects are not being created. The client is still acquiring them.

Feels similar to Flyweight structurally, but it's different. Both involve collections of objects. Flyweight objects are shared. Object Pool objects are not shared.

Object Pools are used when an object is resource intensive. It make take a lot of time to instantiate, or it may be tied to a limited resource.

Even if most haven't implemented an Object Pool, most have used one. A Thread Pool is an object pool.

Objects need to be returned to the pool otherwise you'll end up with a drained pool. I don't think that work references will work. Need a means to release the object. Maybe something like close() might help.

Objects need to be cleaned of any intrinsic state, otherwise, you could end up with a cess pool.

What to do when an object is requested, but the pool is empty?
* Block wait.
* Block wait with timeout.
* Exception immediately.
* Create a new object on the spot.

As for an implementation, consider using a Queue. May need an `initialize(arguments)` method as well.

Still have to deal with concurrency and state. Memory leaks is a different topic. This pattern will always leak in that objects are added to the pool at start up and they remain as long as the process is running.
