---
title: WORK IN PROGRESS – Prototype Design Pattern
description: TBD
unlisted: true
---

# Introduction
I introduced the Prototype Design Pattern (TBD) in my previous blog entry. Prototype is a [Creational Design Pattern] (https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html) that's different from the other creational design patterns. Most creational patterns involve a static method invoking the constructor of the concrete class type their instantiating. This means that they have knowledge of and depend upon that concrete class type. Should there be any class type updates, then the creational pattern will need to be updated.

Prototype instantiates a new instance by invoking a method of an object of the desired type. This object method is responsible for returning an instance of its own class. This object method vs static method invokation is what separates Prototype from it fellow creational patterns.

This blog entry continues with Prototype with a use case featuring elements of a Drawing program, much like what PowerPoint provides.

# Drawing Program Use Case
<img src="/assets/Prototype5.png" alt="PowerPoint Slides"  width = "30%" align="right" style="padding-right: 35px;">

I render all of my UML class diagrams in PowerPoint. I mentioned this in a previous blog entry where I presented [My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html). PowerPoint has its advantages and disadvantages, but I found that it has been sufficient for my needs. PowerPoint's Drawing feature allows me to create shapes, connect them, color them, add/edit text, etc.

This Prototype Use Case will sketchout a design and implementation for some of the features of a drawing tool. It will include:
* A skeleton design and implementation for a drawing tool.
* Using a __Prototype__ and __Prototype Repository__ to acquire Shapes by name. This allows new Shape types to be added without having to update the Prototype portion of the design.
* The ability to group Shapes together, such as one can do with __Group__ in PowerPoint.
* The ability make copies of any Shapes such as one can do with Copy and Paste in PowerPoint.

In addition to __Prototype/Prototype-Repository__ this design will also feature elements of [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html), [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html), [Factory Method](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) and [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html).

## Shape
The contract is declared in Shape.

<img src="/assets/Prototype6.png" alt="PowerPoint Slides"  width = "30%" align="center" style="padding-right: 35px;">

I chose an abstract class rather than an interface, because I want to store some information in `Shape` along with some implementation. It's easier to do that when it's an abstract class rather than an interface..

`Shape` only does two things in this design:
* It acquires a copy of itself
* It renders itself

In a real drawing tool, there would be more behavior and state, such as:
* Position
* Rotation
* Line and Fill formatting
* Optional Text with its own formatting
* **PICK IT UP HERE**

```java
abstract class Shape {
    // Each newly created object will have a unique serialNumber initialized from an incrementing serialCounter.
    // It exists only to demonstrate that new objects are being created when acquired.
    private static int serialCounter = 0;
    private final int serialNumber;

    // Each object has a stae, which will be copied from the breeder when a new object is acquired.
    // It exists to demonstrate that new objets are created via a deep copy.
    // It's a representational placeholder for all possible Shape state information.
    private final String state;

    protected Shape(String state) {
        this.serialNumber = serialCounter++;
        this.state = state;
        System.out.format("Creating Shape serialNumber=%d, state=%s\n", serialNumber, state);

    }

    public final Shape acquire() {
        return acquire(state);
    }

    protected abstract Shape acquire(String state);

    // This example isn't rendering shapes. It demonstrates where they could be rendered.
    // This version of render() is closer to toString().
    public final void render() {
        render(0);
    }

    protected abstract void render(int indentation);

    protected final String getIndentation(int indentation) {
        return " ".repeat(indentation);
    }

    protected final String getShapeDetails() {
        return String.format("serialNumber=%d, state=%s", serialNumber, state);
    }
}
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
