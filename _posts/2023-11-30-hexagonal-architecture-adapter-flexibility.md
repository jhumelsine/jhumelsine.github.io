---
title: Hexagonal Architecture – Adapter Flexibility
description: How a few design options in the Adapter Layer provides more flexibility
unlisted: true
---

# References
See previous blog [References](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html#references).

# NOTES

Cockburn's Tweet and references to his photo images too. He references just 2 layers inside and out. His design basically only has a single red hexagon. He's literal and a bit to restrictive. He views it as an architecure. I view it as a pattern. I feel that the architecture view is a subset of the patterns. Don't restrict yourself.
https://twitter.com/TotherAlistair/status/1704531058023141490
https://twitter.com/TotherAlistair/status/1726618384614535624
https://twitter.com/TotherAlistair/status/1726618384614535624

Feature Pure Stable/Fixed and Pure Unstable/Flexible knowledge/dependency patterns. Reference previous blogs.

Make note of how Hexagonal Architecture supports this and which elements tend to be which.

Make note that some Unstable/Flexible elements aren't technically pure, but that's only because of their inbound creation arrows from pure Configurers.

Breadth is a concept in Cockburn's Facets.

Breadth and Depth together provide a lot of flexible design options.

Nested Hexagons. Pruple Hexagons are Pure Unstable/Flexible. We end up with a recursive structure so all elements of the overall pattern can be applied a level deeper and as deep as necessary.

Most HexArch presentstaions don't give a sense of scope. Is it the entire project? Is it a feature. Hex Arch seems to indicate an architecture. Cockburn views Ports as external ports. But I don't think he really cares all that much. Clean Arch tends to be at the User Story level as Bob Martin presents it. I think that Bounded Context is a good boundary. This uses a defintion from DDD for context, but still gives the designer the freedom to adjust as needed.

List who works on which parts of the design and when. Seniors, Juniors, Architects, etc. Get Business Analysists and Project/Product managers involved too. They should be able to read the business logic code. Even if they can't read code, they should be able to follow the gist of it. If not, then the implementation needs work. It's either too complex, has too much jargon or the ubiquituous language has not been used. Should be able to practice CI/CD too. Nothing "activates" until the Configurer creates the elements and assembles them into the design. Classes only depend upon interfaces so the design is modular and more easily maintained. Unit testing should be relatively simple to set up. The only classes that know other class types are the Configurers.

Deep and wide, deep and wide there a ... flowing deep and wide.

Polymer chains

Dispatcher. Tell the Document story. Update the names in the diagram if possible.

In all diagrams, all arrowheads cross the red hexagons inward and the purple hexes outward with the exception for creation arrows.

All the fun is in the Adapter zone. Red Hexagons are mosly the same in all of the diagrams. They don't change as we "flex" the Adapters. Likewise, the red external frameworks and depenencies don't really change either.

I is not for "Interface." I is for "I". Cockburn's "ForDoingSomething".
