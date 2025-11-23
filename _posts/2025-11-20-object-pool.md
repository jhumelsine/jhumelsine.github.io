---
title: WORK IN PROGRESS – Object Pool Design Pattern
description: TBD
unlisted: true
---

# Introduction
The Gang of Four (GoF) omitted __Object Pool__ as a design pattern from their catalog. I'm not sure why, since the concept existed or was emerging at the time of their publication.

Even if the GoF had included it in their catalog, they may not have considered it a [Creational Design Pattern](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html), since an object is not created when acquired. However, I still consider it a creational pattern, since from the client's point of view, the object is being acquired, even if the specific creation mechanism is encapsulated from the client.

# Intent
Object Pool allows multiple clients to access a set resource intensive objects without having to instantiate them for each use. By resource intensive, I mean that the objects are expensive to instantiate, such as requiring a lot of time, or they are coupled to a limited resources, such as a hardware constraint. A [Thread Pool](https://en.wikipedia.org/wiki/Thread_pool) is an example of an Object Pool.

An Object Pool is allocated with a fixed number of resource intenstive objects at start up. Then when a client requests one, it acquires it from the pool and then returns it when done so that it's available for another client.

There are many real world examples of shared [Resource Pools](https://en.wikipedia.org/wiki/Pooling_(resource_management)) including:
<img src="https://heute-at-prod-images.imgix.net/2022/04/21/0d770b26-f233-4039-9a25-4cce88dcc9ec.jpeg?rect=0%2C283%2C3600%2C2025&auto=format" alt="Mad Men" title="Image Source: https://www.heute.at/i/mad-men-star-im-alter-von-90-jahren-gestorben-100202791/doc-1g16gc2ur0" width = "50%" align="right" style="padding: 35px;">
* [Secretarial Pools](https://en.wikipedia.org/wiki/Secretarial_pool) - Though mostly a thing of the past, executives would acquire a secretary from the pool, who could take dictation, type a letter, do filing or perform other secretarial skills. Once done with a task, secretaries would return to the pool for the next executive's task. Secretarial pools are not as prevalant anymore, but they were common before the advent of office computers. Women in the secretarial pool were sometimes featured on the TV Show set in the 1950s or 1960s such as [Mad Men](https://en.wikipedia.org/wiki/Mad_Men).
* [Libraries](https://en.wikipedia.org/wiki/Library) - Libraries contain a finite number of books. Patrons checkout books and then return them several days or weeks later. Libraries are pool-like, but they are not a perfect fit, since libraries are filled with different books. To be a pure pool, the library would contain multiple copies of only one book.
* [Bowling Ball Shoe Rentals](https://en.wikipedia.org/wiki/Bowling#Shoes) - Customers rent a pair of shoes while at the bowling lanes and then return them when done. They are pool-ish too, since the shoes come in different sizes.
* [Car Rental Agencies](https://en.wikipedia.org/wiki/Car_rental) - Customers rent a car and then return it when done. They aren't pure pools either, but I suspect a car rental of [Model T Fords](https://en.wikipedia.org/wiki/Ford_Model_T) could be a pure pool, since they were mostly identical.

# Object Pool Design and Implementation
I will layer in various Object Pool design and implementation considerations.

## Flyweight vs Object Pool
Object Pool is structurally similar to [Flyweight](https://jhumelsine.github.io/2025/11/14/flyweight.html), but there are some differences. Here are a few similarities and differences.

Flyweight and Object Pool have the following similarities:
* Each maintains a repository of objects
* Each tends to return an existing object rather than instantiate a new one upon demand
* Each has concurrency concerns

However, they have the following differences:
* Acquired Flyweight objects are shared by multiple clients at the same time, whereas acquired Object Pool objects are shared by multiple clients, but at most one client per object at any given time.
* Because Flyweight objects are shared, their intrinsic state must apply to all clients, whereas because Object Pool objects are only used by one client at a time, they can have client specific state
* The number of Flyweight objects can grow as requested, whereas the number of Object Pool objects are fixed
* Flyweight always returns an object when acquired, whereas an Object Pool might not have any available objects when one is requested
* Flyweight objects are initialized via [Lazy Initialization](https://en.wikipedia.org/wiki/Lazy_initialization), whereas Object Pool objects are initialized at start up
* Flyweight object reclamation is optional, whereas Object Pool reclamation is required

## Core Object Pool Design and Implementation
As mentioned above, Object Pool's structure is similar to Flyweight's structure. My design and implementation example does not have a specific context in mind, so class and interface names will not indicate what they do within the context of a domain. Their names indicate how they manage pooled objects.

Here are some highlights from the design:
* `Feature` defines a basic constract with `doSomething()`.
* `PooledObject`contains the bulk of the Object Pool structure. It maintains a collection of `pooledObjects` along with static `acquire()` and `release()` methods, which manage the objects within `pooledObjects`.
* `Client` is not part of the pattern design specifically, but it's important to feature how the client interacts with the pattern design.

<img src="/assets/ObjectPool1.png" alt="Core Object Pool Design" width = "80%" align="center" style="padding: 35px;">

### PooledObject
Here is a Java implementation, which provides more implementation details:
* The `PooledObject` instances can reside in almost any sort of container. I chose a queue with a fixed size of 3.
* The `PooledObjects` are added to `objectPool` via a static method. Adding them via `release(PooledObject)` feels like an oddly named method. I named `release(PooledObject)` from the cllient's point of view to be used when releasing the `PooledObject` back into the poo. However, the behaviors needed to add the instance initially and to release it by the client are the same, so I'm using `release(PooledObject)` to initialize the pool at start up.
* I added `id` to `PooledObject` as a means to uniquely identify each object in the pool.
* `name` is intrinsic state information provided by the client when acquiring a pooled object.
* `acquire(String name)` retrives a `PooledObject` from `objectPool`, initializes its intrinsic state with the client provided name and returns it.
* `release(ObjectPool)` cleans the instance by setting the name to null and adds it back to the `objectPool` queue.

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

A complete implementation of the above is availble at [Core Object Pool Implementation](#core-object-pool-implementation).

<img src="https://live.staticflickr.com/3347/5842100167_d9fe46fe02_b.jpg" alt="Latrine Pit" title="Image Source: https://www.flickr.com/photos/vastateparksstaff/5842100167/" width = "35%" align="right" style="padding: 35px;">

`release(ObjectPool)` cleans the released instance by setting the name to null. If intrinsic data isn't scrubbed in `release(ObjectPool)` then we run the risk of intrinsic data provided by one client remaining in the intrinsic data of another client. Our Object Pool would become a Cesspool, and no one wants a dirty pool.

This example only has to clean `name`. Pooled objects with more intrinsic state would require more cleaning.

It's like spraying disinfectant into bowling shoes whenthey are returned.

### Client Code
Here is the client code:
```java
PooledObject a = PooledObject.acquire("A");  // Take one object from pool
a.doSomething();                             // Perform some operation
PooledObject.release(a);                     // Return it to pool
a = null;                                    // Local reference cleared
```

While `PooledObject` provides the `release(PooledObject)` method, it's the client's responsibility to call it.

I have mentioned object reclaimation a few times in previous blogs:
* [Memory Leaks](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html#memory-leaks) in the [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) blog
* [Omission of Sin](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html#the-sin-of-omission) in the [Proxy](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern) blog
* [What the GoF Missed](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html#what-the-gof-missed) in the [Creational Design Patterns Introduction](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html) blog
* [Memory Leaks](https://jhumelsine.github.io/2025/10/31/singleton.html#memory-leaks) in the [Singleton](https://jhumelsine.github.io/2025/10/31/singleton.html) blog
* [Memory Leaks](https://jhumelsine.github.io/2025/11/14/flyweight.html#memory-leaks) in the [Flyweight](https://jhumelsine.github.io/2025/11/14/flyweight.html) blog

I will continue the theme here as well too. The GoF presented different patterns to create objects, but they did not address what to do with the object once it was no longer needed. Memory management is critical in C++, which was one of their two example languages. Some of their examples leaked memory.

Memory management isn't quite as critical in Java, since [Garbage Collection](https://en.wikipedia.org/wiki/Garbage_collection_(computer_science)) will tend to handle it for us.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/a/a7/SHALLOW_WATER_NO_DIVING_ALLOWED_II.jpg/960px-SHALLOW_WATER_NO_DIVING_ALLOWED_II.jpg" alt="Empty Pool" title="Image Source: https://commons.wikimedia.org/wiki/File:SHALLOW_WATER_NO_DIVING_ALLOWED_II.jpg" width = "45%" align="right" style="padding: 35px;">

The Object Pool pattern is a different case. We cannot rely upon garbage collection. In fact, we don't want the pooled object to be collected. They're in the Object Pool, because their creation is resource intensive. Once created, we want them to remain available for the duration of the process' execution.

We need clients to release their object back to the pool when they no longer need them. If they do not, then we run the risk of a drained pool. This would be like library patrons borrowing books and never returning them. The library would become filled with empty shelves.

I also had the client clear the local reference above by setting it to null as an additional safety consideration. Once an object has been released, the client should not reference it subsequently, since it may have been acquired by another client.

### Empty Pool
Even with clients releaseing their objects reliably and consistently, we can still end up with an empty pool. There may be more requesting clients than pooled objects. For example, the implementation example initilized the object pool with three objects. What if a fourth client requested one? This is an issue that other creational pattern have not had to contend with.

We have several options.

#### Block Wait
The code example features this blocking approach. It's basic, but it may be a bit naïve, since it could result in deadlock like situations.

#### Block Wait with Timeout Exception
This is the same as above, but it prevents indefinite deadlock postponement. However, the client needs to handle the exception or declare it.

#### Throw Exception
Don't even way for a timeout. Just throw the exception. The client needs to handle or declare the exception as well.

#### Create a New Object On Demand
We can expand the pool by adding another object to it upon demand. This is a bit counterintuitive, since the point of this pattern is to have a pool of fixed size.

### Final Thoughts
My implementation example doesn't keep track of client acquired objects. In addition to the `objectPool` queue, we might want to also maintain the set of acquired objects. We might want to do this for pooled object integrity. My `release(PooledObject)` will allow any object to be added to the pool, including one that might be malicious.

If the `ObjectPool` maintains all pooled objects whether current being used by clients or waiting to be acquired, then we would be more likely to identify and prevent foreign, potentially malicious, objects being injected into the pool via `release(PooledObject)`.

## Proxy Wrapped Object Pool Design and Implementation
The core design and implementation listed above places a lot of responsibility upon the client to release the object and clear it locally. I don't trust developers to get that right. I wouldn't even trust myself to geth it right.

When I was a C++ developer I used the [Resource Allocation Is Instantiation](https://en.wikipedia.org/wiki/Resource_acquisition_is_initialization) (RAII) idiom to accommodate this. RAII is used for classes that have start and finish operations, such as a [mutex](https://en.wikipedia.org/wiki/Lock_(computer_science)) lock/unlock and a database open/close.

RAII is a [Proxy](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html). Its constructor executes the start operation. Its destructor executes the finish operation. 

The proxy wrapper object is instantiated upon the stack. Its constructor is executed, which executes the start operation. When the object exists its current scope whether it reaches the end of the scope, returns or an exception is thrown, the object is popped off the stack, and its destructor is executed, which executes the finish operation. It's a nice way to ensure that start/finish operations always execute in pairs. Once I learned of RAII and started using it, I no longer had to worry about leaving a mutex locked because of an unexpected exception being thrown.

However, Java doesn't have destructors nor are objects instantiated on the stack. Java doesn't support RAII in the same form that C++ can. But it does support a version of it with its [try-with-resources](https://docs.oracle.com/javase/tutorial/essential/exceptions/tryResourceClose.html) statement.

Here's a design that adds a `WrappedObject` in front of the `PooledObject`:
<img src="/assets/ObjectPool2.png" alt="Core Object Pool Design" width = "80%" align="center" style="padding: 35px;">

`WrappedObject` has taken on the client's release responsibility from the previous design and implementation:
```java
class WrappedObject implements Feature, Closeable {
    private PooledObject pooledObject;

    private WrappedObject(PooledObject pooledObject) {
        this.pooledObject = pooledObject;
    }

    public static WrappedObject acquire(String name) throws Exception {
        return new WrappedObject(PooledObject.acquire(name));
    }

    @Override
    public void doSomething() {
        pooledObject.doSomething();
    }

    @Override
    public void close() throws IOException  {
        if (pooledObject == null) {
            // Already closed or never initialized - nothing to do.
            return;
        }
        
        try {
            PooledObject.release(pooledObject);
            pooledObject = null;
        } catch (Exception e) {
            System.out.println(e);
        }
    }
}
```

Now that `WrappedObject` has taken on the release and clean up responsibility for the client, the client's code becomes much nicer and safer with:
```java
try (WrappedObject a = WrappedObject.acquire("A")) {
    a.doSomething();
}
```

I was about to write more about RAII, but since I've already addressed it in [Sin of Omission](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html#the-sin-of-omission), I'll just provide a reference if anyone's interested.

A complete implementation of the above is availble at [Proxy Wrapped Object Pool Design and Implementation](#proxy-wrapped-object-pool-implementation).

# Summary
TBD

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
Here’s the entire implementation up to this point as one file. Copy and paste it into a Java environment and execute it. If you don’t have Java, try this [Online Java Environment](https://www.programiz.com/java-programming/online-compiler/). Play with the implementation. Copy and paste the code into Generative AI for analysis and comments.

## Core Object Pool Implementation
```java
```

## Proxy Wrapped Object Pool Design and Implementation
```java
```

+++++++++++++++++++++++++++++++++++++
# NOTES
Still have to deal with concurrency and state. Memory leaks is a different topic. This pattern will always leak in that objects are added to the pool at start up and they remain as long as the process is running.
