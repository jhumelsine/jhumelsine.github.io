---
title: WORK IN PROGRESS – Object Pool Design Pattern
description: TBD
unlisted: true
---

# Introduction
The Gang of Four (GoF) omitted __Object Pool__ as a design pattern from their catalog. I'm not sure why, since I'm sure that the concept existed at the time of the publication, or it may have been emerging. The earliest [Thread Pool](https://en.wikipedia.org/wiki/Thread_pool) references I could find where from around that period as well.

Even if the GoF had included it in their catalog, they may not have considered it a [Creational Design Pattern](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html), since an object is not created when acquired. However, I would still consider it a creational pattern, since from the client's point of view, the object is being acquired, even if the specific creation mechanism is encapsulated from the client.

# Intent
Object Pool allows multiple clients to access a set resource intensive objects one at a time without having to instantiate them for each use. By resource intensive, I mean that the objects are expensive to instantiate or they are coupled to a limited resources, such as a hardware constraint.

An Object Pool is filled with a fixed number of resource intenstive objects at start up. Then when a client requests one, it acquires it from the pool and then returns it when done so that it's available for another client.

There are plenty of real world examples of shared [Resource Pools](https://en.wikipedia.org/wiki/Pooling_(resource_management)) including:
<img src="https://heute-at-prod-images.imgix.net/2022/04/21/0d770b26-f233-4039-9a25-4cce88dcc9ec.jpeg?rect=0%2C283%2C3600%2C2025&auto=format" alt="Mad Men" title="Image Source: https://www.heute.at/i/mad-men-star-im-alter-von-90-jahren-gestorben-100202791/doc-1g16gc2ur0" width = "45%" align="right" style="padding: 35px;">
* [Secretarial Pools](https://en.wikipedia.org/wiki/Secretarial_pool) - Though mostly a thing of the past, executives would acquire a secretary from the pool, who could take dictation, type a letter, do filing or perform other secretarial skills. Once done with a task, secretaries would return to the pool for the next executive's task. Secretarial pools are not so much a thing anymore, but they were common before the advent of office computers. Women in the secretarial pool were featured on the TV Show: [Mad Men](https://en.wikipedia.org/wiki/Mad_Men).
* [Libraries](https://en.wikipedia.org/wiki/Library) - Libraries contain a finite number of books. Patrons checkout books and then return them several days or weeks later. Libraries are pool-like, but they are not a perfect fit, since libraries are filled with different books. To be a pure pool, the library would contain multiple copies of only one book.
* [Bowling Ball Shoe Rentals](https://en.wikipedia.org/wiki/Bowling#Shoes) - Customers rent a pair of shoes while at the bowling lanes and then return them when done. They are pool-ish too, since the shoes some in different sizes.
* [Car Rental Agencies](https://en.wikipedia.org/wiki/Car_rental) - Customers rent a car and then return it when done. They aren't pure pools either, but I suspect a car rental of [Model T Fords](https://en.wikipedia.org/wiki/Ford_Model_T) could be a pure pool, since they were mostly identical.

# Object Pool Design and Implementation
I will layer in various Object Pool design and implementation considerations.

## Flyweight vs Object Pool
Object Pool is structurally similar to [Flyweight](https://jhumelsine.github.io/2025/11/14/flyweight.html), but there are some differences. Here are some of each.

Flyweight and Object Pool have a few similarities:
* Each maintains a repository of objects
* Each tends to return an existing object rather than instantiate a new one upon demand
* Each has concurrency concerns

However, they have a few differences:
* Acquired Flyweight objects are shared by multiple clients, whereas acquired Object Pool objects are used by one client at a time
* Because Flyweight objects are shared, their intrinsic state must apply to all clients, whereas because Object Pool objects are only used by one client at a time, they can have client specific state
* The number of Flyweight objects can grow as requested, whereas the number of Object Pool objects are fixed
* Flyweight always returns an object when acquired, whereas an Object Pool might not have any available objects when one is requested
* Flyweight objects are initialized via [Lazy Initialization](https://en.wikipedia.org/wiki/Lazy_initialization), whereas Object Pool objects are initialized at start up
* Flyweight object reclamation is optional, whereas Object Pool reclamation is required

## Core Object Pool Design and Implementation
As mentioned above, Object Pool's structure is similar to Flyweight's structure. I do not have a specific context in mind, so class and interface names will not tend to indicate what they do within the context of a domain, but how they manage pooled objects.

Here are some highlights from the design:
* `Feature` defines a basic constract with `doSomething()`.
* `PooledObject`contains the bulk of the Object Pool structure. It maintains a collection of `pooledObjects` along with static `acquire()` and `release()` methods, which manage the objects within `pooledObjects`.
* `Client` is not part of the pattern design specifically, but it's important to feature how the client interacts with the pattern design.

<img src="/assets/ObjectPool1.png" alt="Core Object Pool Design" width = "80%" align="center" style="padding: 35px;">

Here is a Java implementation, which fills in more implementation details:
```java
interface Feature {
    void doSomething();
}

class PooledObject implements Feature {
    private final static int POOL_SIZE = 3;
    private static BlockingQueue<PooledObject> objectPool = new ArrayBlockingQueue<>(POOL_SIZE);

    static {
        for (int i = 0; i < POOL_SIZE; i++) {
            try {
                release(new PooledObject(i));
            } catch (Exception e) {
                System.out.println(e);
            }
        }
    }

    private final int idNumber;
    private String name;

    private PooledObject(int idNumber) {
        this.idNumber = idNumber;

        System.out.format("Creating PooledObject idNumber=%d\n", idNumber);
    }

    public static PooledObject acquire(String name) throws InterruptedException {
        PooledObject pooledObject = objectPool.take(); // Blocks if pool empty
        pooledObject.setName(name);
        return pooledObject;
    }

    public static void release(PooledObject pooledObject) throws Exception {
        if (objectPool.contains(pooledObject)) throw new IllegalStateException("Object already released");
        pooledObject.setName(null); // Cleans the object before returning it to the pool.
        objectPool.put(pooledObject); // Blocks if pool full
        System.out.format("Release %s by adding it to objectPool\n", pooledObject.toString());
    }

    private void setName(String name) {
        this.name = name;
    }

    @Override
    public void doSomething() {
        System.out.format("Do something with %s\n", toString());
    }

    @Override
    public String toString() {
        return String.format("PooledObject idNumber=%d, name=%s", idNumber, name);
    }
}
```

Here is the client code:
```java
PooledObject a = PooledObject.acquire("A");  // Take one object from pool
a.doSomething();                             // Perform some operation
PooledObject.release(a);                     // Return it to pool
a = null;                                    // Local reference cleared
```

A complete implementation of the above is availble at [Core Object Pool Implementation](#core-object-pool-implementation).

I have mentioned object reclaimation a few times in previous blogs:
* [Memory Leaks](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html#memory-leaks) in the [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) blog
* [Omission of Sin](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html#the-sin-of-omission) in the [Proxy](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern) blog
* [What the GoF Missed](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html#what-the-gof-missed) in the [Creational Design Patterns Introduction](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html) blog
* [Memory Leaks](https://jhumelsine.github.io/2025/10/31/singleton.html#memory-leaks) in the [Singleton](https://jhumelsine.github.io/2025/10/31/singleton.html) blog
* [Memory leaks](https://jhumelsine.github.io/2025/11/14/flyweight.html#memory-leaks) in the [Flyweight](https://jhumelsine.github.io/2025/11/14/flyweight.html) blog

I will continue the theme here as well too. The GoF presented different patterns to create objects, but they address what to do with the object once it was no longer needed. Memory management is critical in C++, which was one of their two example languages.

Memory management isn't quite as critical in Java, since [Garbage Collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)) will tend to handle it for you.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/SHALLOW_WATER_NO_DIVING_ALLOWED_II.jpg/960px-SHALLOW_WATER_NO_DIVING_ALLOWED_II.jpg" alt="Empty Pool" title="Image Source: https://commons.wikimedia.org/wiki/File:SHALLOW_WATER_NO_DIVING_ALLOWED_II.jpg" width = "45%" align="right" style="padding: 35px;">

The Object Pool pattern is a different case. We cannot rely upon garbage collection. In fact, we don't want the object to be collected. They're in the Object Pool, because they are resource intensive; therefore, once created, we want them to remain available for the duration of the system's run.

We need clients to release their object back to the pool when they no longer need them. If they do not, then we run the risk of a drained pool. This would be like clients borrowing library books and never returning them. The library would become filled with empty shelves.

I also cleared the local reference above as an additional safety consideration. Once an object has been released, the client should not reference it subsequently.

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

## Core Object Pool Implementation
```java
```


+++++++++++++++++++++++++++++++++++++
# NOTES
Objects need to be returned to the pool otherwise you'll end up with a drained pool. I don't think that work references will work. Need a means to release the object. Maybe something like close() might help.

Objects need to be cleaned of any intrinsic state, otherwise, you could end up with a cess pool.

What to do when an object is requested, but the pool is empty?
* Block wait.
* Block wait with timeout.
* Exception immediately.
* Create a new object on the spot.

As for an implementation, consider using a Queue. May need an `initialize(arguments)` method as well.

Still have to deal with concurrency and state. Memory leaks is a different topic. This pattern will always leak in that objects are added to the pool at start up and they remain as long as the process is running.
