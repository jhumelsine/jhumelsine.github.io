---
title: Hexagonal Architecture – Adapter Flexibility
description: How a few design options in the Adapter Layer provides more flexibility
unlisted: true
---

# Introduction
In the [Why It Works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html) blog, I featured knowledge/dependency theory and why I feel that the practice of that theory is one of the main reasons that Hexagonal Architecture works so well.
This blog entry will expand upon those ideas with some additional design options that may be useful in designs that feature Hexagonal Architecture principles.

These additional design options are not my original techniques, but they are my idea to present them within the context of Hexagonal Architecture.
I'm branching away from Alistair Cockburn and Bob Martin, who created and popularized this architecture/design.
Their presentations tend to be more bounded and focused upon _architecture_ whereas I feel this is really a _design_.
Architecture is a subset of design, but I think that good design can be applied at any layer of abstraction.

Alistair Cockburn posted [this](https://twitter.com/TotherAlistair/status/1704531058023141490) on X/Twitter in September, 2023:
> For those who keep asking about #hexagonalarchitecture layers, here it is: 
There are only 2 layers: inside. outside. No relation to layers in Clean, Onion, DDD, Modular Monolith, nothing. Stop dragging them into the pic.
What you do inside each layer is all your biz, not mine.

I interpret his statement as saying that Hexagonal Architecture (Ports and Adapters) is only about the Port and the Adapter. The Port is inside. The Adapter is outside.

But I think he's throwing away so much richness of the design with this constrained definition.
He views this design as restricted to only those elements I represent on either side of the red hexagon.
I include more elements to the basic design, such as the purple hexagon and the Adapters relationships with External Frameworks and External Dependencies.

So much of Hexagonal design applies to _Clean, Onion, DDD, Modular Monolith_ and more. 
I plan to blog about the relationship of Hexagonal Architecture to these concepts. I've already blogged about the relationship of Hexagonal Architecture to Clean Architecture in [Hexagonal/Clean Compare and Contrast blog](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html).

<img src="https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg" alt="The Clean Architecture" title="Image Source: https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html" width = "55%" align="right" style="padding-right: 35px;">

Bob Martin adds a few more layers to Clean Architecture than Cockburn does in Hexagonal Architecture. Martin indicates dependency from the outer most **Frameworks and Drivers** ring pointing inward toward the **Interface Adapters** ring.
I think this is the wrong direction as I mentioned in the [Frameworks and Drivers](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html#frameworks-and-drivers) section of the comparision blog.
I represent this boundary as a purple hexagon, and in my diagrams the knowledge and dependency arrows point outward. 
My outward arrows will become important later.

# Refresher
This blog is a continuation of [Why It Works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html).
I'll highlight a points starting with my basic Hexagonal Architecture diagram:

<img src="/assets/HexArchHexagons.png" alt="Hexagonal Architecture" width = "90%" align="center" style="padding-right: 35px;">

[Pure Stable/Fixed](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#pure-stablefixed-elements) elements have arrows pointing into them, meaning they depend nothing. These elements in Hexagonal Architecture include:
* The Port/Interface/Contract
* The External Frameworks and External Dependencies
* The Red Hexagon Boundary

[Pure Unstable/Flexible](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#pure-unstableflexible-elements) elements have arrows pointing away from them, meaning nothing in the design depends upon them. They are invisible to the other elements in the design. These elements in Hexagonal Architecture include:
* The Configurer
* The Business Logic
* The Adapters
* The Purple Hexagon Boundary

Technically the Configurer and the Purple Hexagon Boundary are the only Pure Unstable/Flexible elements, but the Business Logic and Adapters only have a creation arrow pointing into them, which originate from the Configurer. I consider that pure enough for my needs.

Almost all design options presented here will be with these Pure Unstable/Flexible elements. All the fun is in the Adapter zone. Red Hexagons are mosly unchanged of the diagrams. They don't change as we _flex_ the Adapters. Likewise, the red external frameworks and depenencies don't  change either.

# I is for Interface, or is it?
Interface names start with `I` in C# as convention. It's reminiscent of [Hungarian Notation](https://en.wikipedia.org/wiki/Hungarian_notation), which I've not much cared for myself. Hungarian Notation always reminded me of Klingon.

Here's what Bob Martin has to say about this practice in his book: **Clean Code**:
> These are sometimes a special case for encodings. For example, say you are building an ABSTRACT FACTORY for the creation of shapes. This factory will be an interface and will be implemented by a concrete class. What should you name them? IShapeFactory and ShapeFactory? I prefer to leave interfaces unadorned. The preceding I, so common in today’s legacy wads, is a distraction at best and too much information at worst. I don’t want my users knowing that I’m handing them an interface. I just want them to know that it’s a ShapeFactory. So if I must encode either the interface or the implementation, I choose the implementation. Calling it ShapeFactoryImp, or even the hideous CShapeFactory, is preferable to encoding the interface.

I tend to agree with him on this.
I avoided the `I` prefix for interfaces in my career.
Then a few months ago, I stumbed upon this blog: [I, Interface](https://talesfrom.dev/blog/i-interface).
The author suggested thinking of `I` as _First Person Singular: I_ rathern than _Interface_.
This leads to interface names that still have the `I` prefix, but in a more declarative way that provides more context.
For example, we can define interfaces with names like:
* `IPlaceOrders`
* `IUpdateOrders`
* `ICancelOrders`
* `IPersistPlacedOrders`, which could be implemented by a class with a name like `PersistPlacedOrdersViaMongoDB`.

I'll be using this convention through most of my examples. Alistair Cockburn does something similar but with a slightly different structure. His naming convention starts with `For` as in:
* `ForPlacingOrders`
* `ForUpdatingOrders`

Both declare context, but I rather like the `I` prefix convention, and it's consistent with other `I` interface conventions.

# Deep and wide. Deep and wide. There's a fountain flowing deep and wide
Previous Hexagonal Architecture diagrams I've presented have mostly only included one Framework Adapter and one Dependency Adapter mainly to present the concepts as simply as possible. 
The Hexagonal Architecture deisgn is not restricted to one of each.

We have design flexibility in behavior and structure and then within each we have design flexibility with breadth and depth.

## Behavior and Structure
I borrowed these terms from Design Patterns, which defines many patterns into two categories:
* Structural, which tend to be about the interaction of classes and objects
* Behavioral, which tend to be about the behavior that emerges from these interactions.

I'll be using them to distinguish between flexibility that's used because the behavior defined in the domain requires it versus flexibility that's not behavior driven, but it's useful to keep the design more modular and flexible.

It's somewhat inspired by a quote attributed to Alistair Cockburn:
> If it's your decision, it's design; if not, it's a requirement.

Behavior variations are those that we pretty much have to do. Structural variations are those that we choose to do.

## Behavior and Breadth
The behavior can expand through breadth as needed.
The Hexagonal Architecture can support multiple frameworks and dependencies.
It does so, because the behavior requires it, meaning that it's a requirement.

Consider this diagram where the Business Logic needs to persist stuff, publish events and send email.
The Business Logic implementation will have references to these interfaces, because it's required to do so because the core domain requires it.

<img src="/assets/HexArchBehaviorBreadth.png" alt="Hexagonal Architecture with Multiple Adapters" width = "85%" align="center" style="padding-right: 35px;">

The Business Logic defines three dependencies based upon its domain:
* `IPersistStuff` implemented by `PersistStuffViaDB`, which delegates to a `DB`.
* `IPublishEvents` implemented by `PublishEventsViaKafka`, which delegates to `Kafka`.
* `ISendEmail` implemented by `SendEmailViaSparkPost`, which delegates to `SparkPost`.

Even without any code, this design hopefully makes sense.

There are two frameworks:
* `AndroidFramework` extended by `ManageStuffFromAndroid`, which translates Android based requests and delegates them to `IManageStuff`.
* `RESTFramework` extended by `ManageStuffFromRest`, which translates RESTful based requeests and delegates them to `IManageStuff`.

`ManageStuff` has no dependency or knowledge upon either of these Frameworks or the External Dependencies. `ManageStuff` is loosely couplied to the Frameworks and Dependencies fit Adapters, which are created via the `Configurer`.

I didn't have room to include the `Configurer` in the diagram, but if I had drawn it, it would have only been a purple rectangle with creation lines to each of the blue rectangles. Here's what the Configurer class might look like in Java:
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
This probably isn't exactly the way the Configurer would work.
Most likely, there would be two Configurers, one for each Framework option.
The REST based configuration would be similar to the above, except that `manageStuffFromAndroid` would not be created.
The other Configurer would be for the Android Framework.
It would create the Adapters as well, but these would most likely be Adapters that implement the dependency interfaces and delegate to a different set of External Dependencies that are part of the Android environment.

These multiple Frameworks and External Dependencies is why Alistair Cockburn chose a hexagon in his initial diagrams. When he presented the design with hand drawn images, each framework/dependency port/adapter pair had its own facet, which is a side of the hexagon. He wanted to convey that notion of pluggable ports around a polygon, and he wanted more sides, or facets, than the more traditional rectangle. Here's an example:

<img src="https://blog.allegro.tech/img/articles/2020-05-21-hexagonal-architecture-by-example/ha_example.png" alt="Hexagonal Architecture with Facets" width = "55%" align="center" title="Image Source: https://blog.allegro.tech/2020/05/hexagonal-architecture-by-example.html" style="padding-right: 35px;">

## Behavior and Depth
Many Hexagonal Architectures show external frameworks and dependencies as being outside of the system.
I view external frameworks and dependencies as anything that resides outside of the red hexagon regardless of its location. It could easily be another service in the system.
Consider this diagram where the primary business logic to Place Orders and then delegate to an Inventory Service.

<img src="/assets/HexArchBehaviorDepth.png" alt="Hexagonal Architecture delegating to another service" width = "90%" align="center" style="padding-right: 35px;">

This diagram features two red hexagons for two sets of interface contracts:
* `IPlaceOrders`
* `IManageInventory`

The red hexagons do not know about each other. The glue that connects them is `HandlePlacedOrdersViaManageInventory`, but it's really not the glue. It's the `Configurer` that glues all of the pieces together. Here's how it might look in Java:
```java
PlaceOrdersFromREST placeOrdersFromREST =
    new PlaceOrdersFromRest(
        new (PlaceOrders(
            new (HandlePlacedOrdersViaManageInventory(
                new (ManageInventory(
                    new (PersistInventoryViaDB())
                )
            )
        )
    );
```
Let's remove the hexagons and the Configurer to see what's left.
This chain can go on indefinitely.

<img src="/assets/HexArchBehaviorDepthChain.png" alt="Hexagonal Architecture delegation chain" width = "90%" align="center" style="padding-right: 35px;">

<img src="https://upload.wikimedia.org/wikipedia/commons/9/94/Structure_of_monomers_and_polymers.jpg?20180217054949" alt="Polymer chain" width = "25%" align="right" title="Image Source: https://commons.wikimedia.org/wiki/File:Structure_of_monomers_and_polymers.jpg" style="padding-right: 35px;">

This repeating sequence pattern reminds me of a descending stair case. It's the same repeating patterns of [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) and [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html). It can continue indefinitely, much like polymer chains.

## Structure and Breadth
Imagine you're working with this fairly straightforward design:

<img src="/assets/HexArchStructureBreadth1.png" alt="Basic Hexagonal Architecture" width = "85%" align="center" style="padding-right: 35px;">

Your architect declares that the system is going to be more distributed and that **Domain Events** will be added. So when `Stuff` is persisted, then a Domain Event, such as `PersistedStuff` needs to be created and disseminated on a Message Service.

The diagram in the **Breadth and Behavior** section above shows how this could be done with a new contract for Publishing Events, but is this really a Behavior update? Is the Customer or even the Product Manager asking Domain Events? Probably not. Consider Cockburn's quote above, this is not a requriement. It's a design decision. We'd like to be able to add **Domain Events** without touching anything inside the red hexagon.

The first consideration may be to update, or really replace, `PersistStuffViaDB` with a new Adapter that persists and sends event notifications, possibly with the name: `PersistStuffViaDBAndNotifyStuffViaMessageService`. This is one of the reasons that we favor Adapters. They allow us to swap one out for another one smoothly.

The name is rather long, but I have a different problem with it. It contains **And**. This method is doing more than one thing. We may break the DB functionality when adding the Message Service functionality. The Adapter has more than one reason to be updated. In the future it could be updated because of DB dependencies or Message Service dependencies. An update to one feature runs the risk of breaking the other feature. This **And** Adapter violates the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single-responsibility_principle) (SRP).

Consider this design enhancement, which addes a new Adapter for the Message Service.

<img src="/assets/HexArchStructureBreadth2.png" alt="Basic Hexagonal Architecture with Message Service" width = "90%" align="center" style="padding-right: 35px;">

We no longer have an SRP violation. This is an application of the [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) design pattern.

But there's still a problem. `ManageStuff` only has one reference to `IHandleStuff`, but now we have two classes that implement `IHandleStuff`.
We could update `ManageStuff` to have two references to `IHandleStuff`, but now we're making changes in the red hexagon, and we want to avoid that, since this is an architecture update and not a behavior update.

One solution is to problem is to employ the [Composite](https://en.wikipedia.org/wiki/Composite_pattern) design pattern. Here is the design diagram followed by some implementation snippets to help describe it. Notice that the only difference between this diagram and the one above is the new `HandleStuffViaComposite` class and an update the to `Configurer`. Not only is the red hexagon content unaffected, but the other Adapters are unaffected as well.

<img src="/assets/HexArchStructureBreadth3.png" alt="Basic Hexagonal Architecture with Composite" width = "90%" align="center" style="padding-right: 35px;">

I will blog about **Composite** in the future, but here's a brief update of how it can solve our problem.

Let's assume that this is the declaration for `IHandleStuff`:
```java
interface IHandleStuff {
    void handle(Stuff stuff);
}
```
**NOTE**: The `IHandleStuff` interface really has not context. And the notion of possible Exceptions is ignored. I'm more focused upon how this will handle sunny day scenarios. A real interface and subsequent implementations would require more context and rainy day scenarios as well.

Each of the three Adapters has to implement this. `PersistStuffViaDB` will _handle_ stuff in the DB. `NotifyStuffViaMessageService` will create a `StuffHandled` **Domain Event** and send notifications via the Message Service.

But things get very interesting with `HandleStuffViaComposite`, which doesn't have any external dependencies. It's only dependency is upon `IHandleStuff` both as its parent class as well as referenced interfaces. The diamond indicates it has a Container of `IHandleStuff` references. Here's the gist of what `HandleStuffViaComposite` would look like in Java:
```java
class HandleStuffViaComposite implements IHandleStuff {
    private List<IHandleStuff> stuffIHandle = new LinkedList<>();

    public void add(IHandleStuff iHandleStuff) {
        stuffIHandle.add(iHandleStuff);
    }

    public void handle(Stuff stuff) {
        for (IHandleStuff iHandleStuff : stuffIHandle) {
            iHandleStuff.handle(stuff);
        }
    }
}
```

So far, this is only potential. We need the Configurer to put all the pieces together. The Configurer would looke something like this:
```java
HandleStuffViaComposite handleStuffViaComposite = new HandleStuffViaComposite();
handleStuffViaComposite.add(new PersistStuffViaDB());
handleStuffViaComposite.add(new NotifyStuffViaMessageService());

ManageStuffFromRest manageStuffFromRest =
    new (ManageStuffFromRest(
        new (ManageStuff(handleStuffViaComposite)
    );
```

This will be the sequence of events when `ManageStuff` makes a call such as: `iHandleStuff.handle(stuff)`:
1. `handleStuffViaComposite` will iterate through its List of `iHandleStuff` references with the first being `persistStuffViaDB` and the second one being `notifyStuffViaMessageService.`
2. `handleStuffViaComposite` will invoke `persistStuffViaDB.handle(stuff)`, which will persist stuff in the DB.
3. `handleStuffViaComposite` will invoke `notifyStuffViaMessageService.handle(stuff)`, which will create the Domain Event and send notifications via the Message Service.
4. returns back up to `ManageStuff`.

`HandleStuffViaComposite` is a List of `IHandleStuff` references. It can be configured to manage as many as `IHandleStuff` Adapters as needed.

## Structure and Depth
**Composite** handles breadth, but what about depth? Guess what? **Composite** handles it as well. `HandleStuffViaComposite` is a List of `IHandleStuff` references, and it itself is also an `IHandleStuff` reference. It is self-referential, and it can contain references to other `HandleStuffViaComposite` references.

**WARNING**: A `HandleStuffViaComposite` reference should never contain a reference to itself either directly or indirectly. If it would, then its call to `handle(stuff)` would get stuck in an infinite loop.

Let's also assume that your architect has decided to send Domain Events to two Message Services, Kafka and RabbitMQ. We can remove `NotifyStuffViaMessageService` and replace it with two more Adapter classes:
* `NotifyStuffViaKafka`
* `NotifyStuffViaRabbitMQ`

Here's the updated design with more concrete Message Service Adapters:

<img src="/assets/HexArchStructureBreadth4.png" alt="Basic Hexagonal Architecture with Composite" width = "90%" align="center" style="padding-right: 35px;">

It's practically identical to the previous design. The only difference is in the Message Service Adapters and that we're going to configure a `HandleStuffViaComposite` to contain another `HandleStuffViaComposite`. Then we can update the `Configurer` as follows:
```java
HandleStuffViaComposite notifyStuffViaMessageServices = new HandleStuffViaComposite();
notifyStuffViaMessageServices.add(new NotifyStuffViaKafka());
notifyStuffViaMessageServices.add(new NotifyStuffViaRabbitMQ());

HandleStuffViaComposite handleStuffViaComposite = new HandleStuffViaComposite();
handleStuffViaComposite.add(new PersistStuffViaDB());
handleStuffViaComposite.add(notifyStuffViaMessageServices);

ManageStuffFromRest manageStuffFromRest =
    new (ManageStuffFromRest(
        new (ManageStuff(handleStuffViaComposite)
    );
```

And that's it. This Composite is a List, which contains a List. Technically Composite is more like a Tree where each non-terminal node, that is a Composite object, can branch to as many child nodes as possible, including more Composite object nodes.

Everytime we add a terminal concrete node to a Composite, we're expanding breadth. Everytime we add a non-terminal Composite node, we're expanding depth. Composite is one of my favorite design patterns. So little code and yet so many options.

# Inspired By A True Story
...

# Nested Hexagons
...

# Organization of Work and Conway's Law

# Summary
These techniques allow us to expand in breadth and depth at any time. It's not one or the other. We may not need this level of flexibility often, but when we do, it's good to have these tools in our toolbox.

# References
See previous blog [References](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html#references).

# RAW NOTES

Cockburn's Tweet and references to his photo images too. He references just 2 layers inside and out. His design basically only has a single red hexagon. He's literal and a bit to restrictive. He views it as an architecure. I view it as a pattern. I feel that the architecture view is a subset of the patterns. Don't restrict yourself.
https://twitter.com/TotherAlistair/status/1704531058023141490
https://twitter.com/TotherAlistair/status/1726618384614535624
https://twitter.com/TotherAlistair/status/1726618384614535624

Nested Hexagons. Pruple Hexagons are Pure Unstable/Flexible. We end up with a recursive structure so all elements of the overall pattern can be applied a level deeper and as deep as necessary.

Most HexArch presentstaions don't give a sense of scope. Is it the entire project? Is it a feature. Hex Arch seems to indicate an architecture. Cockburn views Ports as external ports. But I don't think he really cares all that much. Clean Arch tends to be at the User Story level as Bob Martin presents it. I think that Bounded Context is a good boundary. This uses a defintion from DDD for context, but still gives the designer the freedom to adjust as needed.

List who works on which parts of the design and when. Seniors, Juniors, Architects, etc. Get Business Analysists and Project/Product managers involved too. They should be able to read the business logic code. Even if they can't read code, they should be able to follow the gist of it. If not, then the implementation needs work. It's either too complex, has too much jargon or the ubiquituous language has not been used. Should be able to practice CI/CD too. Nothing "activates" until the Configurer creates the elements and assembles them into the design. Classes only depend upon interfaces so the design is modular and more easily maintained. Unit testing should be relatively simple to set up. The only classes that know other class types are the Configurers.

Dispatcher. Tell the Document story. Update the names in the diagram if possible.
