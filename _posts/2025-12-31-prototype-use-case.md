---
title: WORK IN PROGRESS – Prototype Design Pattern Use Case
description: Demonstrating a Prototype Design in the Context of a Drawing Program
unlisted: true
---

# Introduction
I introduced the [Prototype Design Pattern](https://jhumelsine.github.io/2025/12/23/prototype.html) in my previous blog entry. Prototype is a [Creational Design Pattern](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html) that's different from the other creational design patterns. Most creational patterns involve a static method invoking the constructor of the concrete class type they're instantiating. This means that they have knowledge of and depend upon that concrete class type. Should there be any class type updates to the design, then the creational pattern implementation will need to be updated.

Prototype instantiates a new instance by invoking a method of an object of the desired type. This object method is responsible for returning an instance of its own class. This object method vs static method invokation is what allows the implementation to be more flexible to class type updates and is what separates Prototype from it fellow creational patterns.

This blog entry continues with Prototype with a Drawing Program Use Case example, much like what PowerPoint provides to render shapes.

# Drawing Program Use Case
<img src="/assets/Prototype5.png" alt="PowerPoint Slides"  width = "50%" align="right" style="padding-right: 35px;">

I render all of my UML class diagrams in PowerPoint. I mentioned this in a previous blog entry where I presented [My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html). PowerPoint has its advantages and disadvantages as a design tool, but I found that it has been sufficient for my needs. PowerPoint's Drawing feature allows me to create shapes, connect them, color them, add/edit text, etc. Almost every UML class diagram in my blog entries has been rendered using PowerPoint.

This Prototype Use Case will sketchout a design and implementation for some of the features of a drawing tool. It will include:
* A skeleton design and implementation for a drawing tool illustrating the basic shape organization structure.
* Using a __Prototype__ and __Prototype Registry__ to acquire `Shape` objects by name. This allows new concrete `Shape` types to be added without having to update the Prototype portion of the design.
* The ability to group `Shapes` together, such as one can do with the __Group__ feature in PowerPoint.
* The ability make copies of any `Shapes` such as one can do with _Copy-and-Paste_ in PowerPoint.

In addition to __Prototype/Prototype-Registry__, this design will also feature elements of [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html), [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html), [Factory Method](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) and [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html).

I will present the design starting with a `Shape` contract and then expand the design into extending classes.

I'm also fond of declaring attributes and methods as `final` when possible. Since the design features Template Method, I want to ensure that extending classes don't violate Template Method behaviors by overriding them, even if unintentionally.

## Shape
The contract is declared in Shape.

<img src="/assets/Prototype6.png" alt="Shape UML"  width = "40%" align="center" style="padding-right: 35px;">

I chose an abstract class rather than an interface, because I want to store information in `Shape` along with some implementation. It's easier to do that when it's an abstract class rather than an interface.

`Shape` only does two things in this design:
* It acquires a copy of itself
* It renders itself

In a real drawing tool, there would be more behavior and state, such as:
* Position
* Rotation
* Line and Fill formatting
* Optional Text with its own formatting
* Etc.

The `Shape` implementation contains some state:
* `serialNumber` which is an incrementing integer for each newly acquired `Shape` object. I've added it to demonstrate that new objects are being instantiated as they are acquired.
* `state` which is a placeholder for state within the object. In a production drawing program, state would include position, rotation, formatting details, etc. In my simple demo, it's a simple String.

This demo won't render any images. Text will be a substitute for rendering if this were an actual drawing program. Rendering in this demo is closer to a sophisticated `toString()` feature, which leverages the [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html).

```java
TBD
```

## Registered Breeder
The design expands to include `RegisteredBreeder`. It is a Prototye Registry. I contemplated naming it `RegisteredShapeBreeder`, but this felt too long. Maybe `RegisteredShape` would be a better name. Naming things is hard.

It throws an exception if a breeder is not found, and it also throws an exception if a newly `RegisteredBreeder` use the same key of an existing `RegisteredBreeder`.

<img src="/assets/Prototype7.png" alt="Registered Breeder UML"  width = "40%" align="center" style="padding-right: 35px;">

```java
TBD
```

# Summary
TBD

# References
TBD

# Complete Demo Code
Here’s the entire implementation up to this point as one file. Copy and paste it into a Java environment and execute it. If you don’t have Java, try this [Online Java Environment](https://www.programiz.com/java-programming/online-compiler/). Play with the implementation. Copy and paste the code into Generative AI for analysis and comments.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# NOTES
* Copy entire composite structures.
* Closing thoughts. Creational Patterns are not mutually exclusive. A design may incorporate several of them. For example, in Composite trees, the non-terminal composite classes might be acquired via a Factory while the terminal leaf nodes could be acquired via a Singleton, if they are stateless. And Prototype could be used to make a copy of the entire composite structure as seen in the use case.
