---
title: Composable Design Patterns – Basic Concepts
description: Code reuse and dynamic flexibility via composition
---

<img src="https://upload.wikimedia.org/wikipedia/commons/1/16/Four_bit_adder_with_carry_lookahead.svg" alt="Full Adder Logic Gates" title="Image Source: https://www.wikiwand.com/en/Logic_gate#Media/File:Four_bit_adder_with_carry_lookahead.svg" width = "45%" align="center" style="padding-right: 35px;">

# Introduction
This blog entry begins a new series of Design Patterns. I refer to these design patterns as Composable Design Patterns, which feature object composition. That is, behavior emerges from the interaction of a set objects organized in a cohesive pattern.

Consider logic gates as shown above. Binary addition emerges from the interaction among these logic gates. Repeating patterns can be grouped into their own logical components, such as Full and Half Adders. Logic gates can be organized in different combinations from which emerges all behaviors electronic computers depend upon.

# Code Reuse
Code reuse has been the Holy Grail of software since I started my career in the mid-1980s. I recall a kick-off meeting from that time for the second release of a project. One of the project managers was giving us a pep talk. We were considering an almost entire rewrite of the first release. The project manager challenged us. Why did we have to start from scratch? The project manager didn’t just want evolutionary code. He wanted revolutionary code! For context, our project was written in C, and we just didn’t know how to design for reuse at the time.

We, as an industry, have made significant improvements with reuse through frameworks and utility libraries. But this is generic reuse. It isn’t reuse of our business domain components. How can we reuse our business domain concepts?

## Inheritance
Object-Oriented (OO) techniques held the promise of reuse with inheritance. I think that code reuse through inheritance was one of the reasons that OO was widely adopted in the 1990s. However, inheritance wasn’t necessarily the panacea that we thought it might be. While inheritance did afford some code reuse, it came with a cost. Reuse was statically locked into place. The inheritance hierarchies could become unyieldingly large. It violated encapsulation. Changes in ancestor classes could break previously working behavior in the descendant classes. And when descendant classes overrode behavior of their ancestor classes, they ran the risk of violating the [Liskov Substitution Principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle).

These issues reside with implementation inheritance. Interface inheritance doesn’t have these issues.

## Object Composition
The Design Pattern Gang of Four (GoF) authors approached reuse via another approach, which is embedded in their [two design principles](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html): [Program to an interface, not an implementation](https://www.artima.com/articles/design-principles-from-design-patterns) and [Favor object composition over class inheritance](https://www.artima.com/articles/design-principles-from-design-patterns#part4).

The first principle was featured in the [Essential Design Patterns](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html). It is not ignored with the Composable Design Patterns either, since every Composable Design Pattern features an interface or abstract base class as the root element. But the Composable Design Patterns pick up where the Essential Design Patterns left off. Their main feature highlights the second design principle to favor object composition over inheritance.

I'm sure the GoF chose "favor" for a reason. Do not eschew inheritance but compare it against object composition. Favor object composition, but inheritance is still an option.

## E Pluribus Unum
An interface defines [contract](https://jhumelsine.github.io/2025/06/10/contracts.html) behavior. We often implement it with a class, which might delegate to other classes as well.

But we have another design approach that may be a bit more modular. Consider that we can implement the contract behavior using a group of cohesive objects instantiated from low-level classes where the desired behavior emerges from the interaction of these cohesive objects. No individual object is responsible for the entire behavior and yet all of them are responsible for it collectively.

The low-level elements can be reused in different combinations to define different emergent behaviors. Behavior emerges from the collective cohesive objects and not from the implementation within one individual class.

Emergent behavior distributed across multiple cohesive objects is probably the most challenging aspect of Composable Design Patterns. The challenge is not in the implementation itself. The challenge is in the comprehension of distributed behavior across multiple objects.

## Inheritance versus Composition
Inheritance and Composition both feature reuse, but they achieve it in different ways:
* Inheritance is vertical single static class reuse via language constructs configured at compile time.
* Composition is horizontal multiple dynamic object reuse via object delegation configured at runtime.

The use of vertical and horizontal visual comes from UML class diagrams. We tend to draw inheritance vertically can composition horizontally.

# Atoms and Molecules and Minecraft
<img src="https://cdn.stocksnap.io/img-thumbs/960w/dna-helix_CHEZDDXUJY.jpg" alt="DNA Molecules" title="Image Source: https://cdn.stocksnap.io/img-thumbs/960w/dna-helix_CHEZDDXUJY.jpg" width = "30%" align="right" style="padding-right: 35px;">

A real-world example of composability is chemistry. Atoms are the low-level elements from which molecules can be assembled to produce all kinds of chemical behaviors, including life. Our low-level objects are like elements on the Periodic Table. Each has a unique simple behavior. Then low-level objects can be assembled to create more complex behaviors from those simple behaviors. Just as many different complex molecules spring forth from the finite set of elements in the Periodic Table, many different complex behaviors can spring forth from the finite set of low-level objects.

Object composition is front-and-center in Minecraft with its redstone material as described on the [Minecraft Wikipedia](https://en.wikipedia.org/wiki/Minecraft) page:
> The game also contains a material known as redstone, which can be used to make primitive mechanical devices, electrical circuits, and logic gates, allowing for the construction of many complex systems.
> 
> With the introduction of redstone blocks to represent electrical circuits, users have been able to build functional virtual computers within Minecraft. Such virtual creations include a working hard drive, an 8-bit virtual computer, and even a smaller-scale version of Minecraft that is playable and able to be built completely in survival mode with no external modifications. In at least one instance, a mod has been created to use this feature to teach younger players how to program within a language set by a virtual computer within a Minecraft world.
> 
> Another computational mechanic in Minecraft is the command block, a block that is only accessible in creative mode and can alter game logic. It has been used to create emulators for the Atari 2600 (including one by YouTube personality SethBling) and the Game Boy Advance.

# Composable Design Patterns
The GoF did not define a set of Composable Design Patterns per-se. This is my grouping, just like the Essential Design Patterns is my grouping as well. I’m not even sure that this is the best name for these patterns. I’ve also called them the Reuse/Reuseable/Reusability Design Patterns.

Regardless of the name, here are the patterns, which I feel most highlight the concept of composability. They are listed in order of generally least complexity to most complexity. Several patterns expand upon concepts that first appear in previous ones as well. I will feature each of these in this blog series:
* [Proxy](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html) - Place administrative wrapper objects around objects often to help manage their complexity or resources.
* [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) -Layer additional behaviors upon core features.
* [Chain of Responsibility](https://jhumelsine.github.io/2024/02/20/chain-of-responsibility-design-pattern.html) - Delegate a request through a linked chain of handlers until one of the handlers can complete the request.
* [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) - Configure behavior emerging from a group of snippet behavior objects organized in a tree structure.
* [Specification](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html) – Allow a Client to select or filter objects with specific attribute property values as specified by the Client.
* [Interpreter](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) – Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language. This pattern will require several blog entries.

Additionally, there are some two additional [Creational Design Patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html), which I may include in this series as well, since they are useful for these patterns:
* [Builder](https://sourcemaking.com/design_patterns/builder) - Parse a complex representation, create one of several targets.
* [Prototype](https://sourcemaking.com/design_patterns/prototype) - Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype.

# Composition Structure
Object composition is more than just making a single method call to another object. With Composable Design Patterns, behavior aggregates across multiple objects organized within basic data structures, such as linked lists or trees. However, there's one slight data structure variation. Traditional data structures contain data. Composable Design Patterns data structures contain functional behaviors as we’ve seen with [Command](https://jhumelsine.github.io/2023/09/18/command-design-pattern.html)/[Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html). Composition isn’t about the classes. It’s about the composition of the objects instantiated from the classes. Different compositions of objects yield different behaviors.

This means that the same implementation of classes can yield different behavior based upon different compositions of objects instantiated from those classes. This is not different behavior via `if` and `switch` statements or feature flags. It’s configurable behavior that emerges from object composition rather than different execution paths through traditional code.

## Composition Basics
The Composable Design Patterns are an extension of several Essential Design Patterns such as [Command](https://jhumelsine.github.io/2023/09/18/command-design-pattern.html), [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html), [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) and a little bit of [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html). These patterns feature concrete classes, which implement an interface or extend an abstract class.

The Composable Design Patterns feature similar concrete classes as well as delegation, except that the delegation is self-referential. They delegate to elements in the design often their root interface or class.

Here’s a UML class diagram that highlights this self-referential delegation. Each of the specific Composable Design Patterns will be slightly different based upon their own contexts than this example, but they will contain similar structures:

<img src="/assets/ComposableDesignPatternsIntroduction.png" alt="Composable Design Patterns Template" width = "75%" align="center" style="padding-right: 35px;">
 
* `Composable` is self-referential because it both implements `Feature` and delegates to `Feature`. That is, it’s a `Feature` class that delegates to the `Feature` interface. This is not a circular dependency, even if it feels like it at first. Follow the dependency arrowheads connecting `Feature` and `Composable`. They both flow from `Composable` to `Feature`. This delegate reference is the only new feature. If it were removed, we’d have a UML class diagram for Strategy.
* The self-referential nature of `Composable` means that one instance of `Composable` can delegate to other instances of `Composable`. This allows the set of composed objects to be as small or as large as needed. This configuration flexibility is the power behind these patterns.
* The [`Configurer`](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) is critical to the Composable Design Patterns, and it’s never featured by the GoF in their design patterns. It’s the component that instantiates the objects, assembles their configuration, and injects the “root” instance into the `Client`. The `Configurer` manages the entire design. Without it, the classes in these patterns only have potential. The `Configurer` knows the context of each application, and it instantiates and assembles the low-level objects to satisfy that context.
* From the `Client`’s point of view, it has knowledge of a single reference to `Feature` upon which it calls `execute()`. It has no knowledge whatsoever of the design or configuration on the other side of `Feature`. The composition may consist of a single object or thousands of objects. This encapsulation supports the flexibility of these patterns, since the `Client` has no dependencies beyond the `Feature` contract.
  
Non-`Configurer` concrete classes only know about interfaces. New concrete classes can be added without affecting existing classes or existing configurations. All the classes can be easily [unit tested](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html).

The classes in these patterns tend to be stateless. Each can be viewed as a pure function. Therefore, their composition can be viewed as a composition of pure functions. This makes them ideal for concurrent implementations. Multiple threads can execute simultaneously within the set of objects. The composition might only need to be composed once and then used repeatedly by many threads without additional concern.

# Computation and Coordination
Composition has more flexibility than inheritance, but with that comes more runtime responsibility. The composition won't happen by default. Composable patterns are divided into two concepts:
1.	Computation via code reuse using delegation.
2.	Coordination of the objects that comprise that composition.

Computation resides in the class implementation. Composition resides with the `Configurer` as described above. Keep these two concepts separate.

Kevlin Henney pointed out the following in his GOTO 2018 presentation, [Old is the New New](https://www.youtube.com/watch?v=AbgsfeGvg3E&t=2830s):
> _We can build a complete programming model out of two separate pieces – the __computation model__ and the __coordination model__._
> 
> "[Coordination Languages and their Significance](https://dl.acm.org/doi/pdf/10.1145/129630.129635)",  David Gelernter and Nicholas Carriero, published in the Communications of the ACM in 1992.

This __computation/coordination model__ concept appears in many layers of abstraction in computing:
* Programming language designers provide a computational model of expressions and statements. Behaviors emerge from the coordination of concrete expressions and statements assembled by software developers. In other words, Java provides a finite set of computational components, such as classes, methods, variables, etc., and developers coordinate those components, via writing a program, from which behavior emerges as it executes on the JVM.
* Unix/Linux designers provide a computation model of small independent elementary operating system behaviors. Developers coordinate these features to create higher-level behaviors assembled via pipes or shell constructs.
* Microservices provide a computational model of doing one thing. Systems are built upon the coordination of interacting microservices.

The __computation/coordination model__ concept will continue throughout the Design Patterns. It's almost identical to the Design Pattern principles above:
* Computation Model - This is programming to an interface. This is keeping objects independent of their dependencies. This is raw and reusable potential.
* Coordination Model - This is object composition. This is instantiating concrete objects from the computational model and coordinating their assembly to produce the desired behaviors, which have the potential for customer customization.

Behavior emerges from the coordination/composition of objects. The same objects in different compositions will yield different behaviors. Composition management may reside on a spectrum from development organization composition to customer [support] organization composition to customer self-service composition to individual user self-service composition. As the composition management moves away from development and closer to the customer and user, the definition and responsibility of desired behavior moves further from development and closer to the customer and user.

While some compositions can be tested for sanity, it may not be possible to test all possible behaviors. And we may never know the behaviors composed by customers or users via self-service. See: [The Achilles Heel](https://jhumelsine.github.io/2025/06/23/test-layers.html#the-achilles-heel-when-test-layers-arent-enough)

# Use Cases for Composability Design Patterns
These patterns are marvelous. A relatively small implementation provides so much functional potential. These patterns not only provide the potential means of code reuse for new features, but they also provide the possibility of customized features for customers and users as well.

Customized rule or policy-based behaviors are possible. With care, a single code base can support different policy requirements for different customers or even users. Policy behavior can be supported independently for different users and be changed without having to change the implementation. This is not the same as config values, feature flags or branching logic in the implementation. Policy based behaviors tend to reside in specifications, which are rendered as different configuration assemblies from a finite set of implementation objects.

In some cases, the configuration is completely within control of the project and never exposed externally. In other cases, the configuration options might be presented to the customer or user for their own self-service configurations.

## Insurance Industry
The insurance industry can benefit from this flexibility with their insurance policies, as in auto, home, boat, etc. Insurance companies offer many types of insurance, many with the potential for additional riders. Making matters even more complicated, the insurance industry is highly regulated with government policy regulations varying among countries, states, provinces, etc.

It's possible for each insurance policy holder to have a unique policy combination unlike any other insurance policy holder in the system. It's impossible to program all possible insurance policy scenarios with traditional programming. But the compositional flexibility of design patterns could allow a single code base to support any number of potential insurance policies. They could support individualized billing, policy statements and other insurance elements.

Gary Hoberman, CEO and Founder of Unqork, spoke about this in the [Thoughtworks Pragmatism in Practice Podcast](https://www.thoughtworks.com/insights/podcasts/pragmatism-in-practice) episode: [No-code platforms and the art of the possible](https://www.thoughtworks.com/insights/podcasts/pragmatism-in-practice/no-code-platforms). He mentioned this aspect specifically in how his company addressed the insurance industry starting at around minute 25:00 into the podcast and lasting for about 4 minutes. While Hoberman doesn't discuss specific design concerns, what he does describe sounds 100% consistent with composable design patterns.

## User Data Rights Regulations
User data rights are highly rule/policy based. User data rights are subject to external forces, such as government regulations. They are prone to be segregated and segmented, for example the rights in [GDPR](https://en.wikipedia.org/wiki/General_Data_Protection_Regulation), California and other jurisdictions are similar, but there are still subtle differences. They are prone to change with tight inflexible deadlines for conformance with nonconformance resulting in lost revenue or fines. Being regulatory, they tend to be vague, ambiguous and/or incomplete, so you may think you have it right, only to learn in the 11th hour that something is nonconformant and you need to make a quick update. Configuration updates are faster than implementation updates.

A rule/policy-based implementation for user data rights would consist of individual specific low-level user data rights behaviors that are configured into composable behaviors that manage each user's specific rights. As policies change, the specification for the composition would change, which may not require implementation updates. If regulations require new low-level user data rights classes, then they can be added without touching or affecting the existing low-level classes or their object composites until new composite specifications are defined that include those new low-level classes.

## Self-Service Apps and Kiosks
<img src="https://live.staticflickr.com/215/457310711_01c0977e79_b.jpg" alt="Fast-Food Kiosk" title="Image Source: https://www.flickr.com/photos/slworking/457310711" width = "35%" align="left" style="padding-right: 35px;">
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/13/Dagwood_Sandwich.svg/489px-Dagwood_Sandwich.svg.png?20200322074015" alt="Dagwood Sandwich" title="Image Source: https://commons.wikimedia.org/wiki/File:Dagwood_Sandwich.svg" width = "20%" align="right" style="padding-right: 35px;">

Many fast-food establishments allow you to customize your order via an app or kiosk. These often provide options well beyond __A__, __B__ or __C__ static menu options. For example, options for sandwich establishments often allow the customer to design their own sandwich at the app/kiosk. The app/kiosk might start with the concept of a sandwich, but then the customer can choose bread, meat, cheese, condiments, etc. Or for simplicity, a sandwich configuration might be predefined, and the customer only needs to choose optional condiments.

# Summary
This has laid the foundation for the Composable Design Patterns. Subsequent blogs will provide more concrete details with context as we explore them. Also see: [Composable Design Patterns AI Notebook](https://notebooklm.google.com/notebook/bc19b872-144e-4218-8622-023e963becdf?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw).
