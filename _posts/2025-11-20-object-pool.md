---
title: WORK IN PROGRESS – Object Pool Design Pattern
description: TBD
unlisted: true
---

# Introduction
The Gang of Four (GoF) included [Flyweight](https://jhumelsine.github.io/2025/11/15/flyweight.html), but cataloged it as a structural design pattern. They may have felt that most interactions with Flyweight returned an existing object rather than creating a new one. But if this were the case, then [Singleton](https://jhumelsine.github.io/2025/10/31/singleton.html) should have been a structural design pattern too.

The GoF completely ignored __Object Pool__ as a design pattern, creational, structural or otherwise. I'm not sure why, since I'm sure that the [Thread Pools](https://en.wikipedia.org/wiki/Thread_pool) existed during the time of publication.

<img src="https://heute-at-prod-images.imgix.net/2022/04/21/0d770b26-f233-4039-9a25-4cce88dcc9ec.jpeg?rect=0%2C283%2C3600%2C2025&auto=format" alt="Mad Men" title="Image Source: https://www.heute.at/i/mad-men-star-im-alter-von-90-jahren-gestorben-100202791/doc-1g16gc2ur0" width = "35%" align="right" style="padding: 35px;">

And there are plenty of real world examples of shared [Resource Pools](https://en.wikipedia.org/wiki/Pooling_(resource_management)) including:
* [Secretarial Pools](https://en.wikipedia.org/wiki/Secretarial_pool)
* [Libraries](https://en.wikipedia.org/wiki/Library)
* [Bowling Ball Shoe Rentals](https://en.wikipedia.org/wiki/Bowling#Shoes)

# Intent
Flyweight and Object Pool are both about  +++ TBD +++

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
