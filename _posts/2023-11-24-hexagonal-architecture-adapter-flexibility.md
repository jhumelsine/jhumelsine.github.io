---
title: Hexagonal Architecture – Adapter Flexibility
description: How a few design options in the Adapter Layer provides more flexibility
unlisted: true
---

# Introduction
This continues the [Hexagonal Architecture series](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html) with how Hexagonal Architecture affords itself to flexibility, mainly in the Adapter Layer.

In the [Why It Works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html) blog, I featured knowledge/dependency theory, and why I feel that the practice of that theory is one of the main reasons that Hexagonal Architecture works so well.
This blog entry will expand upon those ideas with some additional design options that may be useful in designs that feature Hexagonal Architecture principles.

These additional design options are not my original techniques but applying them within the context of Hexagonal Architecture is my idea.
I'm branching away from Alistair Cockburn and Bob Martin, who created and popularized this architecture/design.
Their presentations tend to be more bounded and focused upon the _architecture_ whereas I am more interested in the _design_.
Architecture is a subset of design, but I think that good design should be applied to any layer of abstraction.

Alistair Cockburn posted [this](https://twitter.com/TotherAlistair/status/1704531058023141490) on X/Twitter in September 2023:
> For those who keep asking about #hexagonalarchitecture layers, here it is:
> There are only 2 layers: inside. outside.
> No relation to layers in Clean, Onion, DDD, Modular Monolith, nothing.
> Stop dragging them into the pic.
> What you do inside each layer is all your biz, not mine.

I interpret his statement as saying that Hexagonal Architecture (Ports and Adapters) is only about the Port and the Adapter. The Port is inside. The Adapter is outside.

But I think he's discarding so much richness of the design with this constrained definition.
He views this design as restricted to only those elements that I represent on either side of the Red Hexagon in my design.
I include more elements to the basic design, such as the Purple Hexagon and the Adapters relationships with External Frameworks and External Dependencies.

So much of Hexagonal design applies to _Clean, Onion, DDD, Modular Monolith_ and more. 
I plan to blog about the relationship of Hexagonal Architecture to these concepts. I've already blogged about the relationship of Hexagonal Architecture to Clean Architecture in [Hexagonal/Clean Compare and Contrast](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html).

<img src="https://blog.cleancoder.com/uncle-bob/images/2012-08-13-the-clean-architecture/CleanArchitecture.jpg" alt="The Clean Architecture" title="Image Source: https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html" width = "55%" align="right" style="padding-right: 35px;">

Bob Martin adds a few more layers to Clean Architecture than Cockburn does in Hexagonal Architecture. Martin depicts dependency from the outer most **Frameworks and Drivers** ring pointing inward toward the **Interface Adapters** ring.
I think this is the wrong direction as I mentioned in the [Frameworks and Drivers](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html#frameworks-and-drivers) section of the comparison blog.
I represent this boundary as a Purple Hexagon, and in my diagrams the knowledge and dependency arrows point outward. 
My outward arrows will become important later.

# Refresher
This blog is a continuation of [Why It Works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html).
I'll highlight a few points here from that blog starting with my basic Hexagonal Architecture diagram:

<img src="/assets/HexArchHexagons.png" alt="Hexagonal Architecture" width = "90%" align="center" style="padding-right: 35px;">

[Pure Stable/Fixed](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#pure-stablefixed-elements) elements have arrows pointing into them, meaning they depend upon nothing. These elements in Hexagonal Architecture include:
* The Port/Interface/Contract
* The External Frameworks and External Dependencies
* The Red Hexagon Boundary

[Pure Unstable/Flexible](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#pure-unstableflexible-elements) elements have arrows pointing away from them, meaning nothing in the design depends upon them. They are invisible to the other elements in the design. These elements in Hexagonal Architecture include:
* The Configurer
* The Business Logic
* The Adapters
* The Purple Hexagon Boundary

Technically the Configurer and the Purple Hexagon Boundary are the only Pure Unstable/Flexible elements, but the Business Logic and Adapters only have a creation arrow pointing into them, which originate from the Configurer. I consider that pure enough for my needs.

Almost all design options presented here will be with these Pure Unstable/Flexible elements. All the fun is in the Adapter zone. Red Hexagons are mostly unchanged in the diagrams. They don't change as we _flex_ the Adapters. Likewise, the Red External Frameworks and Dependencies don't change either.

# I is for Interface, or is it?
Interface names start with `I` in C# by convention. It's reminiscent of [Hungarian Notation](https://en.wikipedia.org/wiki/Hungarian_notation), which I've not much cared for myself. Hungarian Notation always reminded me of Klingon.

Here's what Bob Martin has to say about this practice in his book: **Clean Code**:
> These are sometimes a special case for encodings. For example, say you are building an ABSTRACT FACTORY for the creation of shapes. This factory will be an interface and will be implemented by a concrete class. What should you name them? IShapeFactory and ShapeFactory? I prefer to leave interfaces unadorned. The preceding I, so common in today’s legacy wads, is a distraction at best and too much information at worst. I don’t want my users knowing that I’m handing them an interface. I just want them to know that it’s a ShapeFactory. So if I must encode either the interface or the implementation, I choose the implementation. Calling it ShapeFactoryImp, or even the hideous CShapeFactory, is preferable to encoding the interface.

I tend to agree with him on this.
I avoided the `I` prefix for interfaces in my career.
Then a few months ago, I stumbled upon this blog: [I, Interface](https://talesfrom.dev/blog/i-interface).
The author suggested thinking of `I` as _First Person Singular: I_ rather than _Interface_.
This leads to interface names that still have the `I` prefix, but in a more declarative way that provides more context.
For example, we can define interfaces with names like:
* `IPlaceOrders`
* `IUpdateOrders`
* `ICancelOrders`
* `IPersistPlacedOrders`, which could be implemented by a class named `PersistPlacedOrdersViaMongoDB`, which would delegate to MongoDB.

I'll be using this convention through most of my examples. Alistair Cockburn does something similar but with a slightly different structure. His naming convention starts with `For` as in:
* `ForPlacingOrders`
* `ForUpdatingOrders`

Both declare context, but I rather like the `I` prefix convention, and it's consistent with other `I` interface conventions.

# Deep and wide. Deep and wide. There's a fountain flowing deep and wide
Previous Hexagonal Architecture diagrams I've presented have mostly only included one Framework Adapter and one Dependency Adapter mainly to present the concepts as simply as possible. 
The Hexagonal Architecture design is not restricted to one of each.

We have design flexibility in behavior and structure and then within each we have design flexibility with breadth and depth.

## Behavior and Structure
I borrowed these terms from Design Patterns, which organizes most patterns into two categories:
* Structural, which tends to be about the organization and interaction of classes and objects.
* Behavioral, which tends to be about the behavior that emerges from these organizations and interactions.

I'll be using them to distinguish between flexibility that's used because the behavior defined in the domain requires it versus flexibility that's not behavior driven, but it's useful to keep the design more modular and flexible.

It's somewhat inspired by a quote attributed to Alistair Cockburn:
> If it's your decision, it's design; if not, it's a requirement.

Behavior variations are those that we have to do. Structural variations are those that we choose to do.

## Behavior and Breadth
The behavior can expand through breadth as needed.
The Hexagonal Architecture can support multiple frameworks and dependencies.
It does so, because the behavior requires it, meaning that it's a requirement.

Consider this diagram where the Business Logic needs to persist stuff, publish events and send email.
The Business Logic implementation will have references to these interfaces because the core domain behavior requires it.

<img src="/assets/HexArchBehaviorBreadth.png" alt="Hexagonal Architecture with Multiple Adapters" width = "85%" align="center" style="padding-right: 35px;">

The Business Logic defines three dependencies based upon its domain:
* `IPersistStuff` implemented by `PersistStuffViaDB`, which delegates to a `DB`.
* `IPublishEvents` implemented by `PublishEventsViaKafka`, which delegates to `Kafka`.
* `ISendEmail` implemented by `SendEmailViaSparkPost`, which delegates to `SparkPost`.

Even without any code, this design hopefully makes sense.

There are two frameworks:
* `AndroidFramework` extended by `ManageStuffFromAndroid`, which translates Android based requests and delegates them to `IManageStuff`.
* `RESTFramework` extended by `ManageStuffFromRest`, which translates RESTful based requeests and delegates them to `IManageStuff`.

`ManageStuff` has no dependency or knowledge upon either of these Frameworks or the External Dependencies. `ManageStuff` is loosely coupled to the Frameworks and Dependencies through Adapters, which are created via the `Configurer`. None of the classes depend upon nor have knowledge of each other.

I didn't have room to include the `Configurer` in the diagram, but if I had drawn it, it would have only been a purple rectangle with creation lines to each of the blue rectangles. Here's what the `Configurer` class might look like in Java:
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
This probably isn't exactly the way the `Configurer` would work.
Most likely, there would be two Configurers, one for each Framework option.
The REST based configuration would be similar to the above, except that `manageStuffFromAndroid` would not be created, and its name would probably be `RestConfigurerToManageStuff`.
The other Configurer would be for the Android Framework, and its name would probably be `AndroidConfigurerToManageStuff`.
`AndroidConfigurerToManageStuff` would create the Adapters as well, but these would most likely be different Adapters that implement the dependency interfaces and delegate to a different set of External Dependencies that are part of the Android environment.

Multiple Frameworks and External Dependencies is why Alistair Cockburn chose a hexagon in his initial diagrams. When he presented the design with hand drawn images, each framework/dependency port/adapter pair had its own facet, which he represented as a side of the hexagon. He wanted to convey that notion of pluggable ports around a polygon, and he wanted more sides, or facets, than the more traditional rectangle. Here's an example:

<img src="https://blog.allegro.tech/img/articles/2020-05-21-hexagonal-architecture-by-example/ha_example.png" alt="Hexagonal Architecture with Facets" width = "55%" align="center" title="Image Source: https://blog.allegro.tech/2020/05/hexagonal-architecture-by-example.html" style="padding-right: 35px;">

## Behavior and Depth
Many Hexagonal Architectures represent, or give the impression, that external frameworks and dependencies must be resources outside of the system, such as external vendor products, open source, etc.
I view external frameworks and dependencies as anything that resides outside of the Red Hexagon regardless of its source.
It could easily be another service in the system.
Consider this diagram where the primary business logic to Place Orders loosely interacts with an Inventory Service, which is still part of the product but still outside of the Red Hexagon.

In my view, it should not matter to the Red Hexagon whether its Adapters are connecting it to an external vendor, open source or another service or component of the system.

<img src="/assets/HexArchBehaviorDepth.png" alt="Hexagonal Architecture delegating to another service" width = "90%" align="center" style="padding-right: 35px;">

This diagram features two Red Hexagons for two sets of interface contracts:
* `IPlaceOrders`
* `IManageInventory`

The Red Hexagons do not know about each other. The glue that connects them is `HandlePlacedOrdersViaManageInventory`, but it's really not the glue. It's the `Configurer` that glues all of the pieces together. Here's how it might look in Java:
```java
PlaceOrdersFromREST placeOrdersFromREST =
    new PlaceOrdersFromREST(
        new PlaceOrders(
            new HandlePlacedOrdersViaManageInventory(
                new ManageInventory(
                    new PersistInventoryViaDB()
                )
            )
        )
    );
```
Let's remove the Hexagons and the Configurer to see what's left.

<img src="/assets/HexArchBehaviorDepthChain.png" alt="Hexagonal Architecture delegation chain" width = "90%" align="center" style="padding-right: 35px;">

<img src="https://upload.wikimedia.org/wikipedia/commons/9/94/Structure_of_monomers_and_polymers.jpg?20180217054949" alt="Polymer chain" width = "25%" align="right" title="Image Source: https://commons.wikimedia.org/wiki/File:Structure_of_monomers_and_polymers.jpg" style="padding-right: 35px;">

This chain can go on indefinitely.
This repeating sequence pattern reminds me of a descending staircase.
The repeating pattern alternates [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) and [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) patterns.
It can continue indefinitely, much like polymer chains.

## Structure and Breadth
Imagine you're working with this straightforward design:

<img src="/assets/HexArchStructureBreadth1.png" alt="Basic Hexagonal Architecture" width = "85%" align="center" style="padding-right: 35px;">

Your architect declares that the system is going to be more distributed, and that **Domain Events** will be added. So when `Stuff` is persisted, then a Domain Event, such as `PersistedStuff`, needs to be created and disseminated on a Message Service.

The diagram in the **Breadth and Behavior** section above shows how this could be done with a new contract for Publishing Events, but is this really a Behavior update? Is the Customer or even the Product Manager asking for Domain Events? Probably not. Consider Cockburn's quote above, this is not a requirement. It's a design decision. We'd like to be able to add **Domain Events** without touching anything inside the Red Hexagon.

The first consideration may be to update, or really replace, `PersistStuffViaDB` with a new Adapter that persists and sends event notifications, possibly with the name: `PersistStuffViaDBAndNotifyStuffViaMessageService`. This is one of the reasons that we favor Adapters. They allow us to swap one out for another one smoothly.

The name is rather long, but that's my main issue with it. It contains **And**. This method is doing more than one thing. We may break the DB functionality when adding the Message Service functionality. The Adapter has more than one reason to be changed. In the future it could be updated because of DB dependencies or Message Service dependencies. An update to one feature runs the risk of breaking the other feature. This **And** Adapter violates the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single-responsibility_principle) (SRP).

Consider this design enhancement, which adds a new Adapter for the Message Service.

<img src="/assets/HexArchStructureBreadth2.png" alt="Basic Hexagonal Architecture with Message Service" width = "90%" align="center" style="padding-right: 35px;">

We no longer have an SRP violation. This is an application of the [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) design pattern.

But there's still a problem. `ManageStuff` only has one reference to `IHandleStuff`, but now we have two classes that implement `IHandleStuff`.
We could update `ManageStuff` to have two references to `IHandleStuff`, but now we're making changes in the Red Hexagon, and we want to avoid that, since this is an architecture update and not a behavior update.

One solution is to problem is to employ the [Composite](https://en.wikipedia.org/wiki/Composite_pattern) design pattern. Here is the design diagram followed by some implementation snippets to help describe it. Notice that the only difference between this diagram and the one above is the new `HandleStuffViaComposite` class and an update to the `Configurer`. Not only is the Red Hexagon content unaffected, but the other Adapters are unaffected as well.

<img src="/assets/HexArchStructureBreadth3.png" alt="Basic Hexagonal Architecture with Composite" width = "90%" align="center" style="padding-right: 35px;">

I will blog about **Composite** in the future, but here's a brief update of how it can solve our problem.

Let's assume that this is the declaration for `IHandleStuff`:
```java
interface IHandleStuff {
    void handle(Stuff stuff);
}
```
**NOTE**: The `IHandleStuff` interface really has no context. And the notion of possible Exceptions is ignored. I'm more focused upon how this will handle sunny day scenarios. A real interface and subsequent implementations would require more context and rainy-day scenarios as well.

Each of the three Adapters has to implement this interface. `PersistStuffViaDB` will _handle_ stuff in the DB. `NotifyStuffViaMessageService` will create a `StuffHandled` **Domain Event** and send notifications via the Message Service.

But things get very interesting with `HandleStuffViaComposite`. It doesn't have any external dependencies. Its only dependency is upon `IHandleStuff` both as its parent interface as well as referenced interfaces. The diamond indicates it has a Container of `IHandleStuff` references. Here's the gist of what `HandleStuffViaComposite` would look like in Java:
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

So far, this is only potential. We need the `Configurer` to put all the pieces together. The `Configurer` would look something like this:
```java
HandleStuffViaComposite handleStuffViaComposite = new HandleStuffViaComposite();
handleStuffViaComposite.add(new PersistStuffViaDB());
handleStuffViaComposite.add(new NotifyStuffViaMessageService());

ManageStuffFromRest manageStuffFromRest =
    new ManageStuffFromRest(
        new ManageStuff(handleStuffViaComposite)
    );
```

Here the sequence of what happens when `ManageStuff` makes a call to: `iHandleStuff.handle(stuff)`:
1. `handleStuffViaComposite`, which is the reference for `iHandleStuff` in `ManageStuff`, will iterate through its List of `iHandleStuff` references with the first being `persistStuffViaDB` and the second one being `notifyStuffViaMessageService.`
2. `handleStuffViaComposite` will invoke `persistStuffViaDB.handle(stuff)`, which will persist stuff in the DB.
3. `handleStuffViaComposite` will invoke `notifyStuffViaMessageService.handle(stuff)`, which will create the Domain Event and send notifications via the Message Service.
4. It returns up to `ManageStuff`.

`HandleStuffViaComposite` is a List of `IHandleStuff` references. It can be configured to manage as many `IHandleStuff` Adapters as needed.

## Structure and Depth
**Composite** handles breadth, but what about depth? Guess what? **Composite** handles it as well.
`HandleStuffViaComposite` is a List of `IHandleStuff` references, and it, itself, is also an `IHandleStuff` reference.
It is self-referential, and it can contain references to other `HandleStuffViaComposite` references.

**WARNING**: A `HandleStuffViaComposite` reference should never contain a reference to itself either directly or indirectly.
If it were to contain a reference to itself, then its call to `handle(stuff)` would get stuck in an infinite loop.

Let's further assume that your architect has decided to send Domain Events to two Message Services, Kafka and RabbitMQ. We can remove `NotifyStuffViaMessageService` and replace it with two more Adapter classes:
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
    new ManageStuffFromRest(
        new ManageStuff(handleStuffViaComposite)
    );
```

And that's it. This Composite is a List, where one of the elements in the List also contains a List.
We can think of Composite as a Tree where each non-terminal node that is a Composite object can branch to as many child nodes as possible, including more Composite object nodes.

Every time we add a terminal concrete node to a Composite, we're expanding in breadth. Every time we add a non-terminal Composite node, we're expanding in depth.
Composite is one of my favorite design patterns. So little code and yet so many options.

# Inspired By True Events
I've mostly been doing a lot of verbal handwaving so far. Let's get into a real scenario. This story is inspired by true events, but I'm going to reduce it to its core elements.

I worked on a project where one of the features stored Documents for people. These Documents could be Word documents, PDFs, etc. For years, they were stored in a traditional database as a blob of bytes, because that was the only storage mechanism we had. The simplified diagram would look like this:

<img src="/assets/HexArchDocument1.png" alt="Basic Hexagonal Architecture with Document with DB" width = "80%" align="center" style="padding-right: 35px;">

This worked fine for a while, but we wanted to move from a monolith architecture to a more distributed architecture, and since Documents are just blobs of bytes, we wanted to store them in Cloud Storage.

We couldn't do a flash cut, because the product already had hundreds of millions of Documents in the traditional DB, and it would take too long to migrate them to Cloud Storage during a maintenance window, which we want to keep as short as possible. We could migrate the Documents via batch processing. This would take days if not weeks, so how do we ensure that the correct Document is always managed properly during the migration period?

We embarked on something called **Dual Reads and Writes**. Basically, the feature would delegate to two sets of datastores, DB and Cloud Storage, during the migration period. Documents would be managed in both. This is definitely a structural choice, not a behavior choice. So, we don't to avoid updates to the Red Hexagon as much as possible.

I'm going to simplify this and sanitize it quite a bit. The product contained 20 years of legacy code. It wasn't as clean as the diagram above suggests. I'll omit the trials and tribulations until we converged upon a design much like this.

<img src="/assets/HexArchDocument2.png" alt="Basic Hexagonal Architecture with Document with Dispatcher" width = "90%" align="center" style="padding-right: 35px;">

`PersistDocumentsViaDispatching` was the key to make it all work. This class is a variation of **Composite**. It's self-referential, but it's not going to be quite as flexible as the previous **Composite** example. `PersistDocumentsViaDispatching` will have indirect knowledge of the DB and Cloud Storage Adapters. Dispatching rules are also FeatureFlag controlled.

Let's look at some implementations.

I'm going to omit **update** operation to keep things simpler:
```java
interface IPersistDocuments {

    void add(PersonId personId, Document document);

    Optional<Document> getByPersonId(PersonId personId);

    void delete(PersonId personId);
}
```

`PersistDocumentsViaDB` would implement these methods and delegate to the DB. `PersistDocumentsViaCloudStorage` would implement them and delegate to Cloud Storage.

`PersistDocumentsViaDispatching` is more interesting, and keep in mind that more was needed for the real implementation, but this is the gist of it:
```java
class PersistDocumentsViaDispatching implements IPersistDocuments {
    private final FeatureFlags featureFlags;
    private final IPersistDocuments iPersistDocumentsViaDB;
    private final IPersistDocuments iPersistDocumentsViaCloudStorage;

    public PersistDocumentsViaDispatching(FeatureFlags featureFlags, IPersistDocuments iPersistDocumentsViaDB, IPersistDocuments iPersistDocumentsViaCloudStorage) {
        this.featureFlags = featureFlags;
        this.iPersistDocumentsViaDB = iPersistDocumentsViaDB;
        this.iPersistDocumentsViaCloudStorage = iPersistDocumentsViaCloudStorage;
    }

    public void add(PersonId personId, Document document) {
        if (featureFlags.isEnabled(CLOUD_SERVICE)) {
            iPersistDocumentsViaCloudStorage.add(personId, document);
        }

        if (featureFlags.isEnabled(DB)) {
            iPersistDocumentsViaDB.add(personId, document);
        }
    }

    public Optional<Document> getByPersonId(PersonId personId) {
        Optional<Document> documentViaCloudStorage = iPersistDocumentsViaCloudStorage.getByPersonId(personId);

        if (documentViaCloudStorage.isPresent()) {
            return documentViaCloudStorage;
        }

        Optional<Document> documentViaDB = iPersistDocumentsViaDB.getByPersonId(personId);
        if (documentViaDB.isPresent()) {
            if (!featureFlags.isEnabled(DB)) {
                internalNotification("Document not found in Cloud Storage, but found in DB when DB is disabled for PersonId = {}", personId);
            }
            return documentViaDB;
        }

        return Optional.empty();
    }

    public void delete(PersonId personId) {
        iPersistDocumentsViaDB.delete(personId);
        iPersistDocumentsViaCloudStorage.delete(personId);
    }
}
```

Finally, the `Configurer`:
```java
ManageDocumentsFromREST = manageDocumentsFromREST =
    new ManageDocumentsFromREST(
        new ManageDocuments(
            new PersistDocumentsViaDispatching(
                FeatureFlagAPI,
                new ManageDocumentsViaDB(),
                new ManageDocumentsViaCloudStorage()
            )
        )
    );
```
The feature is enabled via FeatureFlags for DB and CLOUD_STORAGE possibly in this order:
* Enable DB, which emulates the DB-only behavior that we started with.
* Enable CLOUD_STORAGE, which will activate dual writing so that Documents will be added to both the DB and Cloud Storage.
* Disable DB, when all Documents have been migrated, so that all Document management happens via Cloud Storage.

Each stage would be active for a few days, weeks or even months, with observation to ensure that it's working correctly.

I added one invariant confirmation observation and notification in the implementation above. If a Document is not found in Cloud Storage, but it is in the DB and the DB has been disabled, then return the DB's Document and notify someone that there's an inconsistency.

Other invariant confirmation observations and notifications may be desired, but I only provided this one as an example.

Once all Documents have been migrated, then there's no need for `ManageDocumentsViaDB` or even `PersistDocumentsViaDispatching`.
We'd end up with a design that looks like this:
<img src="/assets/HexArchDocument3.png" alt="Basic Hexagonal Architecture with Document with Dispatcher" width = "80%" align="center" style="padding-right: 35px;">

Nowhere in this design transition did we need to touch the Framework Adapter or any elements inside the Red Hexagon.

# Nested Hexagons
We've gone deep and wide. Now we're going to dive inward. Can Hexagons be nested?

Let's return to Alistair Cockburn, who posted [this](https://twitter.com/TotherAlistair/status/1726618384614535624) on X/Twitter in November 2023:
> someone asked me today: "If you were to define what hexagonal architecture is not from the observations you've made of implementation variations over the years, what would you say?"
>
> Here are the pages from the draft book that say as well as I can in just a few pages:

Ah! He's working on a book, and he posted images of a few pages. On [Page 4](https://twitter.com/TotherAlistair/status/1726618384614535624/photo/4) he writes:
> **What about nested hexagons?**
>
> As described in paper _"Component + Strategy"_  in Chapter 1, you can nest the Component + Strategy pattern, assuming you really write the tests.
> You can have components within components, if you like, assuming you meet the criteria.
>
> The Ports & Adapters pattern is really aimed at protecting your team from external technology shifts.
> The pattern suggests declaring the system boundary just in front of each external technology.
> See "Where do I put the 'app' boundary?"
>
> Thus we say that the Hexagonal / Ports & Adapters pattern does not nest.

I think Cockburn is being too restrictive. As I stated above, I think he views the Port as the edge of the system. I view the Port as the edge of the Business Logic. While Cockburn doesn't think the pattern nests, I do.

I considered how to represent nested hexagons. The diagram was going to get way to cluttered. What did I even consider nested hexagons?

I often think about my blogs while I drift to sleep. While dozing off one evening I had a thought. My relationship lines leaving my Purple Hexagons always point outward. Cockburn doesn't address this at all. Martin gets it backwards.

Purple Hexagons are [Pure Unstable/Flexible](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#pure-unstableflexible-elements) elements. But they are not the only Pure Unstable/Flexible elements in Hexagonal Architecture. Others include:
* The Business Logic
* The Adapters
* The Configurer

Except for their creation, they are invisible to the rest of the design. All of these elements have an [Event Horizon](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#event-horizons) much like Black Holes. We can't peer into them. This was a bit of an **Aha!** moment for me. It was there all along, and I hadn't noticed it before. Purple Hexagons can be elements within Purple Hexagons. They are self-referential too.

A picture is worth a thousand words.
All Pure Unstable/Flexible elements can be Purple Hexagons too. Except for creation, all dependency/knowledge arrows point outward.

<img src="/assets/HexArchNested1.png" alt="Basic Hexagonal Architecture with Nested Hexagons" width = "90%" align="center" style="padding-right: 35px;">

This is a fractal design. At this level of abstraction, we know what each internal Purple Hexagon must do, but we don't know nor care how it's done. It could be one class, or it could be many classes. There could be another Red Hexagon eco-system or a completely different design inside. The fractals could descend even further with more Purple Hexagons nesting within these Purple Hexagons.

If the External Dependencies are honored, the developer has carte blanche with respect to the design and implementation within the Purple Hexagon.

Let's revisit the Dispatching example, but with a nested hexagon:

<img src="/assets/HexArchNested2.png" alt="Dispatching Hexagonal Architecture with Nested Hexagons" width = "90%" align="center" style="padding-right: 35px;">

The only difference is the addition of the `PersistingConfigurer` as well as adding the Purple Hexagon. Please remember that Hexagons, regardless of their color, are design boundaries. They do not exist in the implementation. They help organize the design and restrict the flow of dependency and knowledge.

Nothing has really changed except for the Configurers, and I think they will be easier to manage.
`PersistingConfigurer`:
```java
class PersistingConfigurer {
    public static IPersistDocuments getDocumentPersister() {
        return new PersistDocumentsViaDispatching(
            FeatureFlagAPI,
            new ManageDocumentsViaDB(),
            new ManageDocumentsViaCloudStorage()
        );
    }
}
```

Then the main `Configurer` is basically this:
```java
ManageDocumentsFromREST = manageDocumentsFromREST =
    new ManageDocumentsFromREST(
        new ManageDocuments(
            PersistingConfigurer.getDocumentPersister()
        )
    );
```

# Separation of Concerns
These designs are highly modular. This was hopefully evident as I was swapping and adjusting Adapter configurations. Except for creation, the classes don't depend upon or know about one another. They only depend upon interfaces.

The separation of concerns means that different developers and different teams can work on the implementation simultaneously without interfering with one another if their shared interfaces are reasonable stable. All implementations are encapsulated within their Event Horizons. If they honor their dependencies, any internal design and implementation is possible since it's invisible to the rest of the design.

Unit testing should be relatively easy to set up since there are no tight couplings. Test doubles can be provided for all dependencies.

Who should be working on what and when? There are many ways to approach this. Here are some of my personal thoughts:
* Architects and Senior Developers should declare the Port/Interface/Contracts. These are Pure Stable/Fixed elements. Other elements depend upon and have knowledge of them. We don't want them to change often. Architects and Senior Developers should have the most domain knowledge to do this.
* Senior Developers should implement the Business Logic, and this would include Business Objects/Entities as well. Core domain implementations reside here. This is where customer observed behavior resides. It needs to be right. But that doesn't mean that it needs to be complex or complicated. Technical details should be delegated to the Port/Interface/Contracts, which are implemented by Adapters and delegated externally. The Business Logic should be reasonably straight forward. I think it should be obvious enough so that your Business Analysts and Project/Product Managers can follow the gist of the Business Logic implementation. If not, then the design needs more work or refactoring.
* Junior Developers should implement the Adapters and probably with Senior Developer oversight. Adapters won't have business logic within them. This can be a good place where Junior Developers learn the technology, dependencies, and the project's ubiquitous language.
* Architects should implement the Configurers. Nothing happens until the Configurers create and configure the objects. The challenge of the Configueres isn't in their implementations. It's in knowing what parts of the system need to be created and configured. Getting this correct requires broad knowledge of the system. Configurers are responsible for organizing the different elements of the system, and the Architects should know this the best.

# Summary
These techniques allow us to expand in breadth and depth at any time. It's not one or the other. We may not need this level of flexibility often, but when we do, it's good to have these tools in our toolbox.

# References
Here are some free resources:
* [Summary of Clean Code #1](https://gist.github.com/wojteklu/73c6914cc446146b8b533c0988cf8d29) on Github summarized by Wojtek Lukaszuk
* [Summary of Clean Code #2](https://gist.github.com/cedrickchee/55ecfbaac643bf0c24da6874bf4feb08) on Github summarized by Cedric Chee
* [Composite Design Pattern in Wikipedia](https://en.wikipedia.org/wiki/Composite_pattern)
* [Composite Design Pattern in SourceMaking](https://sourcemaking.com/design_patterns/composite)
* [Composite Design Pattern in Refactoring Guru](https://refactoring.guru/design-patterns/composite)

Here are some resources that can be purchased or are included in a subscription service:
* **Clean Code** by Bob Martin ([O'Reilly](https://learning.oreilly.com/library/view/clean-code-a/9780136083238/) and [Amazon](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882))

See previous blog [References](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html#references).
