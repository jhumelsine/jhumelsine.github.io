---
title: Proxy Design Patterns
description: Provide a substitute or placeholder for a resource intensive object.
unlisted: true
---

<img src="/assets/ProxyAmbassador.png" alt="Ambassador as a Proxy" width = "50%" align="center" style="padding-right: 20px;">

# Introduction
The Proxy Design Pattern is the first one of the [Composable Design Pattern](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) series.

Sometimes objects need some additional administration care beyond their basic functionality. The uncaring designer will shift that burden onto the client developer possibly clearing his/her conscious by describing what needs to be done for proper administration in documentation.
There are a few potential problems with relying upon documentation alone:
* The client developer may not read that documentation.
* The client developer may read it, but not fully understand it or correctly implement it.
* Even if the client developer implements it correctly, it probably adds infrastructure details to the client code that obfuscates its true intent.
* Should those administrative needs may change in future release, who’s going to update the existing client code that’s becomes out of date when these needs change?

If the developer knows enough to describe administrative care in the documentation, then the developer may know enough to provide an implementation solution as well.

The Proxy Design Pattern is a way to add an administrative wrapper layer around basic functionality, so that the client developer won’t have to deal with it. It’s akin to a diplomatic ambassador who’s the proxy for the government he or she represents.

# Structure
I lauded the flexibility one gets with [Composable Design Pattern(https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html). We’re not going to quite see that flexibility with Proxy.

I mostly include Proxy with the Composability patterns since it’s a steppingstone toward the composability and flexibility that we’ll eventually see.
Here is my UML Class Diagram rendition of the Gang of Four (GoF) provided in their Design Pattern book:
* The Proxy Design Pattern is an extension of the [Strategy Design Pattern](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html). The only addition is the delegation from `Proxy` to `ConcreteFeature`.
`ConcreteFeature` represents the resource intensive class.
* `Proxy` is the administrative wrapper. It delegates most functionality to `ConcreteFeature`, but it performs additional functions, often administrative, before and/or after the delegation.
* `Client` doesn’t know about either concrete class. It only knows about the `Feature` interface.
* Both `ConcreteFeature` and `Proxy` implement `Feature`, meaning that any references to `Feature` will work with a reference to the `Proxy`->`ConcreteFeature` pair or with a reference to just `ConcreteFeature` in cases where the `Proxy` wrapper isn’t required.

<img src="/assets/ProxyGoF.png" alt="Proxy via GoF" width = "80%" align="center" style="padding-right: 20px;">
 
I have a few issues with the GoF’s diagram as shown above:
* `Proxy` delegates to `ConcreteFeature` directly. The GoF is violating their own [First Principle]( https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html). They are programming to the implementation, not the interface.
* I don’t know why there’s a diamond in the delegation from `Proxy` to `ConcreteFeature`. The diamond suggests that `Proxy` could have multiple references to `ConcreteFeature`, which is not the case here. I don’t think they used a diamond for a single reference in other diagrams. It’s not technically wrong, but it is a little non-conventional.
* There is no indication how the `Client` resolves its reference to `Feature`.

Here’s my updated UML Class Diagram to address some of my issues with their version:
* I’ve added more details, especially with the `Configurer` and construction. This is not the only way to construct these objects. It’s just one example.
* The constructors for `Client` and `Proxy` throw an exception when their `Feature` argument is null. This removes the need for a null check in `execute()` or any other place where `feature` is referenced in the classes. I won't repeat this technique in other diagrams due to space concerns.
* `Proxy` delegates to `Feature` rather than `ConcreteFeature`. This creates more possibilities, which will be featured in future design patterns.
* The diamond has been removed.

<img src="/assets/ProxyMine.png" alt="Proxy via with my UML diagram" width = "80%" align="center" style="padding-right: 20px;">
 
The instantiated `client` will have a `feature` reference to a `Proxy` instance, which will have a `feature` to a `ConcreteFeature` instance. When `client` makes a call to `feature.execute()`, `Proxy`’s `execute()` will run any code before calling its `feature.execute()`. When the `Proxy` instance calls `feature.execute()`, `ConcreteFeature`’s `execute()` will run its code. It will return to `Proxy` which will run any additional optional code, and then it will return back to `client`.

This diagram is a more detailed example of what I provide previously in [Composable Design Pattern](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html):

<img src="/assets/ComposableDesignPatternsIntroduction.png" alt="Composable Pattern Template" width = "40%" align="center" style="padding-right: 20px;">
 
# Lazy Initialization
Administrative care is probably the most common context for the Proxy Design Pattern, but it’s not necessarily the only one.

Imagine you have a class that requires significant resources when instantiated. You may not wish instantiate an object until you’re ready to invoke it. This is [Lazy Initialization](https://en.wikipedia.org/wiki/Lazy_initialization) and a type of administrative care.

Lazy Initialization allows a developer to manage resource allocation with more nuance, but it requires additional coding overhead. The Proxy Design Pattern allows us to move the Lazy Initialization implementation into a separate proxy class so that the client developer won’t have to add that additional overhead.

Here’s a UML class diagram updated specifically for a Lazy Initialization Proxy:
* `Proxy` is a bit different. Its `Feature feature` attribute is only initialized when the `execute()` method is executed.
* I violated the first design principle too a bit. `Proxy` has knowledge of `ConcreteFeature` but only in the context of being able to create an instance of it.
* `Configurer` no longer has any knowledge of `ConcreteFeature`.

<img src="/assets/ProxyLazyInit.png" alt="Proxy with Lazy Initialization" width = "80%" align="center" style="padding-right: 20px;">

`Proxy` will be a lightweight object until it’s executed. If the `Client` flow never executes it, then a `ConcreteFeature` is never instantiated.
The `Client` no longer needs to be concerned about the administration of `ConcreteFeature`. `Proxy` will manage it, even if `Client` does not know that `Proxy` exists. Its lazy initialization need only be implemented once, and it will be consistently applied.

# Use Case: Proxy for Memory Management
I have a beef with several things in the GoF Design Patterns book.

## Lack of Encapsulation with Creational Design Pattern Method Names
The GoF were obsessed with encapsulation. They emphasized that client code should only know about interfaces. It should not call `new()` directly, because that would require their direct knowledge of the concrete class.

They defined several creational design patterns to encapsulate `new()` from the client code. However, most of their creational design pattern method names imply the creation design pattern being used. The public method names reflect the creation mechanism design rather than the client developer’s needs. While the concrete classes are encapsulated, the underlying creational mechanism may not be.

This isn’t a major issue until the designer decides that a different creational design pattern might be a better choice. Then what? Change the existing method name to match the new creational pattern and break the API for any existing client code? Maintain the existing method name and imply a creation mechanism that’s no longer applicable? Neither choice is ideal.

Let’s return to API principles. An API should be designed from the client’s point of view, not the designers. Therefore, design the Creational API as one should for all APIs. The client code doesn’t care about the creational design pattern. That’s the concern of the designer. The client code only desire is to acquire an object instance. 

I started to use `acquire()` for all of my creational method names, regardless of the actual creational mechanism being used. The name can still be modified for client context, for example: `acquireByName(Name name)`.

## The Sin of Omission
I was a C++ developer when I learned the design patterns around 2004. Memory management was always a major concern with C++. The GoF don’t address memory management in their design pattern book, and C++ is one of their two demonstration languages. Some of their C++ examples leak memory.

Some creation mechanisms required memory clean up, whereas others did not. How would the client code know when to delete memory or not, especially with my creational agnostic `acquire()` method name, which provided no clues as to whether the returned object should be deleted or not.

Is memory management really the responsibility for the client code, especially when the client code doesn’t know how the memory was allocated in the first place? I don’t think it is. I had done some CORBA development in C++ a few years before learning the design patterns. CORBA thrusts a lot of mysterious memory management upon developers. I was never quite sure if I was releasing memory correctly or not. I always resented it. From: The Rise and Fall of CORBA:
> Another problem area is the C++ language mapping. The mapping is difficult to use and contains many pitfalls that lead to bugs, particularly with respect to thread safety, exception safety, and memory management. A number of other examples of overly complex and poorly designed APIs can be found in the CORBA specification, such as the naming, trading, and notification services, all of which provide APIs that are error-prone and difficult to use.

How can client code correctly manage memory when it doesn’t even know how the mechanism that acquired that acquired that memory in the first place?

I returned to API principles once more. Consider this from the client’s point of view. What does the client code know and what can it easily do? The client code knows when it’s done with an object. It may not know how to manage the memory, but it can let another entity with memory management knowledge know that it is releasing its claim to that object.

Much in the same way that there’s a creational method that acquires an instance, I introduced a matching method that allows the client code to release it. The API might look something like this:
```java
class Factory {
    public static Feature acquire() {
        // Acquire a Feature instance using any creational mechanism desired.
    }

    public static void release(Feature feature) {
        // Perform any memory management associated with the released object that’s consistent with the creational mechanism.
    }
}
```

Here’s a UML class diagram of how this could be designed:
* The `Client` depends upon `Factory`, but it has no knowledge of the underlying creation mechanism. The `Factory` designer could use a Factory Method, Singleton, Prototype, etc. Then later decide that another creation mechanism would be better, and the `Client` would not need to change.
* The `Client` acquires the `feature` object via `Factory.acquire()`, and it releases the `feature` object via `Factory.release(feature)` when it no longer requires it.
* Regardless of the creation mechanism, the memory associated with `feature` will be managed appropriately, since it’s now the responsibility of the `Factory` and not the `Client`.
* There is no Proxy in this design, yet.

<img src="/assets/ProxyAcquireRelease.png" alt="Factory with acquire() and release()" width = "80%" align="center" style="padding-right: 20px;">

## I don’t trust other developers. Heck, I don’t trust myself.
This works, except for one minor problem - the technique only resides in the design. It is not a common practice. Developers have to remember to call `release(Feature feature)` consistently. That’s not going to happen. I doubt that I’d even remember to call it consistently.

Even if developers take every effort to call `release(…)` consistently, there’s still the issue of another developer adding a return before the `release(…)` or a premature Exception that bails out of the method before `release(…)` has been called.

This is an insidious memory leak. The code will work even when `release(…)` is not called, so any issues are unlikely to be caught with testing. The code will still work in production for a while, but memory will be leaking. Memory resource exhaustion won’t manifest like a tire blowout while driving on the highway. It will manifest like a flat tire in the parking lot when leaving the office.

## What Other Tools Are in our Toolbox?
__CAVEAT: I have not been a C++ developer for over 10 years, and I was using an old release of C++ even then. Mechanisms for C++ resource management may have changed quite a bit since then. What I’m about to describe may not be current state of the art.__

C++ has an idiom called Resource Allocation Is Instantiation (RAII). It’s a horrible name, but it’s a great technique. Unlike Java, C++ allocates local objects on the call stack. When the call stack pops, the deconstructor for any local objects is called, and their resources are restored. This happens on any return or a thrown Exception.

I leveraged RAII along with Proxy to eliminate the burden of the client code having to call `release(…)`. Here’s the basic design:
* `Client` no longer needs to be concerned about `Factory.acquire()` and more importantly, it’s no longer concerned about `Factory.release(…)`. `Client` does not even know that `Factory` exists.
* The `Client` manages its `FeatureAPI` instance as it would any other instance. In the C++ example, this would probably be as a local object on the call stack. But even if the `Client` code developer decided to `new()` the `FeatureAPI`, then the developer would also be taking on the responsibility to `delete` it as well. Regardless, these are techniques that are in the first chapters of every C++ introduction book written. Memory management is no longer a specialized case. It’s the language standard.
* `FeatureAPI` is a proxy. It delegates to `Factory` to acquire and release its Feature attribute.
* When `FeatureAPI` is popped off the stack, it will release its Feature attribute.
* __NOTE: I designed this long before I understood Dependency Injection and Dependency Inversion Principle. Therefore, it violates some of those principles. For example, the `Client` still has indirect dependency upon every element in the entire design. It’s more difficult to unit test it in isolation. Other design modifications may address this, but I’m not going to concern myself with them now.__

<img src="/assets/ProxyRAII.png" alt="Factory with acquire() and release()" width = "80%" align="center" style="padding-right: 20px;">
 
## Java RAII?
I leveraged RAII often when I was a C++ developer. It was useful for more than just memory management. It was useful for any operations that came in open()/close() pairs, such as mutexes, database locks, etc.

I wanted to continue using it even when I moved to a Java environment. I remember putting resource recovery code in `finalize()`, without quite understanding what I was doing. C++ developers who move to Java often think of `finalize()` as the Java destructor, but that’s not quite the case. It’s called when an object is cleaned in garbage collection, not when it goes out of scope. There’s no guarantee that garbage collection will ever happen. `finalize()` has been deprecated, since it’s misused so often, including knuckleheads like me.

For the most part, we don’t need to worry about memory in Java, but what about open()/close() concepts in Java that aren’t memory?

The try/finally block helped with this, but it was a bit awkward. Java’s Try with resources is better. With TWR, a Closeable or AutoCloseable object is created in a try block. When it exists, the object’s `close()` method will be executed, and any additional resource management can be done there. TWR is basically RAII when you can’t explicitly create an object on the call stack.

But what if you have a class that requires resource cleanup management, but it’s not Closeable or AutoCloseable? You can’t use TWR directly, and the class may be outside of your scope to update? This sounds like the perfect place for a Proxy.

Here’s what the UML class diagram might look like:
* `ConcreteExternalFeature` is colored red, to indicate that it’s external, and you cannot modify it to make closeable.
* `ExternalFeature` is an interface, and it’s also red, since it’s external. `releaseResources()` has a comment that it should always be called to release resources, but there’s no enforcement mechanism other than strongly encouraging developers to perform call it.
* `ExternalFeatureProxy` implements `ExternalFeature` and delegates to its `ConcreteExternalFeature` attribute. `ExternalFeatureProxy` also implements `Closeable`, which means that it must implement `close()` as well. Its `close()` implementation calls `releaseResources()`.
* There’s no guarantee that `ExternalFeatureProxy` will have this amount of access to external APIs. Other techniques may be required based upon scoped accessibility.
* `Client` can now leverage try-with-resources even if the external feature doesn’t support it. When leaving the try scope, `ExternalFeatureProxy`’s `close()` method will be called, which will then release the external resources via its call to `releaseResources()`.

<img src="/assets/ProxyCloseable.png" alt="" width = "80%" align="center" style="padding-right: 20px;">

# Other Admins
Lazy Initialization and “Stack” Memory Management aren’t the only administrative concerns for Proxy. Proxy could also be used for a Cache, Remote Method Invocation helpers, etc.

But we have a minor issue. In its traditional presentation, there’s only one Proxy class. What if the Concrete Class has multiple administrative concerns? The next pattern, Decorator, will address this. My UML Class Diagram for Proxy hints at a solution when Proxy delegates to Feature rather than ConcreteFeature. Sit tight for Decorator.

# Summary
Proxy provides a mechanism to manage some administrative concerns in the design rather than thrusting them upon Client developers, who may not get the right anyhow.

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* TBD
Here are some resources that can be purchased or are included in a subscription service:
* TBD
