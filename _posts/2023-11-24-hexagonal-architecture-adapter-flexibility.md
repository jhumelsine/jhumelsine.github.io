---
title: Hexagonal Architecture – Adapter Flexibility
description: How a few design options in the Adapter Layer provides more flexibility
unlisted: true
---

# Introduction
In the [Why It Works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html) blog, I featured knowledge/dependency theory and why I feel that the practice of that theory is one of the main reasons that Hexagonal Architecture works so well.
This blog entry will expand upon those ideas with some additional design options that may be useful in designs that feature Hexagonal Architecture principles.

These additional design options are not new, but they are my idea to present them within the context of Hexagonal Architecture.
I'm branching away from Alistair Cockburn and Bob Martin, who created and popularized this architecture/design.
Their presentations tend to be more bounded.
Their presentations are focused upon _architecture_ whereas I feel this is really a _design_.
Architecture can be a subset of design, but I think that good design can be applied at any layer of abstraction.

Alistair Cockburn posted [this](https://twitter.com/TotherAlistair/status/1704531058023141490) on X/Twitter in September, 2023:
> For those who keep asking about #hexagonalarchitecture layers, here it is: 
There are only 2 layers: inside. outside. No relation to layers in Clean, Onion, DDD, Modular Monolith, nothing. Stop dragging them into the pic.
What you do inside each layer is all your biz, not mine.

I sort of understand what he's trying to say here. I interpret his statement as saying that Hexagonal Architecture (Ports and Adapters) is only about the Port and the Adapter. The Port is inside. The Adapter is outside.

But I think he's throwing away so much richness of the design with this constrained definition.
He views this design as restricted to the elements I represent on either side of the red hexagon.
I include a few more elements to the basic design, such as the purple hexagon.

I also feel that so much of this design applies to _Clean, Onion, DDD, Modular Monolith_ and more. 
I plan to blog about the relationship of Hexagonal Architecture to these concepts. I've already blogged about Clean and Onion, to some degree, in [Hexagonal/Clean Compare and Contrast blog](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html).

<img src="https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg" alt="The Clean Architecture" title="Image Source: https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html" width = "40%" align="right" style="padding-right: 35px;">

Bob Martin adds a few more layers to Clean Architecture than Cockburn does in Hexagonal Architecture. Martin indicates dependency from the outer most **Frameworks and Drivers** ring pointing inward toward the **Interface Adapters** ring. I think this is the wrong direction as I mentioned in the [Frameworks and Drivers](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html#frameworks-and-drivers) section of the comparision blog. I represent this boundary as a purple hexagon, and in my diagrams the knowledge and dependency arrows all point outward. That change will become important later.

# Refresher
This blog is a continuation of [Why It Works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html), but I'll include a refresher here.
Here's my basic Hexagonal Architecture diagram:

<img src="/assets/HexArchHexagons.png" alt="Hexagonal Architecture" width = "85%" align="center" style="padding-right: 35px;">

[Pure Stable/Fixed](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#pure-stablefixed-elements) have arrows pointing into them, meaning they depend nothing. Such elements in Hexagonal Architecture include:
* The Port/Interface/Contract
* The Red Hexagon Boundary
* The External Frameworks and Dependencies

[Pure Unstable/Flexible](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#pure-unstableflexible-elements) have arrows pointing away from them, meaning nothing else in the design depends upon them. They are invisible to the other elements in the design. Such elements in Hexagonal Architecture include:
* The Configurer
* The Business Logic
* The Adapters
* The Purple Hexagon Boundary

Technically the Configurer and the Purple Hexagon Boundary are the only Pure Unstable/Flexible elements, but the Business Logic and Adapters only have a creation arrow pointing into them, which originate from the Configurer. I consider that pure enough for my needs.

Almost all design options presented here will be with these Pure Unstable/Flexible elements. All the fun is in the Adapter zone. Red Hexagons are mosly unchanged of the diagrams. They don't change as we _flex_ the Adapters. Likewise, the red external frameworks and depenencies don't  change either.

# I is for Interface, or is it?
Interface names start with _I_ in C# as convention. It's reminiscent of [Hungarian Notation](https://en.wikipedia.org/wiki/Hungarian_notation), which I've not much cared for myself. Hungarian Notation always reminded me of Klingon.

Here's what Bob Martin has to say about this practice in his book: **Clean Code**:
> These are sometimes a special case for encodings. For example, say you are building an ABSTRACT FACTORY for the creation of shapes. This factory will be an interface and will be implemented by a concrete class. What should you name them? IShapeFactory and ShapeFactory? I prefer to leave interfaces unadorned. The preceding I, so common in today’s legacy wads, is a distraction at best and too much information at worst. I don’t want my users knowing that I’m handing them an interface. I just want them to know that it’s a ShapeFactory. So if I must encode either the interface or the implementation, I choose the implementation. Calling it ShapeFactoryImp, or even the hideous CShapeFactory, is preferable to encoding the interface.

I tend to agree with him on this.
I avoided the **I** prefix for interfaces in my career.
Then a few months ago, I stumbed upon this blog: [I, Interface](https://talesfrom.dev/blog/i-interface).
The author suggested thinking of **I** as _First Person Singular: I_ rathern than _Interface_.
This leads to interface names that still have the **I** prefix, but in a way that they make so much more sense contextually.
For example, we can define interfaces with names like:
* `IPlaceOrders`
* `IUpdateOrders`
* `ICancelOrders`
* `IPersistPlacedOrders`, which can be implemented by a class with a name like `PersistPlacedOrdersViaMongoDB`.

I'll be using this convention through most of my examples. Alistair Cockburn does something similar, but slightly different. His naming convention starts with `For` as in:
* `ForPlacingOrders`
* `ForUpdatingOrders`

Both provide context, but I rather like the **I** prefix convention.

# Deep and wide. Deep and wide. There's a fountain flowing deep and wide
Previous Hexagonal Architecture diagrams I've presented have mostly only included one Framework Adapter and one Dependency Port/Adapter mainly to present the concepts as simply as possible. 
The Hexagonal Architecture deisgn is not restricted to one of each.

We have design flexibility in behavior and structure and then within each we have design flexibility with breadth and depth.

## Behavior and Breadth
The behavior can expand its breadth as needed. The Hexagonal Architecture can support multiple frameworks and dependencies. But it does so, because the behavior requires it. Consider this diagram where the Business Logic needs to persist stuff, publish events and send email. This should be obvious in the Business Logic implementaiton.

<img src="/assets/HexArchBehaviorBreadth.png" alt="Hexagonal Architecture with Multiple Adapters" width = "85%" align="center" style="padding-right: 35px;">

The Business Logic has three dependencies based upon its domain:
* `IPersistStuff` implemented by `PersistStuffViaDB`, which delegates to a `DB`.
* `IPublishEvents` implemented by `PublishEventsViaKafka`, which delegates to `Kafka`.
* `ISendEmail` implemented by `SendEmailViaSparkPost`, which delegates to `SparkPost`.

Even without any code, this hopefully makes sense.

There are two frameworks:
* `AndroidFramework` extended by `ManageStuffFromAndroid`, which translates Android based requests and delegates them to `IManageStuff`.
* `RESTFramework` extended by `ManageStuffFromRest`, which translates RESTful based requeests and delegates them to `IManageStuff`.

`ManageStuff` has no dependency or knowledge upon either of these Frameworks.

I didn't have room for the `Configurer`, but if I had drawn it, it would have only been a purple rectangle with creation lines to each of the blue rectangles. Here's what the Configurer class might look like in Java:
```java
IManageStuff manageStuff =
    new ManageStuff(
        new PersisStuffViaDB(),
        new PublishStuffViaKafka(),
        new SendEmailViaSpartPost()
    );

ManageStuffFromAndroid manageStuffFromAndroid = new ManageStuffFromAndroid(manageStuff);
ManageStuffFromRest manageStuffFromRest = new ManageStuffFromRest(manageStuff);
```
This probably isn't exactly the way it would play out. Most likely, the Android based configuration would have dependencies that reside in the Android environment, rather than a generic DB, Kafka or SparkPost.

This is why Alistair Cockburn chose a hexagon in his initial diagrams. When he draws this by hand, each framework/dependency port/adapter pair tends to have its own facet, which is a side of the hexagon. Here's an example:

<img src="https://blog.allegro.tech/img/articles/2020-05-21-hexagonal-architecture-by-example/ha_example.png" alt="Hexagonal Architecture with Facets" width = "45%" align="center" title="Image Source: https://blog.allegro.tech/2020/05/hexagonal-architecture-by-example.html" style="padding-right: 35px;">

## Behavior and Depth
Many Hexagonal Architectures show external dependencies as being outside of the system. I view external dependencies as anything that resides outside of the red hexagon regardless of its location. It could easily be another service in the system. Consider this diagram where the primary business logic to Place Orders delegates to an Inventory Service.

<img src="/assets/HexArchBehaviorDepth.png" alt="Hexagonal Architecture delegating to another service" width = "85%" align="center" style="padding-right: 35px;">

This diagram features two red hexagons for:
* `IPlaceOrders`
* `IManageInventory`

They do not know about each other. The glue that connects them is `UpdatePlacedOrdersViaManageInventory`, but it's really not the glue. It's the `Configurer` that glues all of the pieces together. Here's how it might look in Java:
```java
PlaceOrdersFromREST placeOrdersFromREST =
    new PlaceOrdersFromRest(
        new (PlaceOrders(
            new (UpdatePlaceOrdersViaManageInventory(
                new (ManageInventory(
                    new (PersistInventoryViaDB())
                )
            )
        )
    );
```
This chain can go on indefinitely. Let's remove the hexagons and the Configurer to see what's left

<img src="/assets/HexArchBehaviorDepthChain.png" alt="Hexagonal Architecture delegation chain" width = "85%" align="center" style="padding-right: 35px;">

<img src="https://upload.wikimedia.org/wikipedia/commons/9/94/Structure_of_monomers_and_polymers.jpg?20180217054949" alt="Polymer chain" width = "25%" align="right" title="Image Source: https://commons.wikimedia.org/wiki/File:Structure_of_monomers_and_polymers.jpg" style="padding-right: 35px;">

This pattern sequence reminded me of a descending stair case. It's the same repeating patterns of [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) and [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html). It can continue indefinitely, much like polymer chains.

## Structure and Breadth
...

## Structure and Depth
...

## Breadth and Depth
...

# Document Example
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

Polymer chains

Dispatcher. Tell the Document story. Update the names in the diagram if possible.
