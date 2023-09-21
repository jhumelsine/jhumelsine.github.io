---
title: Strategy Design Pattern
description: When there’s more than one way to get the job done.
unlisted: true
---

<img src="https://i0.wp.com/s802022855.onlinehome.us/wp-content/uploads/2014/12/settlers_settled.jpg" alt="Settlers of Catan" width = "75%" style="padding-right: 20px;">

# Settlers of Catan Victory
Victory is achieved in Settlers of Catan by acquiring Victory Points received via different criteria in the game. Players may use different strategies to receive these points. There’s more than one way to win this game.

The Strategy Design Pattern supports multiple options as well.

# Strategy vs Command
The Strategy Design Pattern looks so much like the [Command Design Pattern](https://jhumelsine.github.io/2023/09/18/command-design-pattern.html) that you’d swear that they are the same. Both are based upon polymorphism. The difference is context and intent.

Command’s intent is the objectification of a function so that we can treat it as an object. The pattern doesn’t focus upon the nature of the function being objectified. Command’s primary concern is to make a function accessible anywhere an object is accessible.

Strategy’s intent is a more narrowly scoped contract focused upon what the client application wishes to accomplish. Strategy’s primary concern is to provide a means for the client application to accomplish its goal.

The contract, which is usually implemented as an interface, may consist of multiple cohesive methods. For example, if the contract’s single responsibility is persistence, then there may be four methods to Create, Read, Update and Delete business entities.

# Separation of Concerns and Modularity
Strategy leverages [Separation of Concerns](https://en.wikipedia.org/wiki/Separation_of_concerns) and [Modularity](https://en.wikipedia.org/wiki/Modular_programming). It separates **what** the client application wishes to accomplish from **how** it will be accomplished.

The Strategy interface contract should be defined from the client application’s point of view. It should convey **what** the client application needs, **not what** the implementation can provide.

The client application should neither know nor care about its concrete dependencies since it does not have a direct dependency upon those concrete classes. This provides a lot of flexibility. The client application will only have knowledge of and a reference to the Strategy interface.

<img src="/assets/SparkyTheCat.jpg" alt="Sparky The Cat" 
align="right" width = "20%" style="padding-right: 20px;">
There may be multiple concrete classes that implement an interface contract. That is, there’s more than one way to skin a cat, and I use this analogy with my sincerest apologies to Sparky T. Cat, who generally doesn’t appreciate my using that sort of language. The ***T.*** is for ***The***.

The client application’s Strategy reference is resolved at runtime, and it can be updated dynamically. What’s more, the client application can have a collection of Strategy interface references, and each reference can be resolved with references from different concrete classes.

Reference resolution doesn’t get as much attention in Strategy and other similar design patterns as it probably should. The Gang of Four (GoF) provide a few vague suggestions, but I don’t feel they provide clear guidance.

I will provide my own thoughts as I continue with [Essential Design Patterns](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html) blog entries, but I don’t want to get the cart before the horse. For now, assume that the Client Applications will have the Strategy references they require. Rest assured, I’ll provide blogs on how to achieve that. Spoiler Alert: I’ll cover this with [Factory Method](https://refactoring.guru/design-patterns/factory-method), [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection) and possibly more.

# Strategy’s Structure
Since a picture is worth a thousand words, here’s the traditional Strategy UML class diagram from [DoFactory.com](https://www.dofactory.com/net/strategy-design-pattern):

<img src="https://www.dofactory.com/img/diagrams/net/strategy.png" alt="Strategy UML Class Diagram" 
align="center" width = "70%" style="padding-right: 20px;">

Here are the elements and their relationships:
* `Strategy` is an interface.
* `AlgorithmInterface()` is a method that declares the contract that fulfills a need of the `Context` (AKA Client Application). There may be several cohesive methods in the contract, not just one as depicted here. _Algorithm_ is the term chosen by the GoF. It’s not my favorite term, but it appears repeatedly in descriptions of Strategy.
* `Context` is the Client Application, and I will use the term “Client Application” rather than “Context,” since I feel that it’s a more accurate term. The Client Application delegates behavior responsibility to the Strategy interface via a reference to a Strategy object. The Client Application will maintain this reference, often as a private field attribute. Delegating to an interface demonstrates the first design principle: [Program to an interface rather than an implementation](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html#program-to-an-interface-not-an-implementation).
* `ConcreteStrategyA`, `ConcreteStrategyB` and `ConcreteStrategyC` are three different implementations of the `Strategy` interface. There could be any number of concrete `Strategy` implementation classes. The design does not dictate how many there must be.

# The Shape of Things to Come
Strategy is polymorphism. Let’s return to one of our old Object-Oriented friends that’s often used to describe polymorphism – Shape. Shape works perfectly well as an example for Strategy as well. Do a Google Image search on [object-oriented programming shape UML](https://www.google.com/search?sca_esv=567284796&sxsrf=AM9HkKnfpfcU850pj7ZY3qvSDj_LdN8gVQ:1695306945452&q=object-oriented+programming+shape+UML&tbm=isch&source=lnms&sa=X&ved=2ahUKEwiKg5PC9ruBAxV7kIkEHbaJDYUQ0pQJegQIDBAB&biw=1536&bih=747&dpr=1.25) and you’ll see examples that mirror the UML diagrams I’ve presented here.

Here’s my version of a UML class diagram using Shape. The structure is almost identical to the traditional Strategy diagram shown above:

<img src="/assets/Shapes.png" alt="Shapes UML Class Diagram" 
align="center" width = "85%" style="padding-right: 20px;">
 
This is part of the design for a Computer Aided Design (CAD) client application. One of its operations is to render a set of shapes. We want to use Strategy so that the ComputerAidedDesign class does not depend upon the specific concrete Shape classes. 

`Shape` is a Strategy, and its contract is for the `Shape` to draw itself. 
While I didn’t add it, I could have also added additional contract operations such as perimeter and area with similar implementations in the concrete classes.

New Shapes, such as Oval, can easily be added without an impact upon existing classes. Oval would be another class that implements Shape.

# Testing
Even if there’s only one concrete class that the Client Application would ever delegate to, you may still want to consider Strategy, and that would be for testing the Client Application code.

Strategy’s primary concern is to provide a means for the Client Application to accomplish its goal. It’s about supporting the interaction between the Client Application and the Strategy interface contract without tightly coupling the Client Application to a Concrete Strategy class. Strategy also allows us to test the Client Application/Strategy interaction without that tight coupling as well.

The diagram below is a UML sketch of how that might be done. 
The `ComputerAidedDesign` and Shape elements are identical to the UML diagram above. The purple elements have been added for testing. `ComputerAidedDesignTest` runs a test scenario confirming that render draws shapes. `TestDoubleShape` emulates `Shape` behavior. It doesn’t draw anything, but it remembers that there had been a request for it to be drawn. I don’t want to drift too far astray from Strategy. Testing deserves its own set of blogs, which are in my backlog.

<img src="/assets/ShapeTesting.png" alt="Shape Testing UML Class Diagram" 
align="center" width = "95%" style="padding-right: 20px;">
 
The modularity of Strategy may facilitate the testing of the Concrete Strategy classes as well since they don’t have any dependencies upon the Client Application code.

# When to Consider Strategy
You could use Strategy for every class. That’s probably taking it a bit too far.
Consider Strategy when delegating to behavior that requires external dependencies, such as a Database or an Internet call. Consider Strategy when delegating to behavior that resides in components elsewhere in the system, such as a service provided by another team. Consider Strategy to keep your code more modular and cleaner.

I may or may not consider Strategy for a class within my control. It would depend upon the size and sophistication of the class. The larger and more sophisticated the class the more I’d consider using Strategy.

I would not consider Strategy for most utility classes, such as String. 

# Summary
Strategy separates Client Application implementation from Concrete Class dependencies. Concrete Class references are assigned at runtime. They may be assigned only once, or they may be reassigned at any time. The Client Application might have a Container of Strategy references, and each can have its own Concrete Class reference as was shown with the Shape example.

Strategy provides flexibility via Separation of Concerns and Modularity. Strategy supports decoupled testing.

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* [Wikipedia Strategy Design Pattern](https://en.wikipedia.org/wiki/Strategy_pattern)
* [Source Making Strategy Design Pattern](https://sourcemaking.com/design_patterns/strategy)
* [Refactoring Guru Strategy Design Pattern](https://refactoring.guru/design-patterns/strategy)
* [DoFactory Strategy Design Pattern](https://www.dofactory.com/net/strategy-design-pattern)
* and for more Google “Strategy Design Pattern”

Here are some resources that can be purchased or are included in a subscription service:
* [Gang of Four Strategy Design Pattern](https://learning.oreilly.com/library/view/design-patterns-elements/0201633612/ch05.html#page_315)
* Agile Principles, Patterns, and Practices in C#, Chapter 22 ([O'Reilly](https://learning.oreilly.com/library/view/agile-principles-patterns/0131857258/) and [Amazon](https://www.amazon.com/Agile-Principles-Patterns-Practices-C/dp/0131857258))
* Clean Code: Design Patterns, Episode 27 video ([Clean Coders](https://cleancoders.com/episode/clean-code-episode-27) and [O'Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_03_27_00/))
* Head First Design Patterns ([O'Reilly](https://learning.oreilly.com/library/view/head-first-design/9781492077992/ch01.html) and [Amazon](https://www.amazon.com/Head-First-Design-Patterns-Object-Oriented-ebook/dp/B08P3X99QP))
