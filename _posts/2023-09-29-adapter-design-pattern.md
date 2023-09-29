---
title: Adapter Design Pattern
description: What we've got here is... failure to communicate. Maybe there's a cool pattern that could give us a hand with that.
unlisted: true
---

![Electrical Plug Adapters](https://www.ceptics.com/cdn/shop/files/CT-11PK_B07DRBHRVM_REV_LI_IMG_01_600x600.jpg?v=1694694225)
 
# Introduction
You’re traveling internationally, and your electrical devices won’t work in your destination(s). You’ll need to take a set of electrical adapter plugs much like the ones shown above. You plug the adapter into the electrical outlet, and then you plug your device into the adapter. The adapter plug accommodates different outlet shapes as well as voltage, current frequency, and other differences.

The Adapter Design Pattern is much the same. It tends to be a relatively simple class that allows two classes to interact when their APIs don’t directly allow it.

Adapter continues the main theme of the [Command](https://jhumelsine.github.io/2023/09/18/command-design-pattern.html), [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) and [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) design patterns – polymorphism. But Adapter sometimes adds another trick, delegation or inheritance. Other design patterns use these, but Adapter uses them in a special way.

Adapter is about a change in the contract interface but not a change in behavior.

# Structure
The previous design patterns, mentioned above, contain extending classes, but for the most part, they aren’t the main features of those designs. Adapter features extending classes showing how they use delegation or inheritance to bridge the communication gap.

## Adapter Scenario
Before getting into the nuts-and-bolts, let’s start with a scenario. We start with a `Target` interface that declares a `request()` method.

<img src="/assets/AdapterTarget.png" alt="Target Interface" align="center" width = "25%" style="padding-right: 20px;">
 
We have a `Client` that follows the design principle to [Program to an interface, not an implementation](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html#program-to-an-interface-not-an-implementation), so it delegates to `Target`.

<img src="/assets/AdapterClientTarget.png" alt="Client Target Relationship" align="center" width = "50%" style="padding-right: 20px;">

If I were describing Command or Strategy, then the next diagram would show add an extending concrete class from `Target` that implements `request()` and we’d be done.
But in this scenario, the desired behavior has already been implemented in another class, `Service` via a method called `action()`. Now we have this:

<img src="/assets/AdapterScenario.png" alt="Adapter Scenario" align="center" width = "75%" style="padding-right: 20px;">
 
We have a few problems. Command and Strategy are based upon a plug-in technique where the extending concrete class plugs into the interface. The implementation needs to know about and implement the interface method(s). But `Service` doesn’t implement `Target`. We can’t plug it in.

What are our options?
* We could have `Client` access `Service` directly, but that would violate the first design principle. Additionally, there could be several other classes extending `Target` that `Client` is already using. We cannot change `Client`.
* We could change `Service` to extend `Target` and change its method name from `action()` to `request()`, but that probably won’t work:
    * `Service` could be an external element, and we have no control over it.
    * `Service` could be an internal element already in use in the product. Changing it could break other code.
    * `Service` could be legacy code that’s used by the rest of the legacy code. No one wants to touch legacy code.

## Two Adapter Design
Adapter can bridge this gap, and there are two variations of Adapter:
* Object Adapters
* Class Adapters

I think better terms would have been _Composition Adapters_ and _Inheritance Adapter_ respectively, but no one asked me. They compare and contrast in the same way that the Strategy and Template Method do. Both pairs solve the same problem, but in different ways with one favoring composition and the other favoring inheritance. Neither is necessarily better or worse than the other. Both mechanisms have their own strengths and weaknesses.

Regardless of the technique, Adapter allows two classes to communicate when they are not designed to communicate. In our scenario, the `Client` ultimately wants to access `Service`, but `Service` doesn’t fit in the `Client`/`Target` ecosystem. Adapters are translators. They translate the nomenclature of the `Client` with the nomenclature of the `Service`.

### Object Adapter
The Object Adapter is based upon composition and delegation. Here are the basic elements:
* `Target` is the interface contract. It declares `request()`.
* The `Client` has a reference to the `Target` interface, and it delegates to the `Target`’s `request()` method. The `Client` is programming to an interface, not an implementation.
* `Adapter` implements `Target`, and it must implement `request()`. It has a reference to `Service`, and it implements `request()` with a one-line method that calls `service.action()`.

<img src="/assets/ObjectAdapter.png" alt="Object Adapter" align="center" width = "75%" style="padding-right: 20px;">
 
That’s it … mostly. The field attributes still need to be resolved. I’ll get to a way to address that in a few blog posts. Please sit tight.

### Class Adapter 
The Class Adapter is based upon inheritance and tradition. Here are the basic elements:
* `Target` is the interface contract. It declared `request()`.
* The `Client` has a reference to the `Target` interface, and it delegates to the `Target`’s `request()` method. The `Client` is programming to an interface, not an implementation.
* `Adapter` implements `Target`, and it must implement `request()`. `Adapter` extends `Service`, but it only does this so that it has access to `action()`.

<img src="/assets/ClassAdapter.png" alt="Class Adapter" align="center" width = "75%" style="padding-right: 20px;">
 
We still need to resolve a field attribute, but it’s not quite as much to set up as with the Object Adapter technique.
Both Adapter approaches work. I tend to prefer Object Adapters, but Class Adapters work as well. It depends upon whether you prefer composition or inheritance.

# Pairs With Strategy
Adapter is rarely designed without it being paired with Strategy. Let me rephrase that. Adapter is Strategy. The only difference is that we have a little more implementation detail in Adapter in that it delegates to another class and translates API differences.
Do a Google Image Search for [Adapter Design Pattern UML class diagram](https://www.google.com/search?sca_esv=569265910&rlz=1C1EKKP_enUS733US733&sxsrf=AM9HkKkXNiPuOH_Y8M1pM4YYJTc4Gf-V6w:1695946688167&q=Adapter+Design+Pattern+UML+class+diagram&tbm=isch&source=lnms&sa=X&ved=2ahUKEwjct5_fxc6BAxX8MVkFHdbtDIUQ0pQJegQIDRAB&biw=1536&bih=747&dpr=1.25) and you’ll see the telltale signs for Strategy as well.

# Summary
Adapter allows one class to access another class even when their APIs differ. However, the adapted class needs to be similar enough in intent so that using an Adapter makes sense.
Adapters might need to account for argument types and return types as well, which was not shown in the diagrams here.

Adapter implementations tend to be small. Each method is usually only a few lines long.

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* [Wikipedia Adapter Design Pattern](https://en.wikipedia.org/wiki/Adapter_pattern)
* [Source Making Adapter Design Pattern](https://sourcemaking.com/design_patterns/adapter)
* [Refactoring Guru Adapter Design Pattern](https://refactoring.guru/design-patterns/adapter)
* [DoFactory Adapter Design Pattern](https://www.dofactory.com/net/adapter-design-pattern)
* [Project Management Institute Adapter Design Pattern](https://www.pmi.org/disciplined-agile/the-design-patterns-repository/the-adapter-pattern)
* and for more Google "Adapter Design Pattern”

Here are some resources that can be purchased or are included in a subscription service:
* [Gang of Four Adapter Design Pattern](https://learning.oreilly.com/library/view/design-patterns-elements/0201633612/ch04.html#page_141)
* Agile Principles, Patterns, and Practices in C#, Chapter 33 ([O'Reilly](https://learning.oreilly.com/library/view/agile-principles-patterns/0131857258/) and [Amazon](https://www.amazon.com/Agile-Principles-Patterns-Practices-C/dp/0131857258))
* Clean Code: Design Patterns, Episode 34 video ([Clean Coders](https://cleancoders.com/episode/clean-code-episode-34) and [O'Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_03_34_00/))
* Head First Design Patterns, Chapter 7 ([O'Reilly](https://learning.oreilly.com/library/view/head-first-design/9781492077992/ch07.html#adapter_pattern_defined) and [Amazon](https://www.amazon.com/Head-First-Design-Patterns-Object-Oriented-ebook/dp/B08P3X99QP))
