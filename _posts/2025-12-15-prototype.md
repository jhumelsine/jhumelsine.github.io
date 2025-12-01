---
title: WORK IN PROGRESS – Prototype Design Pattern
description: TBD
unlisted: true
---

<img src="https://live.staticflickr.com/3069/2856121959_e773ec796c_b.jpg" alt="Lego Clone Troopers" title="Image Source: https://www.flickr.com/photos/adactio/2856121959" width = "50%" align="center" style="padding: 35px;">

# Introduction
In most mainstream Object-Oriented programming langauges, objects can only be instantiated through their constructors, which depends upon and requires knowledge of their concrete class types. [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) shifts the burden from the application code to a [Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer), but it doesn't eliminate the class dependency and knowledge. It just relocates it.

The [Creational Design Patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html) provide several techniques that encapsulate class details from the application or client code acquiring the object. However, the creation pattern mechanism depends upon and has knowledge of the class type. Each of the previous creational design patterns I've presented depends upon and has knowledge of the class type.

We've seen about a half dozen creational pattern mechanisms that encapsulate the class type from the application/client code. One of them will suffice for most of your object instantiation needs, but if we ever desire to add a new class type into the design, which many [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) based patterns support, then when a new concrete class is added to the design, the creational pattern will need to be updated as well.

**Prototype** is our final creational design pattern. It has a trick up its sleeve. It doesn't depend upon or have knowledge of class types. Prototype acquires a copy of an existing object without depending upon the objects' classes. The constructor call has not disappeared. We've just found another place to stash it. 

# Interpreter Grammar and Parser, Revisited
The [Grammar](https://jhumelsine.github.io/2024/05/14/interpreter-design-pattern-production.html#the-grammar) in the [Production Example](https://jhumelsine.github.io/2024/05/14/interpreter-design-pattern-production.html) for my [Interpreter Design Pattern Series](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) was pretty basic. The Scanner extracted [Keywords and Symbols](https://jhumelsine.github.io/2024/05/14/interpreter-design-pattern-production.html#keywords-and-symbols) as alphanumerics, but it didn't provide any context. The [Parser](https://jhumelsine.github.io/2024/05/14/interpreter-design-pattern-production.html#the-parser) determined the context.

Alphanumerics could be one of three categories in my Project Example's [Domain Specific Language](https://jhumelsine.github.io/2024/03/18/interpreter-design-pattern-dsls.html) (DSL). The parser checked for each category in succession:
* **Keywords** - Such as: `if`, `else`, `and`, `or` and `not`. These were identified and hardcoded directly in the parser. New keywords were very infrequent, but a few were added. I was usually able to add them to the parser implementation easily.
* **Identifiers** - These were entities defined within the script executing the DSL. They were equivalent to variables or function names in most programming languages. When a new identifier was defined in the script, it along with its definition was added to a symbol table, which the parser subsequently checked.
* **Class Names** - The DSL provided a structural framework organizing behavior across a set of objects identified by their class names. Most of the real work resided in the classes. The parser would need to create object instances for those class types. A [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) would have sufficied, as seen in the `switch` statement in [Factory Method](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html#factory-method), but that would mean that when a new class type was added, I would have had to update the `switch` statement in the factory used by the parser. I wanted to avoid updating the parser factory each time we added a class type, and I believe we added at least 50 class types over the lifespan of the project. I used prototype to acquire class named objects, and I never had to update the parser.

# Prototype
<img src="https://i1.pickpik.com/photos/426/49/136/thumb-favorite-hand-arm-preview.jpg" alt="Thumbs Up" title="Image Source: https://www.pickpik.com/human-right-hand-thumb-sign-favorite-hand-40472" width = "30%" align="right" style="padding: 35px;">

Prototype is completely different than the other [Creational Design Patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html). If the other creational design patterns are fingers, then Prototype is a thumb.

## Factory, Revisted
Let's briefly review the basic [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) pattern. The client acquires a `Feature` instance without knowing the class type; however, `static Feature FeatureFactory.acquire()` knows the `FeatureImpl` type. If a new concrete class is added to the design, then `FeatureFactory` will need to be modified.

```java
interface Feature {
    void doSomething();
}

class FeatureImpl implements Feature {
    FeatureImpl() {}

    @Override
    public void doSomething() {
        // Does something
    }
}

class FeatureFactory {
    public static Feature acquire() {
        return new FeatureImpl();
    }
}

...

// Client code
Feature feature = FeatureFactory.acquire();
```

## Basic Prototype
Prototype doesn't use a static method to acquire an object, which is common mechanism in most of the other [Creational Design Patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html). Prototype acquires an object from another breeder object directly by having it clone itself. Other creational design patterns encapsulate the constructor by placing the call to `new()` within a static method. Prototype encapsulates the constructor by placing the call to `new()` within the `copy()` method of an object of the desired type.

It's a little disorienting at first, but it's not complicated. A few lines of code demonstrate the fundamental principle.

```java
interface Feature {
    Feature copy();

    void doSomething();
}

class FeatureImpl implements Feature {
    FeatureImpl(){}

    @Override
    public Feature copy() {
        return new FeatureImpl();
    }

    @Override
    public void doSomething() {
        // Does something
    }
}

...

// Client Code
Feature breeder = // Origins To Be Determined

Feature feature = breeder.copy();
```

### Deep vs Shallow Copy
The `copy()` method above is a shallow copy. No state from the breeder is copied to the new object.

If the newly acquired object has state, then that can be accommodated passing `state` as an argument to `copy(State state)`:

```java
interface Feature {
    Feature copy(State state);

    void doSomething();
}

class FeatureImpl implements Feature {
    private State state;

    FeatureImpl(State state) {
        this.state = state;
    }

    @Override
    public Feature copy(State state) {
        return new FeatureImpl(state);
    }

    @Override
    public void doSomething() {
        // Does something
    }
}

...

// Client Code
Feature breeder = // Origins To Be Determined

Feature feature = breeder.copy(state);
```

We may want a deep copy, which I'll cover in the Use Case. That can be accomplished via an encapsulated copy constructor:
```java
interface Feature {
    Feature copy();

    void doSomething();
}

class FeatureImpl implements Feature {
    private State state;

    FeatureImpl(FeatureImpl featureImpl) {
        this.state = featureImpl.state;
    }

    @Override
    public Feature copy() {
        return new FeatureImpl(this);
    }

    @Override
    public void doSomething() {
        // Does something
    }
}

...

// Client Code
Feature breeder = // Origins To Be Determined

Feature feature = breeder.copy();
```

### Prototype Registry
But something is still amiss. We have a bit of a chicken and egg problem. Prototype uses objects to make copies of objects. We still need that first seed object as the first breeder. My examples only provided a comment `// Origins To Be Determined`. Where does it come from and how do clients access it?

A utility of the basic Shallow/Deep Prototype can be expanded by adding a **Prototype Registry**. A Prototype Registry contains a collection of Prototype objects, which can be copied. Each Prototype object in the registry has a key identifier, such as a String name, but any unique set of identifiers will suffice. When the client wants an object, it asks the registry to acquire one by name. The registry gets the object that matches the name, and returns a copy of the object.

The structure is almost identical to [Flyweight](https://jhumelsine.github.io/2025/11/14/flyweight.html), except that instead of returning the key matching object, which we would do with Flyweight, Prototype returns a copy of the matching object.

Examples for all of this will be forthcoming in the Design and Implementation.

# Design and Implementation

# Summary
TBD

# References
* [Wikipedia Prototype Design Pattern](https://en.wikipedia.org/wiki/Object_pool_pattern)
* [Source Making Prototype Design Pattern](https://sourcemaking.com/design_patterns/prototype)
* [Refactoring Guru Prototype Design Pattern](https://refactoring.guru/design-patterns/prototype)
* [Prototype Pattern in Java](https://www.baeldung.com/java-pattern-prototype) - Baeldung blog by Vivek Balasubramaniam
* [Simplify Your JavaScript Code With Prototype Design Pattern In JavaScript](https://www.calibraint.com/blog/prototype-design-pattern-in-javascript) - Blog by Vishnu Prasath
* [C# Prototype Design Pattern](https://www.dofactory.com/net/prototype-design-pattern) - Blog by dofactory.com
* [What is the Prototype Design Pattern?](https://www.youtube.com/watch?v=tqYQqSLLmEo) - Video by NeetCodeIO
* [Prototype Pattern in C++ (clone) - Creational Design Patterns](https://www.youtube.com/watch?v=x2DaAhZn20Y) - Video by Mike Shah
* [Prototype - Design Patterns in 5 minutes](https://www.youtube.com/watch?v=4KahRnQ39tw) - Video by levonog
* [Prototype Design Pattern Tutorial](https://www.youtube.com/watch?v=AFbZhRL0Uz8) - Video by Derek Banas
* and for more, Google: [Prototype Design Pattern](https://www.google.com/search?q=prototype+design+pattern)

# Complete Demo Code
Here’s the entire implementation up to this point as one file. Copy and paste it into a Java environment and execute it. If you don’t have Java, try this [Online Java Environment](https://www.programiz.com/java-programming/online-compiler/). Play with the implementation. Copy and paste the code into Generative AI for analysis and comments.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# NOTES
* It might not be a registered breeder.
* There could be multiple registrations for the same name.
* The mechanism does not need to know the class type.
* Use Interpreter Parser example.
* I don't like the name Prototype. It's the same as a Prototype version of a produce. Clone, Copy or Breeder would have been better IMHO.
* Basic copying
* Prototype Factory
* Shallow or Deep Copy
* Copy entire composite structures.
* Closing thoughts. Creational Patterns are not mutually exclusive. A design may incorporate several of them. For example, in Composite trees, the non-terminal composite classes might be acquired via a Factory while the terminal leaf nodes could be acquired via a Singleton, if they are stateless. And Prototype could be used to make a copy of the entire composite structure as seen in the use case.
