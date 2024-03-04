---
title: Specification Design Pattern
description: Allow a Client to select or filter objects with specific attribute property values as specified by the Client.
unlisted: true
---
<img src="https://www.picpedia.org/chalkboard/images/specification.jpg" alt="Specification Sign" title="Image Source: https://www.picpedia.org/chalkboard/s/specification.html" width = "60%" align="center" style="padding-right: 20px;">
 
# Introduction
The Specification Design Pattern is the next of the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html)  series. It does not reside within the Gang of Four’s Design Pattern catalog.

Specification allows a Client to select objects based upon the attribute values that the Client is interested in. The attribute specification can be simple or complex. It can be used in several scenarios, such as:
* Selecting the objects that satisfy a specification based upon their attribute values.
* Filtering objects, possibly in a stream, that satisfy a specification based upon their attribute values.
* Requesting notification of objects changing attribute values that satisfy a specification based upon their attribute values.

Specification is similar to the select or query feature in databases. The main distinction is that the objects being considered within Specification are objects and not records in a database.

The Specification Design Pattern will feature several previous design patterns:
* [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) resides at its core, but then again, Strategy resides at the core of many design patterns we’ve reviewed so far. 
* [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) will extend the ability of the Client to construct complex Specifications, since Strategy on its own will only be able to construct simple Specifications. 
* [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) will be used to consolidate pseudo-duplicate code.
* And we’ll see [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) as well, but it will be a little different than before. Previous design patterns featured a Configurer who created objects organized in a design pattern structure and injected them into the Client. In Specification, Clients create their own Specifications and inject them into the pattern at any time.

# Real World Analogies to Specification
Like the previous composable design patterns, Specification is not difficult to implement. In fact, it’s an extension of Composite. Its main challenge is comprehension. Here are a few real-world examples to ease one into the Specification concept.

## Matchmaker, Matchmaker, Make Me a Match
The musical Fiddler on the Roof features the song “[Matchmaker](https://www.youtube.com/watch?v=59Hj7bp38f8)” sung by Tevye’s three oldest daughters speculating upon whom the local matchmaker might find for their husbands.

<img src="https://live.staticflickr.com/3714/9550526732_3c078bd805_o.jpg" alt="Jet Engine Cutaway" title="Image Source: https://www.flickr.com/photos/portlandcenterstage/9550526732" width = "60%" align="right" style="padding-right: 20px;">

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
<BR>I wouldn't holler
<BR>If he were as __handsome__ as anything._

The lyrics define the specification: `(build==slender AND complexion==pale AND education==scholar AND wealth==rich) OR (looks==handsome)`. They want the Matchmaker to look through her list for a groom for those who match this specification. While not in the lyrics, should a new potential groom match the specification, then they would like to be notified as well.

## Collectors
The TV show American Pickers featured antique collectors Mike and Frank driving around rural American looking for trash that they could convert into treasures. 

The show focused upon the interesting characters they encountered, the items these people had collected and Mike and Frank negotiating a price.

These collections often look like a hoarder’s stash stored in a leaning barn about to topple over. Several times the guys would launch themselves onto unstable filth looking for that diamond in the rough. I’m sure they had to keep their tetanus shots up to date.

The treasures they’d find tended to be antique toys, car parts, motorcycle parts, old gas station signs, etc. The collectors often had detailed knowledge of anything pulled from the piles. They could often remember when and where they got something and how much they paid for it.

Quite frankly most of the items Mike and Frank gushed over just looked like junk to me. But they knew the specifications of their customers wanted and what they’d be willing to pay for them.

## Google Searches and Alerts, especially Jobs
Most online search engines are a form of Specification. Your search query is a specification, and the search engine will find and return pages that best match that specification. Google will allow you to create an alert for that specification query so that it will send you an email when it finds additional pages that match that specification.

Google’s Job UI/UX is even closer to Specification. Go to: [https://www.google.com/search?q=jobs](https://www.google.com/search?q=jobs) and click on the Jobs banner at the top.

This is a Google Jobs portal that will let anyone define a Job Specification based upon job title, location, full/part time, keywords, etc. The Specification can be fine tuned as much as needed, and Google will refresh the job list upon each update. Once you have a Job Specification you like, you can save it as an Alert so that Google will send you an email when it finds new jobs that match your Specification.

Each job searcher can customize his or her own Job Specification. And job searchers are not limited to just one Specification. They can create different Specifications with different criteria.

# Design Structure
Specification is an extension of Composite, but with a few enhancements. We have a few steps before we get there.

## Problem Description
We have a `Client` who is interested in a selection of `Context` objects based upon a `Specification` defined by the `Client`. The `Specification` is passed to a `ContextManager` which will return those `Context` objects that satisfy the `Specification`.

That description was awful. Let me match the Problem Description terms above with the language in the three examples above:

| Problem Description | Matchmaker | Antiques | Google Jobs |
| :-------------------------- | :--------------- | :------------ | :---------------- |
| Client | Young Woman | Customers back at the shop | A person looking for a job |
| ContextManager | Matchmaker  | Mike and Frank | Google |
| Context  | Husband Candidate  | Antique/Collector Item, which resides in leaning barns until found | An online Job opening |
| Specification | (build==slender AND complexion==pale AND education==scholar AND wealth==rich) OR (looks==handsome) | The type of items their customers are interested in | Job location, title, salary, etc. |

Here’s a UML class diagram for this behavior:
* At this level of abstraction, the diagram only shows the `Specification` interface. More `Specification` details will be provided shortly. `Specification` declares one method that will return a boolean indicating whether the `Context` satisfies the `Specification`.
* The `Client` has a reference to `ContextManager`, which was probably injected into it. If I had more space, I’d have shown an interface for the `ContextManager` and `Client` would have had a reference to the interface probably resolved by a `Configurer`.
* The `Client` acquires a `Specification` reference. More details to come soon.
* The `Client` calls `ContextManager`’s `getContextsBy(Specification specification)`, which will return the list of `Context` objects that satisfy the `Specification`. We can view `specification` as an argument being passed in, but I like to think of it as a form of [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html). `ContextManager` depends upon a `Specification` for each method call.
* `ContextManager` contains a List of `Context` objects. Its method iterates that List and adds a `Context` to the list of `satisfiedContexts` when a `Context` object satisfies the injected `Specification`. The `ContextManager` has no knowledge of or dependency upon the `Specification` implementation. Its only concern is interacting with the contract interface.

<img src="/assets/SpecificationOverview.png" alt="Specification Overview"  width = "80%" align="center" style="padding-right: 35px;">
 
This is the last time I will show details for `Client` and `ContextManager`. It’s too much to repeat in subsequent diagrams.

## Strategy
Let’s get some ideas for how we could implement `Specification`. Let’s start with the [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html).

We can do quite a bit with Strategy:
* The top row of elements is the same as the previous diagram except that the implementation details have not been shown.
* This diagram has added a little more context to `Context`. While only as examples, `Context` has a Color and Shape attribute. I thought this would be a bit more meaningful than AttributeA and AttributeB.
* We have three `Specifications` based upon these two attributes: Color, Shape and ColorAndShape. Their attributes are `final` so that the `Specification` cannot be modified after being created.

<img src="/assets/SpecificationStrategy.png" alt="Specification via Strategy" width = "80%" align="center" style="padding-right: 35px;">

Let’s see how the `Client` creates `Specification`s. Here are some examples:
```java
ContextManager contextManager; // injected possibly via a Configurer, not shown
…
List<Context> redContexts contextManager.getContextsBy(new ColorSpecification(Color.RED));
List<Context> circleContexts contextManager.getContextsBy(new ShapeSpecification(Shape.CIRCLE));
List<Context> blueSquareContexts contextmanager.getContextsBy(new ColorAndShapeSpecification(Color.BLUE), Shape.SQUARE));
```

## Composite
Strategy will work for simple specifications with one attribute, but the `ColorAndShapeSpecification` is a bit of a concern. It works fine, but it’s not scalable.
What if there are other attributes for `Context`? What about Color OR Shape? What about NOT? We don’t want to create a new Specification from scratch when we want a new combination of attributes. The combinations explode exponentially.

This is where [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) can be useful. This design maintains the previous design, except that it removes the `ColorAndShapeSpecification` and replaces it with three new composable Specifications. Each of these new `Specification`s is a composite. These `Specification` composites have their own specific behavior within the context of the Composite structure. Each of these Boolean operation based composites contain other `Specification`s where the composite return value is based upon the return values of the `Specification`s they contain such that:
* `AndSpecification` - Returns true when all its contained `Specifications` return true and false otherwise.
* `OrSpecification` - Returns true when any of its contained `Specification`s returns true and false otherwise.
* `NotSpecification` - Returns true when its single contained `Specification` returns false and false when it returns true.

Here is the design:
* It looks a little busy, but it’s not horrible. Consider each rectangle in isolation and its dependencies based upon its outward pointing arrows as was described in [Hexagonal Architecture – Why it works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html). This is not a Hexagonal Architecture design, but the same dependency management benefits still apply.
* Each class has limited knowledge of the overall design. Except for the `Client`, most only have knowledge of and depend upon one or two other classes or the interface.

<img src="/assets/SpecificationComposite1.png" alt="Specification via Composite" width = "80%" align="center" style="padding-right: 35px;">

### Specification Example via Composite
The previous `Client` `Specification` examples become:
```java
ContextManager contextManager; // injected possibly via a Configurer, not shown
…
List<Context> redContexts contextManager.getContextsBy(new ColorSpecification(Color.RED)); // Unchanged
List<Context> circleContexts contextManager.getContextsBy(new ShapeSpecification(Shape.CIRCLE)); // Unchanged

Specification blueSquareSpecification = new AndSpecification();
blueSquareSpecification.add(Color.BLUE);
blueSquareSpecification.add(Shape.SQUARE);
List<Context> blueSquareContexts contextmanager.getContextsBy(blueSquareSpecification); // Composed
```

Let’s examine `blueSquareSpecification`. It’s a _BLUE_ `ColorSpecification` and a _SQUARE_ `ShapeSpecification` which are then composed within an `AndSpecification`, such that a `Context` will be satisfied when it is both _BLUE_ and _SQUARE_.

The `AndSpecification` will iterate both. If either of them returns `false`, then it will return `false`. If both return `true`, then it will return `true`.

These three Boolean composite `Specification`s give us almost infinite possibilities in constructing a `Specification` without having to add new code, with the exception of new leaf `Specification`s.

### Specification Example via Showtunes
Here’s the composite `Specification` for the Matchmaker as mentioned in the example above:
```java
Matchmaker matchMaker; // injected possibly via a Configurer, not shown
…
Specification idealHusbandSpecification = new AndSpecification();
idealHusbandSpecification.add(new BuildSpecification(Slender));
idealHusbandSpecification.add(new ComplexionSpecification(Pale));
idealHusbandSpecification.add(new EducationSpecification(Scholar)); // For Papa
idealHusbandSpecification.add(new WealthSpecification(Rich)); // For Mama

Specification husbandSpecification = new OrSpecification();
husbandSpecification.add(idealHusbandSpecification);
husbandSpecification.add(new LooksSpecification(Handsome));

List<Husband> husbandCandidates matchMaker.getContextsBy(husbandSpecification);
```

The composite tree for the Matchmaker `Specification` would look like this:

<img src="/assets/SpecificationMatchmakerObjects.png" alt="Matchmaker Composite Tree" width = "80%" align="center" style="padding-right: 35px;">

* The class types are on the right of the colon.
* The object name is on the left of the colon.
* Anonymous objects don’t have names, so there’s nothing to the left of the colon.
* Attribute values reside under the object:class names.
* A candidate husband will be a match if he satisfies all the Slender/Pale/Scholar/Rich specifications or the single Husband specification.
 
While this violates the specification order in the lyrics, one might wish to reconfigure the specification to be more efficient:
* If the Husband Candidate is Handsome, then we don’t need to check the rest of the specification. Being Slender, Pale, Rich or a Scholar is just a bonus.
* If the Husband Candidate is not Handsome, then he must be Slender, Pale, Rich, and a Scholar. It’s less likely that he’s Rich or a Scholar than Slender and Pale. Therefore, as soon as a non-Handsome and non-Rich Husband Candidate is encountered, then we can eliminate him quickly without checking the other attributes.

<img src="/assets/SpecificationMatchmakerObjectsOptimal.png" alt="Matchmaker Composite Tree with Optimal Order" width = "80%" align="center" style="padding-right: 35px;">

## Composite Consolidation
This is more my personal style than standard practice, but there are two things that I don’t quite like about the design above:
* The `specifications` List in `SpecificationComposite` is protected. The derived classes have knowledge and depend upon it. It breaks encapsulation.
* The implementations in `AndSpecification` and `OrSpecification` are similar. They almost feel like common code, but not quite common code.

I have addressed both concerns via the [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) design pattern. The common code has been pulled up into the abstract `SpecificationComposite` class, but now it depends upon the new abstract `getSatisfactionBoolean()` method.

I have mixed emotions about doing this refactoring. On the one hand it consolidates some near-duplicate code. On the other hand, is it being too clever? Is it too obscure? I’m still on the fence. For what it’s worth, I have used this design technique in production code.

<img src="/assets/SpecificationComposite2.png" alt="Specification with Template Method added to Composite" width = "80%" align="center" style="padding-right: 35px;">
 
### My Final Comment, Or My Comment About Final
The leaf `Specification` classes contain attributes such as `Color` or `Shape`. I declared them as `final` so that they could not be changed. Once a `Specification` is defined, I don’t want to worry about it being updated in such a way that it exhibits different behaviors. If a new `Specification` behavior is needed, then a new `Specification` instance should be created.

I did the same with `NotSpecification`. The context is slightly different, but it’s still the same intent. I don’t want the ability to change a `NotSpecification` instance’s behavior by changing its `Specification` attribute.

The leaf `Specification` classes and `NotSpecification` attributes can be declared as `final` and initialized in their respective constructors.

However, we can’t easily use the same constructor technique with `AndSpecification` and `OrSpecification`. If we want the ability to add `Specification`s individually as I showed in the previous examples, then we have to allow objects of these classes to be updated via `add(Specification specification)` after they’ve been constructed.

There is a way to achieve finalness with a slightly different technique. I’m going to add a very simple 2-state state machine to `SpecificationComposite`:
* __Initializing__ – This is the start state representing when the `SpecificationComposite` is being initialized. When in this state, `add(Specification specification)` can add the `specification` to the `specifications` list.
* __Not Initializing__ – This is the state representing when the `SpecificationComposite` is no longer being initialized. When in this state, `add(Specification specification) will throw an InvalidStateException. There is no event transition from this state back to the Initializing state.
* `isSatisfied(Context context)` - This is the event that transitions from the Initializing state to the Not Initializing state.

Normally, I prefer a separate state machine class, but since this state machine is so simple and localized, I’m going to represent it via the `isInitializing` boolean attribute to keep track of the state and which actions are permitted based upon that boolean attribute state.

When an `AddSpecification` or `OrSpecification` is created, as many `Specification`s can be added to them as needed. But once they are queried with an `isSatisfied(Context context)` call, then they are no longer being initialized. No new `Specification`s can be added.

<img src="/assets/SpecificationFinal.png" alt="Specification with Finals" width = "50%" align="center" style="padding-right: 35px;">
 
The leaf `Specification`s are [Value Objects](https://en.wikipedia.org/wiki/Value_object). Once initialized, they cannot be modified. The `SpecificationComposite` objects are pseudo Value Objects. Once queried, they cannot be modified.

While I don’t have a formal proof, I’m near positive that this level of rigor gives you something else. I think it’s impossible to build a composite tree with circular references. I.e., I don’t think it’s possible for the same object to be in the tree more than once where it is a member of its own composite tree regardless of the levels of composition.

Having a circular reference will result in an infinite loop, and I’d rather make it impossible to configure a circular reference than depend upon those constructing the composite tree to ensure that they don’t even unintentionally. As I mentioned in several previous design patterns, I don’t trust the developers to do it correctly. I don’t trust myself either.

# Use Case
The Use Case is too large to include in this blog. It will be posted in the next blog.

# Specification Pros and Cons
The relative pros and cons of Specification are like those with most of the Composable design patterns.

## Pros

## Cons

# Summary
Specification is … thread safe usually.

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* TBD

Here are some resources that can be purchased or are included in a subscription service:
* TBD
