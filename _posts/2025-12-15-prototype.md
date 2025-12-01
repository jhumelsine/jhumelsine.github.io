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

## Factory, Revisted
Let's briefly review the basic [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) pattern. The client acquires a `Feature` instance without knowing the class type; however, the `FeatureFactory` knows the `FeatureImpl` type. If a new concrete class is added to the design, then `FeatureFactory` will need to be modified.

```java
interface Feature {
    void doSomething();
}

class FeatureImpl implements Feature {
    public void doSometing() {
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
