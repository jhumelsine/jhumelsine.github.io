---
title: Dems D’FFACTS
description: Seven essential design patterns that should be in every developer’s toolbox
unlisted: true
---

<img src="/assets/craiyon_161039_abstract_artwork_showcasing_the_concept_of_strategy_adapter_with_UML_class_diagrams_i.png" alt="Abstraction of Essential Design Patterns" width = "50%" style="padding-right: 20px;">

# Gang of Four Struggles

I [struggled with the original Design Patterns book](https://jhumelsine.github.io/2023/08/24/its-your-move.html) by the Gang of Four (GoF). The detailed patterns are mostly presented in alphabetical order, which is fine for reference, but it didn’t work for me for learning the patterns. Several of the more complex and less frequently used patterns are near the beginning of the alphabet, whereas some of the less complex and more frequently used patterns are near the end of the alphabet. I became overwhelmed by the complex ones, and I was unable to continue through the book alone.

# Dems D'FFACTS

<img src="https://www.joepaduda.com/wp-content/uploads/2023/07/Screen-Shot-2023-07-31-at-7.37.32-AM.png" alt="Just the Facts Ma'am" width = "30%" align="right" style="padding-right: 20px;">

I [learned the design patterns](https://jhumelsine.github.io/2023/08/24/its-your-move.html) via other online resources. Some of the design patterns were like hammers and screwdrivers in my [toolbox](https://jhumelsine.github.io/2023/08/29/toolbox.html). I used them in many of my designs. Other patterns, not so much. They were great tools to have, but I didn’t use them as often.

Everyone will have their own favorite set of design patterns, but here are the seven that I personally feel are essential to all developers. When I rattled off some of them in a Zoom call a few weeks ago, my friend Scott Harden blurted out **FACTS** as an acronym, which covered most of them. When I considered the entire list, I had to make a few adjustments, but here they are in acronym order:
* **D**ependency Injection
* **F**açade
* **F**actory Method
* **A**dapter
* **C**ommand
* **T**emplate Method
* **S**trategy

# Seven Essential Design Patterns in Logical Order

While I love Scott’s acronym to help recall the set of essential design patterns, this is not the order in which I prefer to present them. Some of these design patterns are natural extensions of others. I will introduce them in what I consider to be logical order with **_highlights_** to their [basic OO components](https://jhumelsine.github.io/2023/09/01/parts-is-parts.html). (NOTE: The links are temporary. I will be creating blog entries for each individually and updating the links when posted):

<img src="" alt="Command" width = "20%" align="right" style="padding-right: 20px;">

* [Command](https://refactoring.guru/design-patterns/command) wraps a function in an object, so that a function can become a first-class citizen in an Object-Oriented (OO) language. By objectifying a function, it allows us to span time and space with functions. That is, we easily pass functions are arguments, schedule them for execution later in a queue, etc. Command is inheritance and polymorphism. There’s nothing special about it like most of these patterns. However, the pattern provides intent and context.

<img src="" alt="Strategy" width = "20%" align="right" style="padding-right: 20px;">

* [Strategy](https://refactoring.guru/design-patterns/strategy) is Command with multiple implementations for the interface. It’s structurally the same as Command, except with multiple ways to do the same thing. This is a scaled-up version of inheritance and polymorphism. Consider data structures. Many data structures implement an abstract Container, which declares basic Create, Read, Update and Delete operations. And there are several specific data structure classes that will implement those abstractions, such as List, Queue, Set. And even those classes will have more concrete specific classes, such as Linked List, Array List, Hash Set, Tree Set, etc. One interface and many potential implementations.
* [Template Method](https://refactoring.guru/design-patterns/template-method) is Strategy (inheritance and polymorphism) where an abstract base class at the “top” of the design, which replaces the interface of Strategy. This is done when most of the behavior is so consistent in different contexts that it can reside in a base class. However, the implementation still needs specific details to complete the behavior. These specifics reside in the inherited concrete classes. Framework designs are an example of Template Method.
* [Adapter](https://refactoring.guru/design-patterns/adapter) is Strategy (inheritance and polymorphism) where most of the implementation does not reside in the concrete Strategy class. Instead, concrete class delegates to another class to perform the desired behavior. This is often within the context of when the interface and the delegate class APIs are similar, but the method signatures are slightly different. Strategy is a type of plug-in design. The Adapter plugs into the Strategy interface and then delegates to the delegate class. Adapters allow delegate classes to do work within a Strategy, even when the delegate classes have no knowledge of the Strategy. Adapters translate the Strategy API into the delegate class API. I’ve often used Strategy and Adapter together in a partnership.
* [Façade](https://refactoring.guru/design-patterns/facade) is similar to Adapter, in that it delegates, but it tends to delegate to several classes, whereas Adapter tends to delegate to one. Façade might also use inheritance and polymorphism, but that’s optional. Façades are useful when delegate components that you must use are complicated and/or nasty to use. Rather than pollute your business logic with delegate API details, encapsulate them behind a streamlined bespoke interface to keep your business logic clean. The façade will still contain the complicated and/or nasty code, but this separation of concerns will make it easier to manage.
* [Factory Method](https://refactoring.guru/design-patterns/factory-method) is a means of acquiring a reference to an interface or abstract base class without calling `new` directly. This is done via a static method invocation. The `new` operation is still invoked to create a reference to a specific class, but it resides in a static method which is often part of a separate Factory Class. It encapsulates the class type from the client code.
* [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection) continues the theme of Factory Method. While Factory Method encapsulates the class type for object references returned, there’s still a dependency from the client code to the Factory Method and onto the actual class reference being returned. Dependency Injection removes that dependency by not allowing the client code to resolve its own external references. The external references are directly or indirectly resolved outside of the scope of the client and injected into the client, often as a constructor parameter. Dependency Injection is not in the GoF design pattern catalog I suspect because references to Dependency Injection seem to happen after the publication of the book. It may have been a technique at the time, but it may not have been widely known enough to have made it into the catalog.

# Summary

Most of these patterns demonstrate [Program to an interface, not an implementation](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html) in that most of them have an interface or abstract base class at the top of their diagrams.

Many of these patterns work well together. I think this is demonstrated best with [Hexagonal Architecture](https://www.youtube.com/watch?v=aayl6FysZ_U).
