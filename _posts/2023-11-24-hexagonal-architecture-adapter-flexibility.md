---
title: Hexagonal Architecture – Adapter Flexibility
description: How a few design options in the Adapter Layer provides more flexibility
unlisted: true
---

# Introduction
In the [Why It Works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html) blog, I featured some knowledge/dependency theory and why I feel that the practice of that theory is one of the main reasons that Hexagonal Architecture works so well.

This blog entry will expand upon those ideas with some additional design options that may be useful in future projects.

These additional design options are mostly my ideas. I'm branching away from Alistair Cockburn and Bob Martin, who popularized this architecture/design to start with. Their presentations tend to be more limited. That may be because their presentations are focused upon _architecture_ whereas I feel this is really a _design_. Architecture can be a subset of design, but I think that good design can be applied at any layer of abstraction.

Alistair Cockburn posted this [declaration](https://twitter.com/TotherAlistair/status/1704531058023141490) on X/Twitter in September, 2023:
> For those who keep asking about #hexagonalarchitecture layers, here it is: 
There are only 2 layers: inside. outside. No relation to layers in Clean, Onion, DDD, Modular Monolith, nothing. Stop dragging them into the pic.
What you do inside each layer is all your biz, not mine.

I sort of understand what he's trying to say here. I interpret his statement as saying that Hexagonal Architecture (Ports and Adapters) is only about the Port and the Adapter that plugs into the port. The Port is inside. The Adapter is outside.

But I think he's throwing away so much richness of the design with this constrained definition. He views this design as basically my red hexagon. I add a few more parts to the design, including the purple hexagon. I also feel that so much of this design applies to 
_Clean, Onion, DDD, Modular Monolith_ and more. I plan to blog more about the relationship of Hexagonal Architecture to these concepts, I've already blogged about Clean and Onion, do some degree, in my [Hexagonal/Clean Compare and Contrast blog](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html). However, after five sequential blog entries about Hexagonal Architecture, I'm going to take a break for a bit and return to other topics for a bit. I'll eventually circle back to Hexagonal Architecture.

Cockburn doesn't say that you can't do more. He just says that he doesn't care what you do.

Bob Martin adds a few more layers to Clean Architecture than Cockburn does in Hexagonal Architecture. Martin indicates dependency from the outer most **Frameworks and Drivers** ring pointing inward toward the **Interface Adapters** ring. I think this is the wrong direction as I mentioned in the [Frameworks and Drivers](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html#frameworks-and-drivers) section of my comparision blog. I represent this boundary as a purple hexagon, and in my diagrams the knowledge and dependency arrows all point outward.

# Refresher
This blog is a continuation of [Why It Works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html), but I'll include a refresher here.

Here's my basic Hexagonal Architecture diagram:

<img src="/assets/HexArchHexagons.png" alt="Hexagonal Architecture" width = "75%" align="center" style="padding-right: 35px;">

The color coded rectangles are interfaces or classes:
* Green - Interfaces
* Blue - Class implementations
* Purple - Class Implementations that create and configure the Blue classes
* Red - External interfaces or class APIs that the design depends upon

Relationships between them are represented by lines:
* Dashed lines with triangles represent _implements_
* Dashed lines with small arrows represents _creates_
* Solid lines with triangles represent _extends_
* Solid lines with small arrows represents _references_ or _delegates_

The arrows indicate dependency, but I think my importantly they indicate the direction of knowledge. When A points to B, then A depends upon and knows of B, but B doesn't depend upon or know of A.

Some elements only have arrows pointing into them. I refer to them as [Pure Stable/Fixed](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#pure-stablefixed-elements). This includes:
* The Port/Interface/Contract
* The Red Hexagon Boundary

Some elements only have arrows pointing out of them. I refer to them as [Pure Unstable/Flexible](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#pure-unstableflexible-elements). This includes:
* The Configurer
* The Business Logic
* The Adapters
* The Purple Hexagon Boundary

Technically the Configurer the Purple Hexagon Boundary are the only Pure Unstable/Flexible elements, but the Business Logic and Adapters only have a creation arrow pointing into them, which comes from the Configurer. I consider that pure enough for what I'm going to present.

All of the design options will be with these Pure Unstable/Flexible elements, but the Hexagonal Architecture design provides a zone within which we can be very flexible without affecting the rest of the design. All the fun is in the Adapter zone. Red Hexagons are mosly the same in all of the diagrams. They don't change as we _flex_ the Adapters. Likewise, the red external frameworks and depenencies don't really change either.

# Deep and Wide
...

# Example
...

# Nested Hexagons
...

# Summary
You may not need this level of flexibility. ...

# References
See previous blog [References](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html#references).

# RAW NOTES

Cockburn's Tweet and references to his photo images too. He references just 2 layers inside and out. His design basically only has a single red hexagon. He's literal and a bit to restrictive. He views it as an architecure. I view it as a pattern. I feel that the architecture view is a subset of the patterns. Don't restrict yourself.
https://twitter.com/TotherAlistair/status/1704531058023141490
https://twitter.com/TotherAlistair/status/1726618384614535624
https://twitter.com/TotherAlistair/status/1726618384614535624

Make note of how Hexagonal Architecture supports this and which elements tend to be which.

Breadth is a concept in Cockburn's Facets.

Breadth and Depth together provide a lot of flexible design options.

Nested Hexagons. Pruple Hexagons are Pure Unstable/Flexible. We end up with a recursive structure so all elements of the overall pattern can be applied a level deeper and as deep as necessary.

Most HexArch presentstaions don't give a sense of scope. Is it the entire project? Is it a feature. Hex Arch seems to indicate an architecture. Cockburn views Ports as external ports. But I don't think he really cares all that much. Clean Arch tends to be at the User Story level as Bob Martin presents it. I think that Bounded Context is a good boundary. This uses a defintion from DDD for context, but still gives the designer the freedom to adjust as needed.

List who works on which parts of the design and when. Seniors, Juniors, Architects, etc. Get Business Analysists and Project/Product managers involved too. They should be able to read the business logic code. Even if they can't read code, they should be able to follow the gist of it. If not, then the implementation needs work. It's either too complex, has too much jargon or the ubiquituous language has not been used. Should be able to practice CI/CD too. Nothing "activates" until the Configurer creates the elements and assembles them into the design. Classes only depend upon interfaces so the design is modular and more easily maintained. Unit testing should be relatively simple to set up. The only classes that know other class types are the Configurers.

Deep and wide, deep and wide there a ... flowing deep and wide.

Polymer chains

Dispatcher. Tell the Document story. Update the names in the diagram if possible.

I is not for "Interface." I is for "I". Cockburn's "ForDoingSomething".
