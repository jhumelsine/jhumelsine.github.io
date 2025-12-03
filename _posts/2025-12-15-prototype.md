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

### The Name
<img src="https://live.staticflickr.com/3058/2833972549_6505616f90_b.jpg" alt="Jengo and Boba Fett" title="Image Source: https://www.flickr.com/photos/datarknz/2833972549" width = "30%" align="right" style="padding: 35px;">

I've never been much of a fan of the name ___Prototype___. It's too easy to confuse it with a throwaway _prototype_ implementation. I think that ___Clone___, ___Copy___ or ___Breeder___ would have been better names. But this is the name chosen by the Gang of Four (GoF), so it's the one I'll be using.

Each Prototype concrete class implements the `copy()` command as it sees fit. The term ___copy___ implies that the concrete class will make a copy of itself. While that's an option, it's not a requirement. This is one of the reasons while I prefer the method name `acquire()`. It doesn't imply a mechanism. With this in mind maybe a better name for this pattern could have been __Offshoot__ or __Sprout__ by borrowing botany terms.

### Acquisition Via Object
Prototype doesn't acquires an object via an existing object by calling its `acquire()` method. The concrete class can use several mechanisms to implement `acquire()`. I'll provide several examples. Each will implement one of the following interface methods:
```java
Feature {
    Feature acquire();
    Feature acquire(State state);
}
```

#### Returns Object Itself
This technique returns the the source object itself.

```java
class FeatureImpl implements Feature {
    public Feature acquire() {
        return this;
    }
}
```

This is the most simple version, but it only works when `FeatureImpl` is an immutable Value Object (TBD), such as a [Singleton](https://jhumelsine.github.io/2025/10/31/singleton.html).

#### Returns Object via Default Constructor
This technique returns a new default object.

```java
class FeatureImpl implements Feature {
    private FeatureImpl() {}

    public Feature acquire() {
        return new FeatureImpl();
    }
}
```

A new object is instantiated using the default constructor. This will probably suffice in many cases.

#### Returns Object via Copy Constructor
This technique returns a new copied object.

```java
class FeatureImpl implements Feature {
    private FeatureImpl(FeatureImpl feature) {
        // Copy attributes from feature to this new object.
    }

    public Feature acquire() {
        return new FeatureImpl(this);
    }
}
```

This is the closest implementation to what's used in traditional Prototype presentations.

There are two considerations when using the copy constructor. Are the attributes copied via a ___shallow copy___ or ___deep copy___? This will depend upon the context, but I suspect that in most Prototype implementations the deep copy will be desired. A shallow copy will return a reference to the attribute, whereas a deep copy will return a copy of the attribute.

#### Returns Object with State
This technique returns a new object with state.

```java
class FeatureImpl implements Feature {
    private State state;

    private FeatureImpl(State state) {
        this.state = state;
    }

    public Feature acquire(State state) {
        return new FeatureImpl(state);
    }
}
```

This allows the newly created object to have its own state. `State` is a placeholder type. In an actual implementation, it could be a `String`, another class or even a list of arguments.

#### Returns Object with State via Copy Constructor
This is a hybrid of the State and Copy Constructor techniques shown above.
```java
class FeatureImpl implements Feature {
    private State state;

    private FeatureImpl(FeatureImpl feature, State state) {
        // Copy attributes from feature to this new object.

        this.state = state;
    }

    return Feature acquire(State state) {
        return new FeatureImpl(this, state);
    }
}
```

This is the technique that I'll use in the Use Case.

### Prototype Registry
But something is still amiss. We have a bit of a chicken and egg problem. Prototype uses objects to make copies of objects. We still need that first seed object as the first breeder. My examples only provided a comment `// Origins To Be Determined`. Where does it come from and how do clients access it?

A utility of the basic Shallow/Deep Prototype can be expanded by adding a **Prototype Registry**. A Prototype Registry contains a collection of Prototype objects, which can be copied. Each Prototype object in the registry has a key identifier, such as a String name, but any unique set of identifiers will suffice. When the client wants an object, it asks the registry to acquire one by name. The registry gets the object that matches the name, and returns a copy of the object.

The structure is almost identical to [Flyweight](https://jhumelsine.github.io/2025/11/14/flyweight.html), except that instead of returning the key matching object, which we would do with Flyweight, Prototype returns a copy of the matching object.

Examples for all of this will be forthcoming in the Design and Implementation.

# Prototype/Prototype-Registry Design and Implementation
Let's walk through a Prototype and Prototype Registry design and implementaiton one step at a time.

## Feature
`Feature` declares a contract interface:

<img src="/assets/Prototype1.png" alt="Feature Interface"  width = "20%" align="center" style="padding-right: 35px;">

The implementation is the same as the examples above, minus `copy()`, which will be in the next portion:
```java
interface Feature {
    void doSomething();
}
```

## Prototypical
`Prototypical` implements `Feature`. Like how [Flyweight](https://jhumelsine.github.io/2025/11/14/flyweight.html) contained a static repository within it, `Prototypical` contains a static repository within it too.

<img src="/assets/Prototype2.png" alt="Prototypical Abstract Class"  width = "50%" align="center" style="padding-right: 35px;">

`register(String name, Prototypical prototypical)` places a named `Prototypical` breeder into the breeders.

`acquire(String name)` searches for the `breeder` by name in the repository. If found, it returns an _acquired_ version of hte object.

I debated this with myself, but I have not used `clone()` or `copy()` names. I've used `acquire()`, which encapsulates the creational mechanism. __Prototype__ has another trick up its sleeve. Once a client has a `Prototypical` object, it can `acquire()` a new object from the acquired object, from which another object can be acquired, etc. There is no limit to how many new objects can be acquired regardless of how many generations they have descended from their repository ensconced breeder ancestor. This is a feature that other creational design patterns do not possess. I will feature this chained acquisition in the Use Case.

I have have decided to use `acquire()`, since clients may use it. `acquire()` is my preferred creation method name as I described in [The Sin of Omission, Revisited](https://jhumelsine.github.io/2025/11/28/object-pool.html#the-sin-of-omission-revisited).

```java
abstract class Prototypical implements Feature {
    private static final Map<String, Prototypical> breeders = new ConcurrentHashMap<>();

    public static Optional<Prototypical> acquire(String name) {
        Prototypical breeder = breeders.get(name);
        return breeder != null ? Optional.of(breeder.acquire()) : Optional.empty();
    }

    protected static void register(String name, Prototypical breeder) {
        if (breeders.containsKey(name)) throw new IllegalStateException("A breeder named '" + name + "' has already been registered");
        breeders.put(name, breeder);
    }

    public abstract Prototypical acquire();
}

```

At this point, the core functionality of the __Prototype/Prototype-Repository__ pattern is complete. Notice that there are no concrete classes. There's only an interface and an abstract class. `new()` is not called. If new classes are added to the design, they are registered to the repository. 

## Concrete Prototype Classes
While the core functionality is complete, it needs a few more elements. Concrete classes need to implement `Prototypical` and be registered into `Prototypical`.

<img src="/assets/Prototype3.png" alt="Concrete Prototypical Class"  width = "60%" align="center" style="padding-right: 35px;">

The diagram only has enough room to represent one concrete class, but the code sample shows how we can declare more classes. Each `Prototype` class acquires a shallow copy. Each class registers an instance of itself along with its chosen name, which is not the class name.

We can call `register()` from within a static block in C++, which will register an instance when loaded. Java will not do this. A similar static block will only register the instance when the class is referenced, which is a bit too late. An external entity must register `Prototypes` in Java.

```java
class PrototypeA extends Prototypical {
    private PrototypeA() {}

    @Override
    public Prototypical acquire() {
        return new PrototypeA();
    }

    public static void register() {
        register("A", new PrototypeA());
    }

    @Override
    public void doSomething() {
        System.out.println("PrototypeA does something that's A specific");
    }
}

class PrototypeB extends Prototypical {
    private PrototypeB() {}

    @Override
    public Prototypical acquire() {
        return new PrototypeB();
    }

    public static void register() {
        register("B", new PrototypeB());
    }

    @Override
    public void doSomething() {
        System.out.println("PrototypeB does something that's B specific");
    }
}
```

## Clients
Here is the complete design with Clients. I realize the diagram is inconsistent in its use of `Optional`.

<img src="/assets/Prototype4.png" alt="Complete Design with Clients"  width = "70%" align="center" style="padding-right: 35px;">

Here's the implementation, which registers the `Prototypes` statically and acquires them. I've added an unregistered type, __C__, to demonstrate that the repository may not have what you desire:
```java
public class PrototypeDemo1 {
    public static void main(String[] args) throws Exception {
        for (String name : List.of("A", "B", "C")) {
            FeatureFactory.acquire(name).ifPresentOrElse(
                Feature::doSomething,
                () -> System.out.println("Feature not found for name=" + name));

        }
    }

    static { // Register the Prototypes. NOTE: C has not been registered intentionally for the demo.
        PrototypeA.register();
        PrototypeB.register();
        // PrototypeA.register(); // This will throw an IllegalStateException since a Prototype named A has already been registered.
    }
}
```

## Review
This implementation is close to how I resolved class names with my [Parser](#interpreter-grammar-and-Parser-revisited). When we added a new functional class, we added a static blocked registration, it was in C++, and when the parser encountered an identifier for the class, the Prototype Repository would find the registered breeder and return a copy of it.

# Drawing Tool Use Case
<img src="/assets/Prototype5.png" alt="PowerPoint Slides"  width = "30%" align="right" style="padding-right: 35px;">

I render all of my UML class diagrams in PowerPoint. I had mentioned this in a previous blog entry where I presented [My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html). PowerPoint has it's advantages and disadvantages, but I found that it has been sufficient for my needs. PowerPoint's Drawing feature allows me to create shapes, connect them, color them, add/edit text, etc.

This Prototype Use Case will sketchout a design and implementation for some of the features of a drawing tool. It will include:
* A design for Shapes
* Using a __Prototype__ and __Prototype Repository__ to acquire Shames by name. This allows new Shape types to be added without having to update the Prototype portion of the design.
* The ability to group Shapes together, such as one can do with select in PowerPoint.
* The ability make copies of any Shapes.

In addition to __Prototype/Prototype-Repository__ this design will also feature elements of [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html), [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html), [Factory Method](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) and [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html).

## Shape
The contract is declared in Shape.

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

## Complete Prototype and Prototype Repository Implementation
```java
import java.util.*;
import java.util.concurrent.*;

public class PrototypeDemo1 {
    public static void main(String[] args) throws Exception {
        for (String name : List.of("A", "B", "C")) {
            FeatureFactory.acquire(name).ifPresentOrElse(
                Feature::doSomething,
                () -> System.out.println("Feature not found for name=" + name));

        }
    }

    static { // Register the Prototypes. NOTE: C has not been registered intentionally for the demo.
        PrototypeA.register();
        PrototypeB.register();
        // PrototypeA.register(); // This will throw an IllegalStateException since a Prototype named A has already been registered.
    }
}

interface Feature {
    void doSomething();
}

class FeatureFactory {
    private FeatureFactory() {}

    public static Optional<Prototypical> acquire(String name) {
        return Prototypical.acquire(name);
    }
}

abstract class Prototypical implements Feature {
    private static final Map<String, Prototypical> breeders = new ConcurrentHashMap<>();

    public static Optional<Prototypical> acquire(String name) {
        Prototypical breeder = breeders.get(name);
        return breeder != null ? Optional.of(breeder.acquire()) : Optional.empty();
    }

    protected static void register(String name, Prototypical breeder) {
        if (breeders.containsKey(name)) throw new IllegalStateException("A breeder named '" + name + "' has already been registered");
        breeders.put(name, breeder);
    }

    public abstract Prototypical acquire();
}

class PrototypeA extends Prototypical {
    private PrototypeA() {}

    @Override
    public Prototypical acquire() {
        return new PrototypeA();
    }

    public static void register() {
        register("A", new PrototypeA());
    }

    @Override
    public void doSomething() {
        System.out.println("PrototypeA does something that's A specific");
    }
}

class PrototypeB extends Prototypical {
    private PrototypeB() {}

    @Override
    public Prototypical acquire() {
        return new PrototypeB();
    }

    public static void register() {
        register("B", new PrototypeB());
    }

    @Override
    public void doSomething() {
        System.out.println("PrototypeB does something that's B specific");
    }
}
```

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
* named Prototype might not be found.
* Don't allow for duplicate names.
