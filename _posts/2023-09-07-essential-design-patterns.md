---
title: Dems D’FFACTS
description: Seven essential design patterns that should be in every developer’s toolbox
---

<img src="/assets/craiyon_161039_abstract_artwork_showcasing_the_concept_of_strategy_adapter_with_UML_class_diagrams_i.png" alt="Abstraction of Essential Design Patterns" width = "50%" style="padding-right: 20px;">

# Gang of Four Struggles

I [struggled with the original Design Patterns book](https://jhumelsine.github.io/2023/08/24/its-your-move.html) by the Gang of Four (GoF). The individual patterns are mostly presented in alphabetical order, which is fine for reference, but it didn’t work for me for learning the patterns. Several of the more complex and less frequently used patterns are near the beginning of the alphabet, and hence near the start of the book. Whereas some of the less complex and more frequently used patterns are near the end of the alphabet, and hence near the end of the book. I became overwhelmed by the complex ones, and I was unable to continue learning the design patterns via the book.

# Dems D'FFACTS

<img src="https://www.joepaduda.com/wp-content/uploads/2023/07/Screen-Shot-2023-07-31-at-7.37.32-AM.png" alt="Just the Facts Ma'am" title="Image Source: https://www.joepaduda.com/2023/07/31/just-the-facts-maam-2/" width = "30%" align="right" style="padding-right: 20px;">

I [learned the design patterns](https://jhumelsine.github.io/2023/08/24/its-your-move.html) via other online resources. Some of the design patterns are like hammers and screwdrivers in my [toolbox](https://jhumelsine.github.io/2023/08/29/toolbox.html). I use them in many of my designs. Other patterns, not so much. These less used tools are still great tools to have even if I don't use them as often.

Everyone will have their own favorite set of design patterns, but here are the seven that I personally feel are essential to all developers. When I rattled off some of them in a Zoom call a few weeks ago, my friend Scott Harden blurted out **FACTS** as an acronym, which covered most of them. When I considered the entire list, I made a few adjustments, and here they are in this acronym order:
* **D**ependency Injection
* **F**açade
* **F**actory Method
* **A**dapter
* **C**ommand
* **T**emplate Method
* **S**trategy

# Seven Essential Design Patterns in Logical Order

While I love Scott’s acronym to help recall the set of essential design patterns, this is not the order in which I prefer to present them. Some of these design patterns are natural extensions of others. I will introduce them in what I consider logical order with **_highlights_** to their [basic OO components](https://jhumelsine.github.io/2023/09/01/parts-is-parts.html):

<img src="/assets/SimpleCommand.png" alt="Command" width = "15%" align="right" style="padding-right: 20px;">

* The [Command](https://jhumelsine.github.io/2023/09/18/command-design-pattern.html) pattern wraps a function in an object, so that a function can become a first-class citizen in an Object-Oriented (OO) language.
By objectifying a function, we have more control over where a function is executed and by whom.
That is, we can easily pass functions are arguments, schedule them for execution later in a queue, etc. Command is **_inheritance_** and **_polymorphism_**.
There’s nothing special about it, like most of these patterns, other than it provides intent and context.

<img src="/assets/SimpleStrategy.png" alt="Strategy" width = "30%" align="right" style="padding-right: 20px;">

* The [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) pattern is a variant of the Command pattern with multiple implementations for the interface. It’s structurally the same as Command, except with multiple ways to do the same thing. This is a scaled-up version of **_inheritance_** and **_polymorphism_**. Consider data structures.  Many data structures implement an abstract Container, which declares Create, Read, Update and Delete operations. There are several specific data structure classes that will implement those abstractions, such as List, Queue, Set. And even those classes will have more concrete specific classes, such as Linked List, Array List, Hash Set, Tree Set, etc. One interface and many potential implementations.

<img src="/assets/SimpleTemplateMethod.png" alt="Template Method" width = "30%" align="right" style="padding-right: 20px;">

* [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) is Strategy (**_inheritance_** and **_polymorphism_**) except that an abstract base class at the “top” of the design replaces the interface of Strategy. This is done when most of the behavior is so consistent in different contexts that it can reside in a base class. However, the implementation still needs specific details to complete the behavior. These specifics reside in the inherited concrete classes. Framework designs are an example of Template Method.

<img src="/assets/SimpleAdapter.png" alt="Adapter" width = "20%" align="right" style="padding-right: 20px;">
  
* [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) is Strategy (**_inheritance_** and **_polymorphism_**) where most of the behavior does not reside in the concrete Strategy class. Instead, concrete class **_delegates_** to another class to perform the desired behavior. This is often within the context of when the interface and the delegate class APIs are similar, but the method signatures are different. Strategy is a type of plug-in design. The Adapter plugs into the Strategy interface and then delegates to the delegate class. Adapters allow delegate classes to do work within a Strategy, even when the delegate classes have no knowledge of the Strategy. Adapters translate the Strategy API into the delegate class API. I’ve often used Strategy and Adapter together in a partnership.

<img src="/assets/SimpleFacade.png" alt="Facade" width = "20%" align="right" style="padding-right: 20px;">

* [Façade](https://jhumelsine.github.io/2023/10/03/facade-design-pattern.html) is similar to Adapter, in that it **_delegates_**, but it tends to delegate to several classes, whereas Adapter tends to delegate to one. Façade might also use **_inheritance_** and **_polymorphism_**, but that’s optional. Façades are useful when delegate components that you must use have complicated and/or nasty APIs. Rather than pollute your business logic with delegate API details, encapsulate them behind a streamlined bespoke Façade to keep your business logic clean. The façade will still contain the complicated and/or nasty code, but this separation of concerns will make it easier to manage.

<img src="/assets/SimpleFactoryMethod.png" alt="FactoryMethod" width = "24%" align="right" style="padding-right: 20px;">
  
* [Factory Method](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) is a means of acquiring a reference to an interface or abstract base class without calling `new` directly. This is done via a **_static method invocation_**. The `new` operation is still invoked to create a reference to a specific class, but it resides in a static method which is often part of a separate Factory Class. It encapsulates the class type from the client code.

<img src="/assets/SimpleDependencyInjection.png" alt="Dependency Injection" width = "30%" align="right" style="padding-right: 20px;">
  
* [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) continues the theme of Factory Method. While Factory Method encapsulates the class type for object references returned, there’s still a dependency from the client code to the Factory Method and onto the actual class reference being returned. Dependency Injection removes that dependency by not allowing the client code to resolve its own external references. The external references are directly or indirectly resolved outside of the scope of the client and injected into the client, often as a constructor argument. Dependency Injection is not in the GoF design pattern catalog I suspect because references to Dependency Injection seem to occur after the publication of the book. It may have been a technique at the time, but it may not have been widely known enough to have made it into the GoF catalog.

# Summary

Most of these patterns demonstrate [Program to an interface, not an implementation](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html) in that most of them have an interface or abstract base class at the top of their diagrams.

Many of these patterns work well together. I think this is demonstrated best with [Hexagonal Architecture](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html), which is also known as Ports and Adapters.
