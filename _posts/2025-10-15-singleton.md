---
title: WORK IN PROGRESS – Singleton Design Pattern
description: The One, The Only; The Good, The Bad, And The Ugly.
unlisted: true
---

# Introduction
When I interviewed job candidates, I’d ask them if they knew any Design Patterns. Most didn’t know what I was talking about. Some had heard of them, but they didn't know much more than that. A few had the Gang of Four (GoF) Design Pattern book but had never read it. I never held anyone’s lack of Design Pattern knowledge against them, since the lack of knowledge was so prevalent. I had not learned the Design Pattern myself until my mid-forties, so I felt that shouldn't judge others negatively either.

When candidates could name a few Design Patterns, __Singleton__ seemed to be the pattern that was mentioned most often, even if they weren’t quite sure how to apply it. 

I suspect that most developers still may not know how to apply it, and Singleton’s description is in part to blame for this.

# I come to bury Singleton, not to praise it
I feel that incorporating design patterns into your designs will improve the quality of your code as I’ve described in previous [Design Pattern blog entries](https://jhumelsine.github.io/table-of-contents#design-patterns). I’m not convinced that Singleton falls into that category.

Singleton has its place in a design, but it’s a narrow spot. It’s often used outside of its narrow niche. Modern techniques may even make that niche more narrow.

Singleton is simple; almost too simple. It’s fraught with traps and pitfalls. Singleton’s misuse reduces to several issues:
* It’s easy to understand, implement and use without fully grasping the implications
* It’s easy to implement incorrectly
* It’s easy to reference from almost anywhere within a design, which can cause additional issues

# Singleton’s Single Purpose in a Design
As I pointed out in [What is an Object?](https://jhumelsine.github.io/2025/09/03/what-is-an-object.html), Singleton’s purpose is to ensure that there’s no more than one instance for a class.

The GoF defined Singleton’s intent as:
>Ensure a class only has one instance, and provide a global point of access to it.

[Refactoring Guru’s](https://refactoring.guru/design-patterns/singleton) description is almost identical:
>__Singleton__ is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

[Source Making’s](https://sourcemaking.com/design_patterns/singleton) description adds a new nuance:
>* Ensure a class has only one instance, and provide a global point of access to it.
>
>* Encapsulated "just-in-time initialization" or "initialization on first use".

Here are a few valid domain model situations for which we would desire no more than one instance for a class: System Clock, the File System, a Data Repository, etc.

# Unit Testing Concerns
Singleton is one instance ... everywhere. Its access often resides deep within the code, often embeeded within a statement. These attributes can create unit testing concerns.

Singleton acquisition occurs via a static method, which is difficult to replace with a [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html) in unit testing. And if you're not able to inject a dedicated Test Double for each unit test, you'll end up acquiring the sole Singleton instance for each of those tests. The Singleton is often associated with an external dependency. There will be one instance that all of your unit test cases may be accessing at the same time, and that instance may be coupled to an external dependency. The exeuction of one test might update the Singleton instance such that it affects the behavior of other unit test cases making them [flaky](https://jhumelsine.github.io/2025/04/14/humble-object.html#flaky-tests).

Mockito 3.4.0 provided the ability to inject Test Double behaviors into static methods without having to touch the implementation as described in this [Mockito Static Method Tutorial](https://www.baeldung.com/mockito-mock-static-methods). Even though Mockito provided this ability, the tutorial adds this [caveat](https://www.baeldung.com/mockito-mock-static-methods#a-quick-word-on-testing-static-methods):
>Generally speaking, some might say that when writing clean object-orientated code, we shouldn’t need to mock static classes. __This could typically hint at a design issue or code smell in our application.__
>
>Why? First, a class depending on a static method has tight coupling, and second, it nearly always leads to code that is difficult to test. Ideally, a class should not be responsible for obtaining its dependencies, and if possible, they should be externally injected.
>
>__So, it’s always worth investigating if we can refactor our code to make it more testable.__ Of course, this is not always possible, and sometimes we need to mock static methods.

I worked on a project that used [MongoDB](https://en.wikipedia.org/wiki/MongoDB). Long before I joined the project, they had created static methods for each operation as wrappers around Mongo details. It was a type of [Façade](https://jhumelsine.github.io/2023/10/03/facade-design-pattern.html). While I appreciated not having to dive into Mongo details, it was a bit cumbersome to create and initialize Test Doubles for these static methods. I used [Mockito's](https://site.mockito.org/) static method mocking. While it worked, it was not the most elegant mechanism I’ve seen, but it allowed me to inject Test Doubles and avoid the Singleton type of testing issues listed above.

# Singleton Implementation
The Singleton implementation appears simple, which is one of its issues.

## The Classic Implementation
Here is a Java equivalent of the C++ Singleton implementation from the GoF book:
```java
class SingletonA {
    private static SingletonA singleton = null;

    private SingletonA() {}

    public static SingletonA acquire() {
        if (singleton == null) {
            singleton = new SingletonA();
        }

        return singleton;
    }
}
```
The `SingletonA` class contains an internal static reference to an object of its own type, `singleton`. The constructor is `private` so that no other instance can execute it. The `static` `acquire()` method initializes the internal `singleton` if null, via the constructor, assigns and returns a reference. The GoF use `instance()` rather than `acquire()`, but I prefer the latter name, since it doesn’t suggest the underlying creation mechanism. `getInstance()` is another popular method name you’ll see.

It’s nice and simple, but it contains a flaw. It is not thread safe. If multiple initial threads execute the `singleton` null check at the same time, all will find it null and initialize `singleton`. Multiple instances of `SingletonA` will be initialized, but only the last one will be retained.

## The Thread Safe Implementation
The addition of `synchronized` makes the previous implementation thread safe:
```java
class SingletonB {
    private static SingletonB singleton = null;

    private SingletonB() {}

    public synchronized static SingletonB acquire() {
        if (singleton == null) {
            singleton = new SingletonB();
        }

        return singleton;
    }
}

```

But this comes at a minor performance cost. `acquire()` has become single threaded, which incurs an expense by adding `synchronized`. I suspect that for most applications, the cost is worth the protection.

## The Double-Check Locked Implementation
The [double-check locked]( https://en.wikipedia.org/wiki/Double-checked_locking) implementation looks like the best of both worlds:
```java
class SingletonC {
    private static SingletonC singleton = null;
    private final static Object lock = new Object();

    private SingletonC() {}

    public static SingletonC acquire() {
        if (singleton == null) {
            synchronized (lock) {
                if (singleton == null) {
                    singleton = new SingletonC();
                }
            }
        }

        return singleton;
    }
}

```
The `synchronized` lock is only needed when `singleton` is not initialized. Once it’s initialized, with thread safety, then we never have to execute the `synchronized` code again.

Looks great, right? It has a very nasty problem, which can be tricky to understand. __NOTE:__ This problem is language specific. It was a problem in C++ at one time. It might have been a problem in Java at one time. I don’t know if it’s a problem now.

Here is the nasty problem. Constructors do three things:
* Allocate memory from the heap.
* Initialize the returned memory via executing the constructor.
* Return a reference to the memory location.

In some languages, order was not specified. While the first bullet has to be first, for some languages, they returned the reference before the memory was initialized.

Therefore, it’s possible for two threads to execute this code simultaneously. The first thread will call the constructor, allocate the memory and potentially assign `singleton` before the constructor has initialized the memory.

While the first thread is still initializing the memory, a second thread proceeds. Since `singleton` is not null, it resolves the reference by returning `singleton`, which may still be in process of being initialized. This means that the second thread may start to invoke a method on the uninitialized `singleton`, which might throw a bizarre exception. Good luck repeating that exception and figuring it out.

You can read more about this issue in [C++ and the Perils of Double-Checked Locking](https://www.aristeia.com/Papers/DDJ_Jul_Aug_2004_revised.pdf) by Scott Meyers and Andrei Alexandrescu. Unless you know with 100% certainty that your language does not have an issue with double-checked locking, then don’t do it.

## The Initialize at Start Up Implementation
The previous implementations use lazy initialization. The Singleton is not initialized until it’s needed. I suspect that in most applications, Singletons will always be needed, so don’t use lazy initialization. Initialize it statically.

```java
class SingletonD {
    private static SingletonD singleton = new SingletonD();

    private SingletonD() {}

    public static SingletonD acquire() {
        return singleton;
    }
}
```

## The Definitive Implementation, Maybe
I read about this implementation in [SourceMaking.com]( https://sourcemaking.com/design_patterns/singleton/java/1):
>University of Maryland Computer Science researcher Bill Pugh has written about the code issues underlying the Singleton pattern when implemented in Java. Pugh's efforts on the "Double-checked locking" idiom led to changes in the Java memory model in Java 5 and to what is generally regarded as the standard method to implement Singletons in Java. The technique is known as the initialization on demand holder idiom, is as lazy as possible, and works in all known versions of Java. It takes advantage of language guarantees about class initialization, and will therefore work correctly in all Java-compliant compilers and virtual machines.
>
>The inner class is referenced no earlier (and therefore loaded no earlier by the class loader) than the moment that getInstance() is called. Thus, this solution is thread-safe without requiring special language constructs (i.e. volatile or synchronized).

```java
class SingletonE {
    private SingletonE() {}

    private static class SingletonHolder {
        private static final SingletonE singleton = new SingletonE();
    }

    public static SingletonE acquire() {
        return SingletonHolder.singleton;
    }
}
```

## Complete Code
A complete set of the code snippets above reside at [Singleton Implementations](#singleton-implementations). It also contains some of the code snippets listed below as well.

## Implementation Recommendation
Singleton seems like a simple implementation, but it can burn you. Look up implementations for other programming languages. Both resources below contain references to Singleton implementations in several languages at the bottom of each page:
* [Singleton via SourceMaking.com](https://sourcemaking.com/design_patterns/singleton)
* [Singleton via Refactoring.guru](https://refactoring.guru/design-patterns/singleton)

# What Happened to Class Encapsulation?
Most [creational design patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html) encapsulate the class type from the user, which adheres to the GoF's first design principle: [_Program to an interface, not an implementation_](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html#program-to-an-interface-not-an-implementation). Yet Singleton has violated this principle in almost every use of it that I’ve seen in production code. Almost every example I’ve seen in production code obtains a Singleton with the `acquire()`, `instance()`, or `getInstance()` method returning an instance of its own class.

As a result, almost every use of Singleton I’ve seen in production code is tightly coupled to the Singleton class, and since Singletons tend be coupled to external dependencies, the application code that uses this technique is coupled to those external dependencies as well. It’s no different than obtaining an instance via `new()`, except that with Singleton, there’s only one instance ever created.

A Singleton instance is so easy to obtain that it can be referenced almost anywhere, which can make maintenance more challenging. Notice that every reference for intent above contains a version of the following:
>Provides a global access point

Singleton is often used as an inlined variable so that they are almost hidden in plain sight such as:
```java
System.out.println(“The value is =” + Singleton.instance().toString());
```

Singleton’s solitary  instances tend to be associated with external dependencies, such as System Clock, File System, Database, etc. Providing a class specific Singleton not only violates encapsulation, it violates a lot of the principles that I championed in the [Hexagonal Architecture Series](https://jhumelsine.github.io/table-of-contents#hexagonal-architecture-aka-ports-and-adapters-design). Specifically, applications have direct knowledge and dependency upon externals.

In defense of the GoF, they do provide a ___Registry___ example, which looks somewhat like a [Factory Method](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html), which addresses this to some degree, but it comes too late in their Singleton description. Singleton’s relatively simple implementation is easy enough to understand, that I suspect many GoF readers read it, understood it, and then didn’t continue reading the rest of Singleton section, which contained more the subtle points.

## First Design Principle
Michael Feathers devotes a chapter in [Working Effectively with Legacy Code]( https://jhumelsine.github.io/2025/03/24/legacy-code.html) to the struggles of testing with Singletons.

One of his suggestions harkens back to the GoF’s first design principle to [_Program to an interface, not an implementation_](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html#program-to-an-interface-not-an-implementation). Other than the single instance restriction of Singleton, it is just like any other class. A Singleton can implement an interface.

Here’s an example:
```java
interface MyFeature {
    String getMessage();
}

class SingletonF implements MyFeature {
    private SingletonF() {}

    private static class SingletonHolder {
        private static final SingletonF singleton = new SingletonF();
    }

    public static SingletonF acquire() {
        return SingletonHolder.singleton;
    }

    @Override
    public String getMessage() {
        return "A Message from SingletonF";
    }
}
```

Here it is being instantiated and invoked:
```java
MyFeature myFeature = SingletonF.acquire();
System.out.println(myFeature.getMessage());
```

This is closer to what we want, but not quite there. The instantiation above still references the Singleton. We can encapsulate it within a [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html):
```java
class MyFeatureFactory {
    public static MyFeature acquire() {
        return SingletonF.acquire();
    }
}
```

The application no longer references the Singleton type directly:
```java
MyFeature myFeature = MyFeatureFactory.acquire();
System.out.println(myFeature.getMessage());
```

## Dependency Injection
I’ve only kicked the can down the road a bit. Even if the Singleton type is encapsulated, the above code still depends upon it indirectly. This code depends upon `MyFeatureFactory` which depends upon `SingletonF`. And if `SingltonF` were an external dependency, I’d be stuck, unless using the Mockito Static Methods, which is a sign of a design issue.

I’m no closer to being able to inject my own Test Double. It’s similar to the problem I described in the beginning of [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) where my team could only test our software in the lab emulating the entire environment because of our tight dependencies.

We can address this by using an [Injected Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer). The application would look something like this, where `MyFeature` is injected into it:
```java
class MyFeatureApp {
    private final MyFeature myFeature;

    public MyFeatureApp(MyFeature myFeature) {
        this.myFeature = myFeature;
    }

    public void print() {
        System.out.println(myFeature.getMessage());
    }
}
```

The Singleton injecting Configuration would look something like this:
```java
MyFeatureApp myFeatureApp = new MyFeatureApp(SingletonF.acquire());
myFeatureApp.print();
```

We  can even encapsulate the Singleton by using a Factory:
```java
MyFeatureApp myFeatureApp = new MyFeatureApp(MyFeatureFactory.acquire());
myFeatureApp.print();
```

This is very similar to what [Spring’s @Autowired](https://www.baeldung.com/spring-autowire) does. I don’t know if all @Autowired references are resolved with Singletons, but many are.

## Is That All There Is?

While this works, because we can inject Test Doubles into `MyFeatureApp`, it leaves me a bit unsatisfied. Do we even need a Singleton? A Configurer only injects one instance anyway. The only reason to implement a Singleton would be to ensure that there’s only one instance across multiple Configurers that are injecting the single-instance domains, such as Databases, File Systems and other external dependencies. Since [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) injects a single instance into the application, Singleton feels a bit redundant in that case.

However, if we were to desire the ability for an application to call `acquire()` repeatedly upon a single instance, then we can still provide this by injecting an [Abstract Factory](https://jhumelsine.github.io/2025/07/30/abstract-factory.html). Here is the basic implementation where `AbstractFeatureFactory` declares a method that returns a `MyFeatureFactory`. The `AbstractFeatureFactoryDemo` implements `AbstractFeatureFactory` and `acquireMyFeatureFactory()` returns `MyFeatureFactory`:
```java
interface AbstractFeatureFactory {
    MyFeatureFactory acquireMyFeatureFactory();
}

class AbstractFeatureFactoryDemo implements AbstractFeatureFactory {
    @Override
    public MyFeatureFactory acquireMyFeatureFactory() {
        return new MyFeatureFactory();
    }
}
```

Here is some sample code that references an `AbstractFeatureFactory` injected into it. The `print()` method has to go through two factories just to finally get the message:
```java
class MyFeatureAppB {
    private final AbstractFeatureFactory abstractFeatureFactory;

    public MyFeatureAppB(AbstractFeatureFactory abstractFeatureFactory) {
        this.abstractFeatureFactory = abstractFeatureFactory;
    }

    public void print() {
        MyFeatureFactory myFeatureFactory = abstractFeatureFactory.acquireMyFeatureFactory();
        for (int i = 0; i < 5; i++) {
            System.out.println(myFeatureFactory.acquire().getMessage());
        }
    }
}
```

And finally, here's the code that creates and injects the abstract factory into `MyFeatureAppB`:
```java
MyFeatureAppB myFeatureApp = new MyFeatureAppB(new AbstractFeatureFactoryDemo());
myFeatureApp.print();
```

Technically this works. This code calls `myFeatureFactory.acquire()` repeatedly, and it will always acquire the same Singleton instance, but I can't help feeling that it's overengineered. I would only implement to this level of multiple indirections if I had no other options.

As I look at this implementation, I keep hearing this quote in my head:
>_All problems in CS can be solved by another level of indirection, except for the problem of too many layers of indirection._ — David Wheeler

# Internal State
Singleton instances can have state. But since there's one instance for all clients, the state needs to apply to all. For example, a Print Spooler Singleton might contain the printer queue. Therefore, if multiple clients submitted jobs to be printed, then they would be queued until printed. There might even be a method to obtain the list of jobs currently the queue, and any client that requested the print queue would see all jobs that had been submitted.

## You Did What? Why?
I worked on a [middleware](https://en.wikipedia.org/wiki/Middleware) project for about twenty years ago, which was to be used by hundreds if not thousands of developers creating applications that would run upon our middleware platform. Our middleware was a bespoke super-charged operating system. It extended beyond typical operating system features by providing Communications, Fault Management, among other supporting featuress as a single cohesive platform for our domain-specific application developers. We were encouraged to use our own product within the middleware with the only restriction that we could not reference any code that was higher than our code in the architecture stack for our middleware.

One of our components was the __ConfigurationManager__. Configuration values could be declared in an [XML](https://en.wikipedia.org/wiki/XML) file. The ConfigurationManager would read the XML files, and provide an API to retrieve values. Its use looked something like this:
```java
ConfigurationManager configurationManager = new ConfigurationManager(); // I can't remember if we had to specify the XML files, or whether it knew where to find them.
int timeout = configurationManager.get("TimeOut");
```

We could even update values in the ConfigurationManager, looking something like this:
```java
ConfigurationManager configurationManager = new ConfigurationManager();
configurationManager.set("TimeOut", 15);
```

I wanted to modify the timeout on a communication channel to something like 15 seconds rather than the default 30 seconds, which I did with code that similar to what I've shown above. Someone from the ConfigurationManager team, which was a sibling team in my department, contacted me about my code. Our conversation went something like this:

>__Configuration Manager Dev (CM Dev)__: You changed the TimeOut value.
>
>__Me__: Yes. I want a shorter TimeOut.
>
>__CM Dev__: You can't do that. You'll change it for everyone.
>
>__Me__: What do you mean I can't do that? You provided the API that allows me to do it. Besides, it won't affect others because I'm only changing my local ConfigurationManager object.
>
>__CM Dev__: I know it looks like you have your own ConfigurationManager object, but there's a Singleton implementation within it. All of the configuration values reside in one place and they're shared. When you change "your" TimeOut value, you changed it for everyone.
>
>__Me__: _Pause_ ... So let me get this straight. You provided the ability for me to change a value with your API, but I'm not supposed to use it, because it will change the value for everyone, because it's really a Singleton, and there's no indication anywhere that it's a shared Singleton. Do I have that right?
>
>__CM Dev__: Yes.
>
>__Me__: Do you think that's a good idea? We're in the same department, and I had no idea that this could happen. Your API allows it. The documentation does not suggest that updating a configuration value will affect all other clients. We all have what appears to be our own ConfigurationManager instance. What are the odds that one of the hundreds or thousands of our application developers will do exactly what I have done? Are you going to review code from thousands of developers to make sure they haven't done what I have done.
>
>__CM Dev__: _Silence_

This is an excellent example of the [Principle of Least Astonishment](https://en.wikipedia.org/wiki/Principle_of_least_astonishment) (POLA).

The only way to get around this issue was to create a bespoke XML file with my own TimeOut value. It was a pain, since the XML grammar rules were extremely confusing. I never figured them out. The XML parser developer was on my team. Whenever I needed a new XML file, I would describe what I needed to him, and he'd guide me. I have no idea how our application developers would ever figure out how to use it.

In hindsight, I should have just accepted the default TimeOut value. 

This project was the poster child for POLA. I'm sure all include more POLA examples in the future. (TBD)

## Wrapping Singleton With State
There are probably many ways to provide the desired __ConfigurationManager__ behavior described above without the inadvertently changing the value for everyone.

Here's one possible way to do it with a wrapper. This is a hybrid of ideas from the [Proxy](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html) and [Chain of Responsibility](https://jhumelsine.github.io/2024/02/20/chain-of-responsibility-design-pattern.html) design pattern.

The bulk of the configuration values still reside within a Singleton instance as a Map of Strings to Integers. The `getInt(String name)` method returns the Integer that's associated with the String. This is an extremely simplistic example of the __ConfigurationManager__ on my project 20 years ago. Notice that there's no means to update any configuration value in `ConfigurationRepoSingleton`. The String to Integer mappings are created from XML files when the Singleton is initialized, which is not shown in the diagram.

`ConfigurationCache` is the wrapper that provides the additional ability to modify configuration values, but only for the client. It won't affect others. It sets a String to Integer mapping within itself using `setInt(String name, int value)`. Unlike the `ConfigurationRepoSingleton`, which is shared by all, each client would have its own `ConfigurationCache`, so each updated name/value mapping would be local to that client and it would not affect other clients.

`ConfigurationCache's` `getInt(String name)` is where the primary method that allows the wrapper to work. When executed, if the `name` is found in its mapping, then the local value is returned; otherwise, it returns the value that resides in the Singleton.

`ConfigurationManagerFactory` ensures that a `ConfigurationManager` object is acquired without the client having to know about `ConfigurationCache` or `ConfigurationRepoSingleton` specifically.

<img src="/assets/SingletonWrapper.png" alt="Singleton with Wrapper" width = "80%" align="center" style="padding-right: 35px;">

A complete implementation resides at [State Wrapped Singleton](#state-wrapped-singleton).

# State Injection
Singletons can contain state, but their state applies to all clients who share a reference to the Singleton instance as was seen with the original version of the __ConfigurationManager__. The solution shown above introduced a `ConfigurationCache` wrapper class that intercepted and satisfied the request without having to invoke the Singleton.

Most Singleton examples focus upon the creation of the sole Singleton instance, but that's only part of the story. Singleton classes also have methods for behavior. What if our Singleton request is more functional? What if we want Singleton to process with client specific state without that state impacting other clients?

Rather than depending upon state within the Singleton class when calling a functional method, pass state as an argument to the Singleton functional method.

State injection is not a new concept for this blog. It appeared as __Context__ within the [Specification Design Pattern](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html), which is a special case of the [Interpreter Design Pattern](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html). From [Specification Design Pattern](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html):
>Leaf objects in the composite tree are immutable. The non-terminal objects will be immutable once activated. The entire composite tree is a pure function. That means that Specification, and Composite in general, are thread-safe structures. Any number of threads can be calculating satisfiability simultaneously without concern of affecting each other. __State resides within the Context argument__.

Composite objects are immutable, but injecting Context into them allows them to vary their behavior based upon that state. Singletons and Composites are similar in that we can use the same technique to inject state while still maintaining their immutable properties. While this works, it can be a bit cumbersome to inject state, since it adds to the parameter list.

We can use a similar wrapper technique that was show above, except in this technique state resides within the wrapper and it's injected into the Singleton instance. This allows each client to have its own state within the wrapper and injected into the shared Singleton without affecting any other Singleton clients.

Let's expand the previous __ConfigurationManager__ scenario. I wanted to update `TimeOut` in the __ConfigurationManager__ without affecting other clients who preferred to use the configured `TimeOut` value.

Now I want to use my own `TimeOut` value within a communication `Channel`, which is designed as a Singleton. I want the ability to configure my own `TimeOut` value when sending messages via `Channel`, and I don't want to be overly burdened with `TimeOut` management overhead. Basically, I want to set the `TimeOut` value once, and then forget about it.

`Channel` defines a `send(String message)` interface. `ChannelSingleton`, which is mostly hidden in the design, defines `send(String message, int timeout)`, which allows us to send a message with an injected timeout. `ChannelWrapper` gets the `TimeOut` value from its injected `ConfigurationManager` and includes the `TimeOut` value when sending the message via `ChannelSingleton`.

Here's the design (Note: it only references the `ConfigurationManager` interface. Any design that implements it would suffice, but my implementation repeats the `ConfigurationManager` design from the previous section):

<img src="/assets/SingletonStateInjection.png" alt="Singleton with state injection" width = "80%" align="center" style="padding-right: 35px;">

A complete implementation resides at [State Injection](#state-injection).

# Memory Leaks
I have one last shot to take at Singleton. It leaks memory, maybe not a lot, but it does allocate resources that it never releases, even when clients are no longer accessing the Singleton. It's own static self reference is enough to prevent it from being cleaned up during garbage collection.

Most times this won't be an issue. It's not leaking memory repeated that will eventually crash the process. But it's possible for the Singleton to allocate a lot of memory or another limited resource, which will never be released. Consider if that were the case for a Singleton that's only accessed at start up. We'd end up with an allocated resource that's never released.

There is a way to address this at least in Java; however, I won't present it until the next blog entry, which will feature the Flyweight Design Pattern (TBD).

# Summary
As you can tell if you've made it this far, I'm not a big fan of Singleton. I rarely use it myself.

[Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) will cover many cases where Singleton would have previously been used. Rather than depending upon Singleton, a [Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer) can create and inject a single instance into an application.

# References
* [Wikipedia Singleton Design Pattern](https://en.wikipedia.org/wiki/Singleton_pattern)
* [Source Making Singleton Design Pattern](https://sourcemaking.com/design_patterns/singleton)
* [Refactoring Guru Singleton Design Pattern](https://refactoring.guru/design-patterns/singleton)
* [Singleton Design Pattern and 7 Ways to Implement it](https://blog.algomaster.io/p/singleton-design-pattern) - blog by Ashish Pratap Singh
* [Drawbacks of the Singleton Design Pattern](https://www.baeldung.com/java-patterns-singleton-cons) - Baeldung Article
* [What is Singleton Design Pattern -Tutorial with Practical Example (For Beginners)](https://www.youtube.com/watch?v=tItFlLprRMY) - Video by CodeBeauty
* [Singleton Pattern EXPLAINED IN 10 MINS: Javascript Design Patterns for beginners](https://www.youtube.com/watch?v=CWkD2kP6Wug) - Video by developedbyed
* [Singleton Pattern – Design Patterns (ep 6)](https://www.youtube.com/watch?v=hUE_j6q0LTQ) - Video by 
Christopher Okhravi
* and for more, Google: [Singleton Design Pattern](https://www.google.com/search?q=singleton+design+pattern)

# Complete Demo Code
Here’s the entire implementation up to this point as one file. Copy and paste it into a Java environment and execute it. If you don’t have Java, try this [Online Java Environment](https://www.programiz.com/java-programming/online-compiler/). Play with the implementation. Refactor some of the code.

## Singleton Implementations
The following code is a program containing all the Singleton implementation snippets, which appeared in [Singleton Implementation](#singleton-implementation).

```java
import java.util.*;

public class SingletonDemo {
    public static void main(String[] args) throws Exception {

        SingletonA singletonA = SingletonA.acquire();
        System.out.println(singletonA.toString());

        SingletonB singletonB = SingletonB.acquire();
        System.out.println(singletonB.toString());

        SingletonC singletonC = SingletonC.acquire();
        System.out.println(singletonC.toString());

        SingletonD singletonD = SingletonD.acquire();
        System.out.println(singletonD.toString());

        SingletonE singletonE = SingletonE.acquire();
        System.out.println(singletonE.toString());

        MyFeature myFeature1 = SingletonF.acquire();
        System.out.println(myFeature1.getMessage());

        MyFeature myFeature = MyFeatureFactory.acquire();
        System.out.println(myFeature.getMessage());

        MyFeatureAppA myFeatureApp1 = new MyFeatureAppA(SingletonF.acquire());
        myFeatureApp1.print();

        MyFeatureAppA myFeatureApp2 = new MyFeatureAppA(MyFeatureFactory.acquire());
        myFeatureApp2.print();

        MyFeatureAppB myFeatureApp = new MyFeatureAppB(new AbstractFeatureFactoryDemo());
        myFeatureApp.print();
    }
}

class SingletonA {
    private static SingletonA singleton = null;

    private SingletonA() {}

    public static SingletonA acquire() {
        if (singleton == null) {
            singleton = new SingletonA();
        }

        return singleton;
    }

    @Override
    public String toString() {
        return "SingletonA";
    }
}

class SingletonB {
    private static SingletonB singleton = null;

    private SingletonB() {}

    public synchronized static SingletonB acquire() {
        if (singleton == null) {
            singleton = new SingletonB();
        }

        return singleton;
    }

    @Override
    public String toString() {
        return "SingletonB";
    }
}

class SingletonC {
    private static SingletonC singleton = null;
    private final static Object lock = new Object();

    private SingletonC() {}

    public static SingletonC acquire() {
        if (singleton == null) {
            synchronized (lock) {
                if (singleton == null) {
                    singleton = new SingletonC();
                }
            }
        }

        return singleton;
    }

    @Override
    public String toString() {
        return "SingletonC";
    }
}

class SingletonD {
    private static SingletonD singleton = new SingletonD();

    private SingletonD() {}

    public static SingletonD acquire() {
        return singleton;
    }

    @Override
    public String toString() {
        return "SingletonD";
    }
}

class SingletonE {
    private SingletonE() {}

    private static class SingletonHolder {
        private static final SingletonE singleton = new SingletonE();
    }

    public static SingletonE acquire() {
        return SingletonHolder.singleton;
    }

    @Override
    public String toString() {
        return "SingletonE";
    }
}

interface MyFeature {
    String getMessage();
}

class SingletonF implements MyFeature {
        private SingletonF() {}

    private static class SingletonHolder {
        private static final SingletonF singleton = new SingletonF();
    }

    public static SingletonF acquire() {
        return SingletonHolder.singleton;
    }

    @Override
    public String getMessage() {
        return "A Message from SingletonF";
    }
}

class MyFeatureFactory {
    public static MyFeature acquire() {
        return SingletonF.acquire();
    }
}

class MyFeatureAppA {
    private final MyFeature myFeature;

    public MyFeatureAppA(MyFeature myFeature) {
        this.myFeature = myFeature;
    }

    public void print() {
        System.out.println(myFeature.getMessage());
    }
}

interface AbstractFeatureFactory {
    MyFeatureFactory acquireMyFeatureFactory();
}

class AbstractFeatureFactoryDemo implements AbstractFeatureFactory {
    @Override
    public MyFeatureFactory acquireMyFeatureFactory() {
        return new MyFeatureFactory();
    }
}

class MyFeatureAppB {
    private final AbstractFeatureFactory abstractFeatureFactory;

    public MyFeatureAppB(AbstractFeatureFactory abstractFeatureFactory) {
        this.abstractFeatureFactory = abstractFeatureFactory;
    }

    public void print() {
        MyFeatureFactory myFeatureFactory = abstractFeatureFactory.acquireMyFeatureFactory();
        for (int i = 0; i < 5; i++) {
            System.out.println(myFeatureFactory.acquire().getMessage());
        }
    }
}
```

## State Wrapped Singleton
The following code is an implementation for the design presented in [Wrapping Singleton With State](#wrapping-singleton-with-state).

It shows how `ConfigurationCache` allows the client to modify configuration values without affecting the default state values in `ConfigurationRepoSingleton`, which is shared by all clients.

```java
import java.util.*;

public class SingletonWithState {
    public static void main(String[] args) throws Exception {
        ConfigurationManager configurationManager1 = ConfigurationManagerFactory.acquire();
        System.out.println("Original TimeOut=" + configurationManager1.getInt("TimeOut"));
        configurationManager1.set("TimeOut", 15);
        System.out.println("Updated TimeOut=" + configurationManager1.getInt("TimeOut"));
        
        ConfigurationManager configurationManager2 = ConfigurationManagerFactory.acquire();
        System.out.println("Should be Original TimeOut=" + configurationManager2.getInt("TimeOut"));
    }
}

interface ConfigurationRepo {
    int getInt(String name);
}

class ConfigurationRepoSingleton implements ConfigurationRepo {
    private final Map<String, Integer> configurations = new HashMap<>();

    private ConfigurationRepoSingleton() {
        System.out.println("ConfigurationRepoSingleton initialization. Should only happen once.");
        configurations.put("TimeOut", 30); // This would be populated via an XML file.
    }

    private static class SingletonHolder {
        private static final ConfigurationRepoSingleton singleton = new ConfigurationRepoSingleton();
    }

    public static ConfigurationRepoSingleton acquire() {
        return SingletonHolder.singleton;
    }

    public int getInt(String name) {
        return configurations.get(name);
    }
}

interface ConfigurationManager extends ConfigurationRepo {
    void set(String name, int value);
}

class ConfigurationCache implements ConfigurationManager {
    private final Map<String, Integer> configCache = new HashMap<>();
    private final ConfigurationRepoSingleton configRepoSingleton = ConfigurationRepoSingleton.acquire();

    public int getInt(String name) {
        if (configCache.containsKey(name)) return configCache.get(name);
        return configRepoSingleton.getInt(name);
    }

    public void set(String name, int value) {
        configCache.put(name, value);
    }
}

class ConfigurationManagerFactory {
    public static ConfigurationManager acquire() {
        return new ConfigurationCache();
    }
}
```

## State Injection
The following code is an implementation for the design presented in [State Injection](#state-injection).


```java
import java.util.*;

public class ChannelSingletonWrapper {
    public static void main(String[] args) throws Exception {
        ConfigurationManager configurationManager1 = ConfigurationManagerFactory.acquire();
        Channel channel1 = ChannelFactory.acquire(configurationManager1);
        channel1.send("Message with default thirty second timeout.");

        ConfigurationManager configurationManager2 = ConfigurationManagerFactory.acquire();
        configurationManager2.set("TimeOut", 15);
        Channel channel2 = ChannelFactory.acquire(configurationManager2);
        channel2.send("Message with modified fifteen second timeout.");

        ConfigurationManager configurationManager3 = ConfigurationManagerFactory.acquire();
        Channel channel3 = ChannelFactory.acquire(configurationManager3);
        channel1.send("Message with default thirty second timeout.");
    }
}

interface Channel {
    void send(String message);
}

class ChannelFactory {
    public static Channel acquire(ConfigurationManager configurationManager) {
        return new ChannelWrapper(configurationManager);
    }
}

class ChannelWrapper implements Channel {
    private final ConfigurationManager configurationManager;
    private final ChannelSingleton channelSingleton = ChannelSingleton.acquire();

    public ChannelWrapper(ConfigurationManager configurationManager) {
        this.configurationManager = configurationManager;
    }

    public void send(String message) {
        channelSingleton.send(message, configurationManager.getInt("TimeOut"));
    }
}

class ChannelSingleton {
    private ChannelSingleton() {
        System.out.println("ChannelSingleton initialization. Should only happen once.");
    }

    private static class SingletonHolder {
        private static final ChannelSingleton singleton = new ChannelSingleton();
    }

    public static ChannelSingleton acquire() {
        return SingletonHolder.singleton;
    }

    public void send(String message, int timeOut) {
        System.out.format("Sending message=>>%s<< with timeout=%d\n", message, timeOut);
    }
}

interface ConfigurationRepo {
    int getInt(String name);
}

class ConfigurationRepoSingleton implements ConfigurationRepo {
    private final Map<String, Integer> configurations = new HashMap<>();

    private ConfigurationRepoSingleton() {
        System.out.println("ConfigurationRepoSingleton initialization. Should only happen once.");
        configurations.put("TimeOut", 30); // This would be populated via an XML file.
    }

    private static class SingletonHolder {
        private static final ConfigurationRepoSingleton singleton = new ConfigurationRepoSingleton();
    }

    public static ConfigurationRepoSingleton acquire() {
        return SingletonHolder.singleton;
    }

    public int getInt(String name) {
        return configurations.get(name);
    }
}

interface ConfigurationManager extends ConfigurationRepo {
    void set(String name, int value);
}

class ConfigurationCache implements ConfigurationManager {
    private final Map<String, Integer> configCache = new HashMap<>();
    private final ConfigurationRepoSingleton configRepoSingleton = ConfigurationRepoSingleton.acquire();

    public int getInt(String name) {
        if (configCache.containsKey(name)) return configCache.get(name);
        return configRepoSingleton.getInt(name);
    }

    public void set(String name, int value) {
        configCache.put(name, value);
    }
}

class ConfigurationManagerFactory {
    public static ConfigurationManager acquire() {
        return new ConfigurationCache();
    }
}
```
