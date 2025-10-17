---
title: WORK IN PROGRESS – Singleton Design Pattern
description: The One. The Only. The Good, The Bad and the Ugly.
unlisted: true
---

# Introduction
When I interviewed job candidates, I’d ask them if they knew any Design Patterns. Most didn’t know what I was talking about. Some had heard of them, but they didn't know much more than that. A few had the Gang of Four (GoF) Design Pattern book but had never read it. I never held anyone’s lack of Design Pattern knowledge against them, since it was so prevalent. I had not learned the Design Pattern myself until my mid-forties, so I felt that shouldn't judge others negatively either.

When candidates could name a few Design Patterns, __Singleton__ seemed to be the pattern that was mentioned most often, even if they weren’t quite sure how to apply it. 

I suspect that most developers still may not know how to apply it, and Singleton’s description is in part to blame for this.

# I come to bury Singleton, not to praise it
I feel that incorporating design patterns into your designs will improve the quality of your code as I’ve described in previous [Design Pattern blog entries](https://jhumelsine.github.io/table-of-contents#design-patterns). I’m not convinced that Singleton falls into that category.

Singleton has its place in a design, but it’s a narrow spot. It’s often used outside of its narrow niche.

Singleton is simple. Almost too simple. It’s fraught with traps and pitfalls. Singleton’s misuse reduces to several issues:
* It’s easy to understand and use without fully grasping the implications of its use.
* It’s easy to implement, possibly too easy.
* It’s easy to implement incorrectly.
* It’s easy to reference from almost anywhere within a design, which can cause additional issues.

# Singleton’s Purpose in a Design
As I pointed out in [What is an Object?](https://jhumelsine.github.io/2025/09/03/what-is-an-object.html), Singleton’s purpose is to ensure that there’s no more than one instance for a class.

The GoF defined Singleton’s intent as:
>Ensure a class only has one instance, and provide a global point of access to it.

[Refactoring Guru’s](https://refactoring.guru/design-patterns/singleton) description is almost identical:
>__Singleton__ is a creational design pattern that lets you ensure that a class has only one instance, while providing a global access point to this instance.

[Source Making’s](https://sourcemaking.com/design_patterns/singleton) description adds a new nuance:
>* Ensure a class has only one instance, and provide a global point of access to it.
>
>* Encapsulated "just-in-time initialization" or "initialization on first use".

# Single Instance
There are valid domain model situations for which we would desire no more than one instance for a class, such as the system clock, the file system, a repository, etc.

# Considering Static Methods
Static methods are already associated with the class rather than an instance of the class. That is, for most Object-Oriented (OO) languages, we can access a static method without having to do so via an instance of the class. Static attributes, which are associated with the class rather than an instance of the class, are also accessible without the need for an instance of the class. Isn’t making everything static already a type of Singleton?

Technically, yes, it is. And we will see that Singleton’s mechanism uses a static method and static attribute, but Singleton is different, since it is an instance.

Static methods are great for [Pure Functions](https://en.wikipedia.org/wiki/Pure_function), such mathematical operations. They are a core concept to [Functional Programming](https://en.wikipedia.org/wiki/Functional_programming).

But objects object instances are first class citizens in OO languages. They can do things that static methods can’t do or can’t do as easily. They can be passed as arguments. They are more flexible. Since Singleton is an object instance, it can do things that static methods may not be able to do.

I worked on a project that used [MongoDB](https://en.wikipedia.org/wiki/MongoDB). Long before I joined the project, they had created static methods for each operation as wrappers around Mongo details. It was a type of [Façade](https://jhumelsine.github.io/2023/10/03/facade-design-pattern.html). While I appreciated not having to dive into Mongo details, it was a bit cumbersome to create and initialize [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html) for these static methods. Fortunately newer versions of [Mockito](https://site.mockito.org/) provide static method mocking. While it works, it’s not the most elegant mechanism I’ve seen, as I pointed out in [Mocking Frameworks](https://jhumelsine.github.io/2025/03/24/legacy-code.html#mocking-frameworks). 

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
The `SingletonA` class contains an internal reference to an object of its own type, `singleton`. The constructor is `private` so that no other instance can execute it. The `static` `acquire()` method initializes the internal `singleton`, if null, via the constructor, assigns and returns a reference. The GoF use `instance()` rather than `acquire()`, but I prefer the latter name, since it doesn’t suggest the underlying creation mechanism. `getInstance()` is another popular method name you’ll see.

It’s nice and simple, and it contains a flaw. It is not thread safe. If two threads are first to execute the null check at the same time, both will find it null and initialize `singleton`. Two instances of `SingletonA` will be initialized, but only the second one will be retained.

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

Constructors do three things:
* Allocate memory from the heap.
* Initialize the returned memory via executing the constructor.
* Return a reference to the memory location.

In some languages, order was not specified. While the first bullet has to be first, for some languages, they returned the reference before the memory was initialized.

Therefore, it’s possible for two threads to execute this code simultaneously. The first thread will call the constructor, allocate the memory and potentially assign `singleton` before the constructor has initialized the memory.

While the first thread is still initializing the memory, a second thread proceeds. The first null check is false, so it resolves the reference by returning `singleton`, which may still be in process of being initialized. This means that the second thread may start to invoke a method on the uninitialized `singleton`, which would most likely throw a bizarre exception. Good luck repeating that behavior and figuring it out. You can read more about this issue in [C++ and the Perils of Double-Checked Locking](https://www.aristeia.com/Papers/DDJ_Jul_Aug_2004_revised.pdf) by Scott Meyers and Andrei Alexandrescu. Unless you know with 100% certainty that your language does not have an issue with double-checked locking, then don’t do it.

## The Initialize at Start Up Implementation
The previous implementations use lazy initialization. The Singleton is not initialized until it’s needed. I suspect that in most applications, Singletons will be needed, so don’t use lazy initialization. Initialize it statically.

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
I read of this implementation in [SourceMaking.com]( https://sourcemaking.com/design_patterns/singleton/java/1):
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

## Implementation Recommendation
Singleton seems like a simple implementation, but it can burn you. Look up other implementations. Both resources below contain Singleton implementations in several languages at the bottom of each page respectively:
* [Singleton via SourceMaking.com](https://sourcemaking.com/design_patterns/singleton)
* [Singleton via Refactoring.guru](https://refactoring.guru/design-patterns/singleton)

# What Happened to Class Encapsulation?
Most [creational design patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html) encapsulate the class type from the user, which adheres to their first design principle: [_Program to an interface, not an implementation_](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html#program-to-an-interface-not-an-implementation). Yet Singleton has violated this principle in almost every use of it that I’ve seen in production code. Almost every example I’ve seen illustrates Singleton with the `acquire()`, `instance()`, `getInstance()`, method returning an instance of its own class.

As a result, almost every use of Singleton I’ve seen in production code is tightly coupled to the class. It’s no different than obtaining an instance via `new()`, except that with Singleton, there’s only one instance ever created.

Making matters worse, Singleton is often used as an inlined variable such as:
```java
System.out.println(“The value is =” + Singleton.instance().toString());
```

Technically we could do the same with `new()`, but we rarely see:
```java
System.out.println(“The value is =” + new MyClass().toString());
```

A Singleton instance is so easy to obtain that it can be referenced almost anywhere, which can make maintenance more challenging. Notice that every reference for intent above contains a version of the following:
>Provides a global access point

Singleton’s solitary  instances tend to be associated with external dependencies, such as System Clock, File System, Database, etc. Providing a class specific Singleton not only violates encapsulation, it violates a lot of the principles that I championed in the Hexagonal Architecture Series. Specifically, applications have direct knowledge and dependency upon externals.

In defense of the GoF, they do provide a ___Registry___ example, which looks somewhat like a Factory Method, which addresses this to some degree, but it comes too late. Singleton’s relatively simple implementation is easy enough to understand, that I suspect many GoF readers read it, understood it, and then didn’t continue reading the rest of Singleton section, which contained more subtle points.

Substituting [Test Double](https://jhumelsine.github.io/2024/07/02/test-doubles.html) for Singtons can be challenging, especially if writing your Test Doubles.

Mockito 3.4.0 provided the ability to inject Test Double behaviors into static methods without having to touch the implementation as described in this Mockito Static Method Tutorial. Even though Mockito provided this ability, the tutorial adds this [caveat](https://www.baeldung.com/mockito-mock-static-methods#a-quick-word-on-testing-static-methods):
>Generally speaking, some might say that when writing clean object-orientated code, we shouldn’t need to mock static classes. __This could typically hint at a design issue or code smell in our application.__
>
>Why? First, a class depending on a static method has tight coupling, and second, it nearly always leads to code that is difficult to test. Ideally, a class should not be responsible for obtaining its dependencies, and if possible, they should be externally injected.
>
>__So, it’s always worth investigating if we can refactor our code to make it more testable.__ Of course, this is not always possible, and sometimes we need to mock static methods.

## First Design Principle
Michael Feathers devotes a chapter in [Working Effectively with Legacy Code]( https://jhumelsine.github.io/2025/03/24/legacy-code.html) to the struggles of testing with Singletons.

One of his suggestions harkens back to the GoF’s first design principle. Other than the single instance restriction of Singleton, it is just like any other class. A Singleton can implement an interface. Here’s an example:
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

While this works, because we can inject Test Doubles into `MyFeatureApp`, it leaves me a bit unsatisfied. Do we even need a Singleton? A Configurer only injects one instance anyway. The only reason to implement a Singleton would be to ensure that there’s only one instance across multiple Configurers that are injecting the single-instance domains, such as Databases, File Systems and other external dependencies.

If we were to desire the ability for an application to call `acquire()` repeatedly, then we can still provide this by injecting an [Abstract Factory](https://jhumelsine.github.io/2025/07/30/abstract-factory.html). Here is the basic implementation where `AbstractFeatureFactory` declares a method that returns a `MyFeatureFactory`. The `AbstractFeatureFactoryDemo` implements `AbstractFeatureFactory` and `acquireMyFeatureFactory()` returns `MyFeatureFactory`:
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
        MyFeature myFeature = myFeatureFactory.acquire();
        System.out.println(myFeature.getMessage());
    }
}
```

And finally, here's the code that creates and injects the abstract factory into `MyFeatureAppB`:
```java
MyFeatureAppB myFeatureApp = new MyFeatureAppB(new AbstractFeatureFactoryDemo());
myFeatureApp.print();
```

Technically this works. This code can call `myFeatureFactory.acquire()` repeatedly, and it will always acquire the same Singleton instance, but I can't help feeling that it's over engineered. I would only go to this level of multiple indirections if I had no other options.

As I look at this implementation, I keep thinking of his quote:
>_All problems in CS can be solved by another level of indirection, except for the problem of too many layers of indirection._ — David Wheeler

# Unit Testing Concerns
Singleton is one instance ... everywhere. It's often embedded deep within the code, often within a statement as I've shown above. These 

# Summary
__TBD__
# References
_TBD_
# Complete Demo Code
_TBD_

++++++++++++++++++++++++++++++++++++++++++++++++
ConfigurationManager. Principle of Least Astonishment
Intrinsic state.
Proxy for local state.
Unit test concerns. One instance. Tests may interfere with each other. Difficult to inject test doubles.
