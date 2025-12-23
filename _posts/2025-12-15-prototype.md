---
title: DRAFT OPEN FOR REVIEW AND COMMENTS – Prototype Design Pattern; Acquisition Without Class Knowledge
description: Prototype is not about cloning objects. It’s about surviving change without rewriting factories.
unlisted: true
---

<img src="https://live.staticflickr.com/3069/2856121959_e773ec796c_b.jpg" alt="Lego Clone Troopers" title="Image Source: https://www.flickr.com/photos/adactio/2856121959" width = "50%" align="center" style="padding: 35px;">

# Introduction
In most mainstream Object-Oriented programming languages, objects can only be instantiated through their constructors, which depends upon and requires knowledge of their concrete class types. [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) shifts the burden from the application code to a [Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer), but it doesn't eliminate class dependency and knowledge. It just relocates it.

The [Creational Design Patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html) provide several techniques that encapsulate class details from the application or client code acquiring the object. However, the creation pattern mechanism itself depends upon and has knowledge of the class type. That knowledge becomes a maintenance hotspot as systems grow.

The problem is not object creation; __it’s ongoing system evolution__.

**Prototype** is our final creational design pattern. It has a trick up its sleeve. It doesn't depend upon or have knowledge of class types. Prototype doesn't acquire an object via a constructor directly. It acquires an object from an existing object of the type it desires. The constructor call has not disappeared. Prototype has found another place to stash it, that is, within the concrete class itself.

# Interpreter Grammar and Parser, Revisited
Prototype is easiest to understand in systems where _new types are added over time_, often by people who did not write the original framework. My [Interpreter/DSL project](https://jhumelsine.github.io/2024/05/14/interpreter-design-pattern-production.html) is a concrete example of this pressure.

The Production Example [Grammar](https://jhumelsine.github.io/2024/05/14/interpreter-design-pattern-production.html#the-grammar) for my [Interpreter Design Pattern Series](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) was basic. The Scanner extracted [Keywords and Symbols](https://jhumelsine.github.io/2024/05/14/interpreter-design-pattern-production.html#keywords-and-symbols) as alphanumerics, but it didn't provide any context other than determining an identifier. The [Parser](https://jhumelsine.github.io/2024/05/14/interpreter-design-pattern-production.html#the-parser) determined the semantic context.

Alphanumeric identifiers could be one of three categories in my Project Example's [Domain Specific Language](https://jhumelsine.github.io/2024/03/18/interpreter-design-pattern-dsls.html) (DSL). The parser checked for each category in succession:
* **Keywords** - Such as: `if`, `else`, `and`, `or` and `not`. These were identified and hardcoded directly in the parser. New keywords were added very infrequently, but a few were subsequently added. I was usually able to add them to the parser implementation easily.
* **Variable/Function Names** - These were entities defined within the script executing the DSL. They were equivalent to variables or function names in most programming languages. When a new identifier was defined in the script, it along with its definition was added to a symbol table, which the parser subsequently would query. If a name was found in the symbol table then its definition would be used.
* **Class Names** - The DSL defined a structural framework organizing behavior across a set of objects identified by their class names. Most of the real work resided in the classes. The DSL was the structural framework that organized objects of the class types in different configurations allowing a relatively small set of elements to exhibit a near infinite number of behaviors based upon how they were configured. The parser needed to create object instances for those class types. A [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) would have sufficed, as seen in the `switch` statement in [Factory Method](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html#factory-method), but that would mean that when a new class type was added, I would have had to update the `switch` statement in the factory used by the parser. I wanted to avoid updating the parser factory each time we added a class type, and I believe we added at least 50 class types over the lifespan of the project. I used Prototype to acquire class named objects, and I never had to update the parser when we added a new class type.

# Prototype
<img src="https://i1.pickpik.com/photos/426/49/136/thumb-favorite-hand-arm-preview.jpg" alt="Thumbs Up" title="Image Source: https://www.pickpik.com/human-right-hand-thumb-sign-favorite-hand-40472" width = "30%" align="right" style="padding: 35px;">

Prototype is completely different than the other [Creational Design Patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html). If the other creational design patterns are fingers, then Prototype is a thumb.

The Prototype _Thumb_ is different from the other creational pattern _Fingers_ in that it does not require:
* A static factory
* Switch/map of types
* Centralized creation logic updates
* Constructor calls in the creation mechanism

## Factory, Revisted
Let's briefly review the basic [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) pattern. The client acquires a `Feature` instance without knowing the class type. For example, when the client acquires a `Feature` instance using the `FeatureFactory` as such:
```java
Feature feature = FeatureFactory.acquire();
```
the client is encapsulated from the concrete class that implements `Feature`:
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
```

If a new `Feature` implementation class is introduced, this won't affect the client, but updates will be needed for `FeatureFactory`.

Factories encapsulate which class is created; Prototype eliminates the need to even know what the class types are.

## Basic Prototype
Prototype doesn't use a static method to acquire an object, which is the primary mechanism in most of the other [Creational Design Patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html). Prototype acquires an object from a breeder object by having it clone itself. Other creational design patterns encapsulate the constructor by placing the call to `new()` within a class static method. Prototype encapsulates the constructor by placing the call to `new()` within a non-static method accessed via an object instance of that class.

The client code would acquire a `Feature` object using Prototype as follows:
```java
Feature breeder = // Reference To Be Determined

Feature feature = breeder.acquire();
```

It's a little disorienting at first, but it's not complicated. A few lines of code demonstrate the fundamental principle.

```java
interface Feature {
    Feature acquire();

    void doSomething();
}

class FeatureImpl implements Feature {
    FeatureImpl(){}

    @Override
    public Feature acquire() {
        return new FeatureImpl();
    }

    @Override
    public void doSomething() {
        // Does something
    }
}
```

The constructor is not called from a Factory static method, which is often the case with the other creational patterns. With Prototype, the constructor is in an object method, which can be invoked from an object of the given type, but it often returns a reference to its interface definition, thereby maintaining encapsulation.

### The Name
<img src="https://live.staticflickr.com/3058/2833972549_6505616f90_b.jpg" alt="Jengo and Boba Fett" title="Image Source: https://www.flickr.com/photos/datarknz/2833972549" width = "40%" align="right" style="padding: 35px;">

I've never been much of a fan of the name ___Prototype___. It's too easy to confuse it with an early throwaway proof-of-concept _prototype_ implementation. I think that ___Clone___, ___Copy___ or ___Breeder___ would have been better names. But ___Prototype___ is the name chosen by the Gang of Four (GoF), so it's the one I'll be using.

Traditionally, each Prototype concrete class implements the `clone()` or `copy()` method as it sees fit. These method names imply that the concrete class will make a copy of itself. While that's an option, it's not a requirement. This is one of the reasons while I prefer the method name `acquire()`. It does not convey the creational mechanism as `clone()` or `copy()` would. From the client’s point of view, nothing is being _cloned_. Something is being _acquired_. The GoF emphasized cloning mechanics; I emphasize acquisition semantics.

Additionally, `clone` is a reserved word in Java, and using it adds some language baggage that I prefer to avoid. Maybe an even better name for this pattern could have been __Offshoot__ or __Sprout__ by borrowing botany terms.

### Acquisition Via Object
Prototype is more of a contract declaration than an implementation. It declares that a class that implements the interface must provide a method that returns an instance of the interface. It doesn't dictate how the class creates the object instance.

The concrete class has several options to implement `acquire()`. I'll provide several examples. Each will implement one of the following interface methods, but not both, which technically would be required if this were an implemented interface:
```java
Feature {
    Feature acquire();
    Feature acquire(State state);
}
```

#### Returns the Object Itself
This technique returns the source object itself.

```java
class FeatureImpl implements Feature {
    public Feature acquire() {
        return this;
    }
}
```

This is the most simple version, but it only works when `FeatureImpl` is an immutable Value Object (TBD), such as a [Singleton](https://jhumelsine.github.io/2025/10/31/singleton.html).

By _immutable value object_, I mean an object whose observable state cannot change after construction. It holds no mutable fields, exposes no setters, and does not represent an identity that evolves over time. In these cases, returning this is safe because sharing the instance cannot introduce cross-client interference. If mutation is possible, even indirectly, this technique should be avoided.

#### Returns the Object via Default Constructor
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

#### Returns the Object via Copy Constructor
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

#### Returns the Object with State
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

#### Returns the Object with State via Copy Constructor
This is a hybrid of the State and Copy Constructor techniques shown above.
```java
class FeatureImpl implements Feature {
    private State state;

    private FeatureImpl(FeatureImpl feature, State state) {
        // Copy attributes from feature to this new object.

        this.state = state;
    }

    public Feature acquire(State state) {
        return new FeatureImpl(this, state);
    }
}
```

This is the technique that I'll use in the Use Case (TBD).

#### Acquisition Summary

| Acquisition Technique | How It Works | When to Use |
|----------------------|-------------|-------------|
| Return `this` (Self) | `acquire()` returns the breeder object itself, not a new instance. | When the object is **immutable**, **stateless**, or a true **value object**. Also appropriate for **Singleton-like** semantics where sharing is intentional and safe. |
| Default Constructor | `acquire()` creates a new instance using the default constructor. | When the object has **no meaningful internal state**, or when each acquired instance should start from a **clean, identical baseline**. This is the simplest and most common Prototype form. |
| Copy Constructor | `acquire()` creates a new instance by copying the breeder’s fields. | When the breeder acts as a **template** and the new object should inherit its configuration. Use a **deep copy** when mutable fields are involved. |
| State-Based Acquisition | `acquire(state)` creates a new instance initialized with external state. | When the client must supply **context-specific data** at acquisition time, and the breeder primarily defines **behavior**, not configuration. |
| Copy + State Hybrid | `acquire(state)` copies the breeder and then applies additional state. | When the breeder provides a **default configuration**, but each acquired instance needs **customized runtime state**. Common in DSLs, parsers, and rule engines. |


# Prototype Registry
But something is still amiss. We have a bit of a chicken and egg problem. Prototype uses objects to make copies of objects. We still need that first seed object as the breeder. My example above only provided a comment `// Reference To Be Determined`. Where does it come from and how do clients access it?

Let's add a new concept: **Prototype Registry**. A Prototype Registry contains a collection of Prototype objects, which can be copied via their `acquire` method. Each Prototype object in the registry has a key identifier, such as a String name. When the client wants an object, it asks the registry to acquire one by name. The registry gets the object that matches the name and returns an acquired copy of the object.

There are several features and caveats to this Prototype Registry:
* The key identifier does not need to match the breeder object's class type name.
* The key identifier does not need to be a name. Any set of key attributes that uniquely identify a breeder object will suffice.
* Multiple key identifiers can map to a breeder object of the same class type.
* A unique key should map to no more than one breeder object.
* A queried key may not be registered. The registry will need to define how it will respond, which could be a returned `Optional`, `null` or a thrown Exception. The client will need to accommodate these ___Breeder Not Found___ cases.

The structure is almost identical to [Flyweight](https://jhumelsine.github.io/2025/11/14/flyweight.html), except that instead of returning the key matching object, which is what Flyweight does, Prototype returns an acquired copy of the matching object.

Examples for all of this will be forthcoming in the [Design and Implementation](#prototype-prototype-registry=design-and-implementation).

## Prototype Registry Lifecycle
A Prototype Registry is not just a data structure; it is a __lifecycle decision__. When and how breeders are registered determines the flexibility, safety, and testability of the system. Below are several common lifecycle strategies, each with tradeoffs.

| Registry Lifecycle Strategy | How Registration Occurs | Best Suited For | Key Tradeoffs |
|-----------------------------|-------------------------|----------------|---------------|
| Startup-Time Registration | All Prototypes are registered during application startup and remain fixed for the lifetime of the application. | Stable systems, core infrastructure, production deployments. | Least flexible; adding new Prototypes requires restart or redeploy. |
| Bootstrap / Initializer | A dedicated bootstrap class explicitly registers all Prototypes in a controlled sequence. | Medium to large systems; Java applications needing deterministic registration. | Centralized but can grow large; requires manual maintenance. |
| Module / Plugin-Based | Each module or plugin registers its own Prototypes when loaded or activated. | Extensible frameworks, plugin architectures, optional features. | Registration order and lifecycle must be carefully managed. |
| Dependency Injection–Assisted | A DI container creates and registers Prototypes as part of configuration or wiring. | Applications already using DI frameworks; testable, configurable systems. | Adds framework coupling; registry behavior depends on container lifecycle. |
| Test-Time Registration / Override | Prototypes are registered or replaced during test setup. | Unit, integration, and scenario-based testing. | Must ensure registry isolation between tests. |
| Dynamic Runtime Registration | Prototypes are added or removed while the system is running. | Long-running systems, rule engines, live-reconfigurable platforms. | Highest complexity; requires strict synchronization and invariant enforcement. |
| Controlled Global Registry | Registry exists as shared global state accessed via well-defined APIs. | Most Prototype implementations regardless of lifecycle choice. | Global state must remain intentional, documented, and constrained. |

## Prototype Registry Allows More Granularity
<img src="https://www.camelotgamestore.com/images/thumbs/0002417_panzer-blitz-quarter-page-of-general-magazine-v13n3-counters_550.jpeg" alt="Panzer Blitz Game Pieces" title="Image Source: https://www.camelotgamestore.com/panzer-blitz-quarter-page-of-general-magazine-v13n3-counters" width = "40%" align="right" style="padding: 35px;">

A Prototype Registry is a registry of objects. It's not a registry of classes. That means that the same class can be represented as different registered objects that vary in behavior based upon distinguishing attributes or their configuration. Prototype Registry would work well with [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html), since their behavior is defined via the assembly of a set of objects. The named root of the assembled objects can be registered. I'll provide an example of this in the Use Case (TBD).  While this is technically possible with some creational patterns, such as [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html), it feels more natural within a Prototype Registry.

Acquisition from this type of Prototype Registry should probably return a reference to the [breeder object](#returns-the-object-itself) or acquire a new one using the [copy constructor](#returns-the-object-via-copy-constructor) since its behavior is based upon its attributes.

This type of Prototype Registry might work well in a game. [Magic: The Gathering](https://en.wikipedia.org/wiki/Magic:_The_Gathering) consists of different types of cards, but for the most part, they vary based upon different values for the same set of attributes. [Board Wargames](https://en.wikipedia.org/wiki/Board_wargame), such as [Panzer Blitz](https://en.wikipedia.org/wiki/PanzerBlitz), also come to mind, where each piece's abilities depends upon its attributes for mobility, offense, defense, etc.

# Prototype/Prototype-Registry Design and Implementation
Let's walk through a Prototype and Prototype Registry design and implementation one step at a time.

## Feature
`Feature` declares a contract interface:

<img src="/assets/Prototype1.png" alt="Feature Interface"  width = "20%" align="center" style="padding-right: 35px;">

The implementation is the same as the examples above, minus `acquire()`, which will be in the next portion:
```java
interface Feature {
    void doSomething();
}
```

## Prototypical
`Prototypical` implements `Feature`. Just as [Flyweight](https://jhumelsine.github.io/2025/11/14/flyweight.html) contained a static repository within it, `Prototypical` contains a static repository within it too. `Prototypical` is both a Prototype _and_ a Registry façade. `Prototypical` plays two roles: it defines the prototype contract and hosts the registry that manages initial breeders.

<img src="/assets/Prototype2.png" alt="Prototypical Abstract Class"  width = "50%" align="center" style="padding-right: 35px;">

`register(String name, Prototypical prototypical)` places a named `Prototypical` breeder into the breeders.

`acquire(String name)` searches for the `breeder` by name in the repository. If found, it returns an _acquired_ version of the object.

__Prototype__ has another trick up its sleeve. Once a client has a `Prototypical` object, it can `acquire()` a new object from the acquired object, from which another object can be acquired, etc. There is no limit to how many new objects can be acquired regardless of how many generations they have descended from their repository ensconced initial breeder ancestor. This is a feature that other creational design patterns do not possess. I will feature this chained acquisition in the Use Case (TBD).

Chained acquisition allows objects to act as localized factories for their own variants. Once acquired, an object can refine or specialize itself further without consulting the registry again. This enables hierarchical or progressive specialization, something static factories and builders cannot do without reintroducing class knowledge.

Since there may not be a registered breeder, I have defined `acquire(String name)` to return an `Optional<Prototypical>` if a breeder is not found.

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

The diagram only has enough room to represent one concrete class, but the code sample shows how we can declare more classes. Each class registers an instance of itself along with its chosen name, which does not need to be the class name. It just needs to be unique in the registry. Rather than class self-registration, a class administrator could create the breeder instance and register it as well.

We can call `register()` from within a static block in C++, which will register an instance when loaded. Java will not do this. A similar static block will only register the instance when the class is referenced, which is probably too late. Therefore, an external entity must register `Prototypes` in Java.

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
Here is the complete design with Clients. I realize the diagram is inconsistent in its use of `Optional`. I had to make a few accommodations due to space constraints.

<img src="/assets/Prototype4.png" alt="Complete Design with Clients"  width = "70%" align="center" style="padding-right: 35px;">

Here's the implementation, which registers the `Prototypes` statically and acquires them. I went all in with Java's `Optional` syntax. I've added an unregistered type, __C__, to demonstrate that the repository may not have what you desire:
```java
public class PrototypeDemo1 {
    public static void main(String[] args) throws Exception {
        for (String name : List.of("A", "B", "C")) {
            FeatureProvider.acquire(name).ifPresentOrElse(
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
This implementation is close to how I resolved class names with my [Parser](#interpreter-grammar-and-Parser-revisited). When we added a new functional class, we added a static blocked registration. It was in C++, so registration happened automatically at start up. When the parser encountered an identifier for the class, the Prototype Repository would find the registered breeder and return a copy of it.

# Prototype Use Case
The next blog will feature a Prototype Use Case (TBD).

# When Not to Use Prototype
Prototype is not a default choice. If your system has a closed set of types, changes infrequently, or prioritizes simplicity over extensibility, a Factory or Builder will usually be clearer and easier to reason about. Prototype earns its complexity only when the cost of change dominates the cost of indirection.


# Summary
Prototype is not about cloning objects; it is about **decoupling object acquisition from concrete class knowledge**. By allowing objects to create other objects of their own kind, Prototype eliminates the need for centralized creation logic that must be updated as systems evolve.

This power comes with responsibility. Prototype introduces a registry, lifecycle decisions, and potential global state. Used carelessly, it can obscure system behavior. Used deliberately, it enables architectures that remain stable even as new types are introduced.

Choose Prototype when the cost of change outweighs the cost of indirection.
# References
* [Wikipedia Prototype Design Pattern](https://en.wikipedia.org/wiki/Prototype_pattern)
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
            FeatureProvider.acquire(name).ifPresentOrElse(
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

class FeatureProvider {
    private FeatureProvider() {}

    public static Optional<Prototypical> acquire(String name) {
        return Prototypical.acquire(name);
    }
}

// No switch, no reflection, no class knowledge.
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
