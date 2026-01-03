---
title: Specification Design Pattern
description: Allow a Client to select or filter objects with specific attribute property values as specified by the Client.
---
<img src="https://www.picpedia.org/chalkboard/images/specification.jpg" alt="Specification Sign" title="Image Source: https://www.picpedia.org/chalkboard/s/specification.html" width = "80%" align="center" style="padding-right: 20px;">
 
# Introduction
The Specification Design Pattern is the next of the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html)  series. It does not reside within the Gang of Four’s (GoF) Design Pattern catalog.

The Specification Design Pattern allows a Client to identify objects via an attribute value based specification defined by the Client. The attribute specification can be simple or complex. There are several use case scenarios featuring specifications, such as:
* Selects specification satisfying objects from a repository.
* Filters specification satisfying objects within an iterator. This is similar to the above.
* Filters specification satisfying objects in a stream. Also similar to the above.
* Requests notifications for specification satisfying object subscriptions. For example, subscribe to an object and request notifications only when one of its attributes crosses a threshold value.
* Defines specification satisfying alert criteria, which is similar to the above as well.

Specification is similar to the select or query feature in databases. The main distinction is that the objects being considered within Specification are objects and not records in a database.

The Specification Design Pattern will feature several previous design patterns:
* [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) resides at its core, but then again, Strategy resides at the core of many design patterns we’ve reviewed so far. 
* [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) will extend the ability of the Client to construct complex Specifications, since Strategy on its own will only be able to construct simple Specifications. 
* [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) will be used for Specification management.
* And we’ll see [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) as well, but it will be a little different than before. Previous design patterns featured a Configurer who created objects organized in a design pattern structure and injected them into the Client. In Specification, Clients create their own Specifications and inject them into the pattern at any time.

# Real World Analogies to Specification
Like the previous composable design patterns, Specification is not difficult to implement. It’s an extension of Composite. Its main challenge is comprehension. Here are a few real-world examples to ease one into the Specification concept.

## Matchmaker, Matchmaker, Make Me a Match
The musical __Fiddler on the Roof__ features the song [_Matchmaker_](https://www.youtube.com/watch?v=59Hj7bp38f8) sung by Tevye’s three oldest daughters speculating upon whom the local matchmaker might find for their husbands.

<img src="https://live.staticflickr.com/3714/9550526732_3c078bd805_o.jpg" alt="Fiddler on the Roof Poster" title="Image Source: https://www.flickr.com/photos/portlandcenterstage/9550526732" width = "45%" align="right" style="padding-right: 20px;">

Here are several lyrics:
<BR> _Matchmaker, Matchmaker,
<BR>Make me a match,
<BR>Find me a find,
<BR>Catch me a catch
<BR>Matchmaker, Matchmaker
<BR>Look through your book,
<BR>And make me a perfect match
<BR>Matchmaker, Matchmaker,
<BR>I'll bring the veil,
<BR>You bring the groom,
<BR> __Slender__ and __pale__.
<BR>Bring me a ring for I'm longing to be,
<BR>The envy of all I see.
<BR>For Papa,
<BR>Make him a __scholar__.
<BR>For mama,
<BR>Make him __rich__ as a king.
<BR>
<BR>For me, well,
<BR>I wouldn't holler
<BR>If he were as __handsome__ as anything._

The lyrics define the specification: `(build==slender AND complexion==pale AND education==scholar AND wealth==rich) OR (looks==handsome)`. They want the Matchmaker to look through her list for a groom for those who match this specification. While not in the lyrics, should a new potential groom match the specification, then they would like to be notified as well.

## Collectors
The TV show __American Pickers__ featured antique collectors Mike and Frank driving around rural American looking for trash that they could convert into treasures and sell at their antique store in the Midwest.

<img src="https://live.staticflickr.com/4083/5180513438_8f4144ec71_b.jpg" alt="Junk Barn" title="Image Source: https://www.flickr.com/photos/theresearchman/5180513438" width = "50%" align="right" style="padding-right: 20px;">

The show focused upon the interesting characters they encountered on the road, the items these characters had collected, and Mike and Frank negotiating a price.

These collections often look like a hoarder’s stash stored in a leaning barn about to topple over. Several times the guys would launch themselves onto unstable filth looking for that diamond in the rough. I’m sure they had to keep their tetanus shots up to date.

The treasures they’d find tended to be antique toys, car parts, motorcycle parts, old gas station signs, etc. The collectors often had detailed knowledge of anything pulled from the piles. They could often remember when and where they acquired something and how much they paid for it.

Quite frankly most of the items Mike and Frank gushed over just looked like junk to me. But they knew the specifications for what their customers wanted and what they’d be willing to pay for them.

## Google Searches and Alerts, especially Jobs
Most online search engines are a form of Specification. Your search query is a specification, and the search engine will find and return pages that best match that specification. Google will allow you to create an alert for that specification query so that it will send you an email when it finds additional pages that match that specification.

Google’s Job UI/UX is even closer to Specification. Go to: [https://www.google.com/search?q=jobs](https://www.google.com/search?q=jobs) and click on the Jobs banner at the top.

<img src="/assets/SpecificationJobs2.png" alt="Google Jobs Portal"  width = "70%" align="right" style="padding-right: 35px;">

This is a Google Jobs portal that will let anyone define a Job Specification based upon job title, location, full/part time, keywords, etc. as shown in the provided image. The Specification can be fine-tuned as much as needed, and Google will refresh the job list upon each update. Once you have a Job Specification you like, you can save it as an Alert so that Google will send you an email when it finds new jobs that match your Specification.

Each job searcher can customize his or her own Job Specification. And job searchers are not limited to just one Specification. They can create different Specifications with different criteria.

# Design Structure
Specification is an extension of Composite, but with a few enhancements. We have a few steps before we get there.

## Problem Description
We have a `Client` who is interested in a selection of `Context` objects based upon a `Specification` defined by the `Client`. The `Specification` is passed to a `ContextManager` which will return those `Context` objects that satisfy the `Specification`.

That description was awful. Let me match the Problem Description terms using the examples above:

| Problem Description | Matchmaker | Antiques | Google Jobs |
| :-------------------------- | :--------------- | :------------ | :---------------- |
| Client | Young Woman | Customers back at the shop | A person looking for a job |
| ContextManager | Matchmaker  | Mike and Frank | Google |
| Context  | Husband Candidate  | Antique/Collector Item residing in a leaning barn | An online Job opening |
| Specification | `(build==slender AND complexion==pale AND education==scholar AND wealth==rich) OR (looks==handsome)` | Items their customers are interested in | Job location, title, salary, etc. |

Here’s a UML class diagram for this behavior:
* At this level of abstraction, the diagram only shows the `Specification` interface. `Specification` details will be provided shortly. `Specification` declares one method that will return a boolean indicating whether the `Context` satisfies the `Specification`.
* The `Client` has a reference to `ContextManager`, which was probably injected into it. If I had more space, I’d have shown an interface for the `ContextManager` and `Client` would have had a reference to the interface probably resolved by a `Configurer`.
* The `Client` acquires a `Specification` reference. Details coming soon.
* The `Client` calls `ContextManager`’s `getContextsBy(Specification specification)`, which will return the list of `Context` objects that satisfy the `Specification`. We can view `specification` as an argument being passed in, but I like to think of it as a form of [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html). `ContextManager` depends upon a `Specification` for each method call.
* `ContextManager` contains a List of `Context` objects. Its method iterates that List and adds a `Context` to the list of `satisfiedContexts` when a `Context` object satisfies the injected `Specification`. The `ContextManager` has no knowledge of or dependency upon the `Specification` implementation. Its only concern is interacting with the [contract](https://jhumelsine.github.io/2025/06/10/contracts.html) interface.

<img src="/assets/SpecificationOverview.png" alt="Specification Overview"  width = "90%" align="center" style="padding-right: 35px;">
 
This is the last time I will show details for `Client` and `ContextManager`. It’s too much to repeat in subsequent diagrams.

## Strategy
Let’s get some ideas for how we could implement `Specification`. Let’s start with the [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html).

We can do quite a bit with Strategy:
* The top row of elements is the same as the previous diagram except that the implementation details have not been shown.
* This diagram has added a little more context to `Context`. While only as examples, `Context` has a Color and Shape attribute. I thought this would be a bit more meaningful than AttributeA and AttributeB.
* We have three `Specification`s based upon these two attributes: Color and Shape. Their attributes are `final` so that the `Specification` cannot be modified after being created.

<img src="/assets/SpecificationStrategy.png" alt="Specification via Strategy" width = "100%" align="center" style="padding-right: 35px;">

Let’s see how the `Client` creates `Specification`s. Here are some examples:
```java
ContextManager contextManager; // injected possibly via a Configurer, not shown
...
List<Context> redContexts contextManager.getContextsBy(new ColorSpecification(Color.RED));
List<Context> circleContexts contextManager.getContextsBy(new ShapeSpecification(Shape.CIRCLE));
List<Context> blueSquareContexts contextmanager.getContextsBy(new ColorAndShapeSpecification(Color.BLUE), Shape.SQUARE));
...
```

## Composite
Strategy will work for simple specifications with one attribute, but the `ColorAndShapeSpecification` is a bit of a concern. It works fine, but it’s not scalable.
What if there are multiple specifications with other attributes for `Context`? What about Color __OR__ Shape? What about __NOT__? We don’t want to create a new `Specification` from scratch when we want a new combination of attributes. The number of potential combinations explodes exponentially.

This is where [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) can be useful. This design maintains the previous design, except that it removes the `ColorAndShapeSpecification` and replaces it with three new composable Specifications. Each of these new `Specification`s is a composite. These `Specification` composites have their own specific behavior within the context of the composite structure. Each of these Boolean operation-based composites contains other `Specification`s where the composite return value is based upon the return values of the `Specification`s they contain such that:
* `AndSpecification` - Returns true when all its contained `Specification`s return true; otherwise, false.
* `OrSpecification` - Returns true when any of its contained `Specification`s returns true; otherwise, false.
* `NotSpecification` - Returns true when its single contained `Specification` returns false, and false when it returns true.

Here is the design:
* It looks a little busy, but it’s not horrible. Consider each rectangle in isolation and its dependencies based upon its outward pointing arrows as was described in [Hexagonal Architecture – Why it works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html). This is not a Hexagonal Architecture design, but the same dependency management benefits still apply.
* Each class has limited knowledge of the overall design. Except for the `Client`, the others only have knowledge of and depend upon one or two other classes or the interface.

<img src="/assets/SpecificationComposite1.png" alt="Specification via Composite" width = "100%" align="center" style="padding-right: 35px;">

### Specification Example via Composite
The previous `Client` `Specification` examples become:
```java
ContextManager contextManager; // injected possibly via a Configurer, not shown
...
List<Context> redContexts contextManager.getContextsBy(new ColorSpecification(Color.RED)); // Unchanged
List<Context> circleContexts contextManager.getContextsBy(new ShapeSpecification(Shape.CIRCLE)); // Unchanged

Specification blueSquareSpecification = new AndSpecification();
blueSquareSpecification.add(new ColorSpecification(Color.BLUE));
blueSquareSpecification.add(new ShapeSpecification(Shape.SQUARE));
List<Context> blueSquareContexts contextmanager.getContextsBy(blueSquareSpecification); // Composed
...
```

Let’s examine `blueSquareSpecification`. It’s a _BLUE_ `ColorSpecification` and a _SQUARE_ `ShapeSpecification` which are then composed within an `AndSpecification`, such that a `Context` will be satisfied when it is both _BLUE_ and _SQUARE_.

The `AndSpecification` will iterate both. If either of them returns `false`, then it will return `false`. If both return `true`, then it will return `true`.

These three Boolean composite `Specification`s give us almost infinite possibilities in constructing a `Specification` without having to add new code except when a new `Specification` leaf class is needed for a new other attribute.

### Specification Example via Showtunes
Here’s the composite `Specification` for the Matchmaker as mentioned in the example above:
```java
Matchmaker matchMaker; // injected possibly via a Configurer, not shown
...
Specification idealHusbandSpecification = new AndSpecification();
idealHusbandSpecification.add(new BuildSpecification(Slender));
idealHusbandSpecification.add(new ComplexionSpecification(Pale));
idealHusbandSpecification.add(new EducationSpecification(Scholar)); // For Papa
idealHusbandSpecification.add(new WealthSpecification(Rich)); // For Mama

Specification husbandSpecification = new OrSpecification();
husbandSpecification.add(idealHusbandSpecification);
husbandSpecification.add(new LooksSpecification(Handsome));

List<Husband> husbandCandidates matchMaker.getContextsBy(husbandSpecification);
...
```

The composite tree for the Matchmaker `Specification` would look like this:

<img src="/assets/SpecificationMatchmakerObjects.png" alt="Matchmaker Composite Tree" width = "90%" align="center" style="padding-right: 35px;">

* The class types are on the right of the colon.
* The object name is on the left of the colon.
* Anonymous objects don’t have names, so there’s nothing to the left of the colon.
* Attribute values reside under the object:class names.
* A candidate husband will be a match if he satisfies all the Slender/Pale/Scholar/Rich specifications or the single Handsome specification.
 
While this violates the specification order in the lyrics, one might wish to reconfigure the specification to be more efficient:
* If the Husband Candidate is Handsome, then we don’t need to check the rest of the specification. Being Slender, Pale, Rich or a Scholar is just a bonus.
* If the Husband Candidate is not Handsome, then he must be Slender, Pale, Rich, and a Scholar. It’s less likely that he’s Rich or a Scholar than Slender and Pale. Therefore, as soon as a non-Handsome and non-Rich Husband Candidate is encountered, then we can eliminate him quickly without checking the other attributes.

<img src="/assets/SpecificationMatchmakerObjectsOptimal.png" alt="Matchmaker Composite Tree with Optimal Order" width = "90%" align="center" style="padding-right: 35px;">
 
### My Final Comment, Or My Comment About Final
The leaf `Specification` classes contain attributes such as `Color` or `Shape`. I declared them as `final` so that they could not be changed. Once a `Specification` is defined, I don’t want to worry about it being updated in such a way that it exhibits different behaviors. If a new `Specification` behavior is needed, then a new `Specification` instance should be created.

I did the same with `NotSpecification`. The context is slightly different, but it’s still the same intent. I don’t want the ability to change a `NotSpecification` instance’s behavior by changing its `Specification` attribute.

The leaf `Specification` classes and `NotSpecification` attributes can be declared as `final` and initialized in their respective constructors.

However, we can’t easily use the same constructor technique with `AndSpecification` and `OrSpecification`. If we want the ability to add `Specification`s individually as I showed in the previous examples, then we have to allow objects of these classes to be updated via `add(Specification specification)` after they’ve been constructed.

There is a way to achieve _finalness_ with a slightly different technique. I’m going to add a simple 2-state state machine to `SpecificationComposite`:
* __Initializing__ – This is the start state representing when the `SpecificationComposite` is being initialized. When in this state, `add(Specification specification)` can add a `specification` to the `specifications` list.
* __Not Initializing__ – This is the state representing when the `SpecificationComposite` is no longer being initialized. When in this state, `add(Specification specification)` will throw an `InvalidStateException`. There is no event transition from this state back to the Initializing state.
* `isSatisfied(Context context)` - This is the event that transitions from the Initializing state to the Not Initializing state.

Normally, I prefer a separate state machine class, but since this state machine is so simple and localized, I’m going to represent it via the `isInitializing` boolean attribute to keep track of the state and which actions are permitted based upon that boolean attribute state. I'm using the [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) to keep the state machine in the common base class. `AndSpecification` and `OrSpecification` are only concerned with the satisfiability of the `context` with `specifications`. They are not concerned with the management of the `specifications`.

When an `AndSpecification` or `OrSpecification` is created, as many `Specification`s can be added to them as needed. But once they are activated with an `isSatisfied(Context context)` call, then they are no longer being initialized. No new `Specification`s can be added.

<img src="/assets/SpecificationFinal.png" alt="Specification with Finals" width = "100%" align="center" style="padding-right: 35px;">
 
The leaf `Specification`s are [Value Objects](https://en.wikipedia.org/wiki/Value_object). Once initialized, they cannot be modified. The `SpecificationComposite` objects are pseudo-Value Objects. Once activated, they cannot be modified.

Leaf objects in the composite tree are immutable. The non-terminal objects will be immutable once activated. The entire composite tree is a pure function. That means that Specification, and Composite in general, are thread-safe structures. Any number of threads can be calculating satisfiability simultaneously without concern of affecting each other. State resides within the Context argument.

# Use Case
This blog entry is too large to include the Use Case. It will be posted in the next [blog](https://jhumelsine.github.io/2024/03/07/specification-design-pattern-use-case.html).

# Specification Pros and Cons
The relative pros and cons of Specification are like those with most of the Composable design patterns. Its flexibility is both a pro and a con.

The implementation for Specification is shockingly small. Almost the entire implementation has been shown in the code snippets above. [Unit testing](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html) will also be trivial.

The power of Specification resides in giving the Client ability to design their own Specifications. Clients can construct a Specification with almost any Boolean satisfiability expression needed. It could be simple. It could be complex. When they configure a logical mistake, they'll probably blame you first.

# Summary
Specification is not in the GoF as its own concept, but it's an extension of the Composite structure. It will tend to have more narrow uses than some of the other design patterns, but when you fall into that narrow use case, it's good to have.

# References
Since Specification is not in the GoF design pattern catalog and a bit on the margins of design patterns, there aren't as many online resources as with other design patterns.
Here are a few free resources:
* [Specifications. Original article by Eric Evans and Martin Fowler](https://martinfowler.com/apsupp/spec.pdf)
* [Wikipedia Specification Design Pattern](https://en.wikipedia.org/wiki/Specification_pattern)
* [DevIQ Specification Pattern](https://deviq.com/design-patterns/specification-pattern)
* [Design Pattern Specification Blog](https://marcaube.ca/2015/05/specifications)
* [InfoWorld: How to use the specification design pattern in C#](https://www.infoworld.com/article/3710289/how-to-use-the-specification-design-pattern-in-c-sharp.html)
* and for more, Google: [Specification Design Pattern](https://www.google.com/search?q=specification+design+pattern)
