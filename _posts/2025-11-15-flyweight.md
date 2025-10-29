---
title: WORK IN PROGRESS – Flyweight/Multiton Design Pattern
description: TBD
unlisted: true
---

<img src="https://live.staticflickr.com/7295/14000278151_141357835b_b.jpg" alt="Flyweight Boxers" title="Image Source: https://www.flickr.com/photos/worldseriesboxing/14000278151/" width = "75%" style="padding-right: 20px;">

# Introduction
The Gang of Four (GoF) categorize __Flyweight__ as a [Structural Pattern](http://refactoring.guru/design-patterns/structural-patterns), whereas I view it as a [Creational Pattern](https://refactoring.guru/design-patterns/creational-patterns), which is why I've included it in my [Creational Pattern Series](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html).

Flyweight allows the client to create multiple instances of a Singleton class. I know this sounds like an oxymoron, but hang in there for a bit. Singleton only allows one instance for a class. Flyweight allow multiple attribute specified instances of a class, but with the constraint that when there can only be one instance for each set of unique attributes.

Though not included by the GoF, there is a creational pattern for this behavior: __Multiton__, which is a portmanteau of _Multiple_ and _Singleton_. Multiton describes __what__ it does. Flyweight describes __how__ it does it. I view them as two patterns for the same concept approached from two different directions: __What__ vs __How__. As I alluded to in my opening sentence, I think the GoF should have focused upon its creational aspects and possibly Multiton would have been a better name. But since Flyweight is the name they chose, I'll stick with it for the rest of this blog.

I view Flyweight is an extension of the [Singleton Design Pattern](https://jhumelsine.github.io/2025/10/31/singleton.html), or maybe it might be more accurate to view Sington as a special case of Flyweight.

Regardless of its categorization, I've not much liked the name Flyweight either. I think it's based upon [weight class](https://en.wikipedia.org/wiki/Weight_class_(boxing)) in boxing, where [flyweight](https://en.wikipedia.org/wiki/Flyweight) is the lightest weight class. The pattern focuses upon keeping memory acquisition lean and clean.

# Summary
TBD

# References
* [Wikipedia Flyweight Design Pattern](https://en.wikipedia.org/wiki/Flyweight_pattern)
* [Source Making Flyweight Design Pattern](https://sourcemaking.com/design_patterns/flyweight) - I'm including this resource, since I always include this resource; however, their diagram doesn't feel right to me.
* [Refactoring Guru Flyweight Design Pattern](https://refactoring.guru/design-patterns/flyweight)
* [Flyweight Pattern in Java](https://www.baeldung.com/java-flyweight) - By Baeldung
* [Flyweight Design Pattern](https://www.patterns.dev/vanilla/flyweight-pattern/) - by Patterns.dev
* [Flyweight Design Pattern in Java](https://www.digitalocean.com/community/tutorials/flyweight-design-pattern-java) - by DigitalOcean
* [Understanding the Flyweight Design Pattern](https://www.carloscaballero.io/understanding-the-flyweight-design-pattern/) - Blog by Carlos Caballero
* [Flyweight Design Pattern and Immutability: A Perfect Match](https://www.sitepoint.com/flyweight-design-pattern-immutability-perfect-match/) - Blog by Andrew Carter
* [Flyweight](https://www.cs.unc.edu/~stotts/GOF/hires/pat4ffso.htm) - University of North Carolina Computer Science Department. Looks like a direct copy from the GoF book.
* [Flyweight Design Pattern](https://www.youtube.com/watch?v=0vV-R2926ss) - Video by Derek Banas
* [Flyweight Pattern - DESIGN PATTERNS (C#/.NET)](https://www.youtube.com/watch?v=RDV0ioVTF4g) - Video by SingletonSean
* [Wikipedia Multiton Design Pattern](https://en.wikipedia.org/wiki/Multiton_pattern)
* [Design Pattern Series: Singleton and Multiton Pattern](https://www.codeproject.com/articles/Singleton-and-Multiton-Pattern#comments-section) - Blog by CodeProject
* [Multiton Pattern in Java: Mastering Advanced Singleton Variants](https://java-design-patterns.com/patterns/multiton/) - Blog by Java Pattern Designs
* and for more, Google: [Flyweight Design Pattern](https://www.google.com/search?q=flyweight+design+pattern) and [Multiton Design Pattern](https://www.google.com/search?q=multiton+design+pattern)
