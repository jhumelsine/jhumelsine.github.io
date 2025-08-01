---
title: Coupling and Cohesion – Take 2
description: Let’s revisit these concepts one more time
---

# Introduction
I received feedback on my most recent blog, [What Are Cohesion and Coupling?](https://jhumelsine.github.io/2024/11/07/cohesion-coupling.html) The comments mentioned historical references I had missed and questioned some of my content and examples.

I consumed the references provided by those who had made comments and realized that I do need to make some updates. I don’t think my original blog is wrong, but it’s missing some nuance and context.

I originally planned to update the previous blog and call it a day, but I realized that these updates to the previous blog might change too much. For example, I should have introduced _Coupling_ before _Cohesion_. Flipping them would too drastically change the original blog.

Therefore, I decided to create a new blog entry. Plus, a new blog ensures that everyone knows about the follow up.

# Coupling and Cohesion History
[Coupling](https://en.wikipedia.org/wiki/Coupling_(computer_programming)) and [Cohesion](https://en.wikipedia.org/wiki/Cohesion_(computer_science)) were invented by [Larry Constantine](https://en.wikipedia.org/wiki/Larry_Constantine) as part of [Structured Design](https://en.wikipedia.org/wiki/Structured_analysis#Structured_design). The terms gained popularity in the [__Structured Design__](https://wstomv.win.tue.nl/quotes/structured-design.html) book co-authored with [Ed Yourdon](https://en.wikipedia.org/wiki/Edward_Yourdon). [Chapter 6]( https://wstomv.win.tue.nl/quotes/structured-design.html#6) is devoted to _Coupling_ and [Chapter 7](https://wstomv.win.tue.nl/quotes/structured-design.html#7) is devoted to _Cohesion_.

[Meilir Page-Jones](https://www.dorsethouse.com/authors/page-jones.html) also contributed to the Coupling and Cohesion literature in [What Every Programmer Should Know About Object-Oriented Design](https://www.amazon.com/Every-Programmer-Should-Object-Oriented-Design/dp/0932633315), but I haven’t found many online references with details.

# Coupling and Cohesion Word Salad
__Coupling__ is dependency. __Cohesion__ is dependency as well, but more aggressively so when all the elements depend upon one another like a team. Coupling is an attribute of a bad design. Given that cohesion is even more aggressive coupling,  one would think that it's an attribute of a bad design, but no, cohesion is an attribute of a good design. 

There's more confusion when throwing in more terms, such as __loose/tight coupling__ and __low/high cohesion__. Grouping __loose-coupling/high-cohesion__ together and __tight-coupling/low-cohesion__ together as paired opposites adds more fuel to the fire. Continue the befuddlement by throwing in multiple [__types__](https://en.wikipedia.org/wiki/Coupling_(computer_programming)#Types_of_coupling) and [__dimensions__](https://en.wikipedia.org/wiki/Coupling_(computer_programming)#Types_of_coupling) of coupling along with several [__types__]( https://en.wikipedia.org/wiki/Cohesion_(computer_science)#Types_of_cohesion) of cohesion. Meilir Page-Jones put a bow on the confusion package when he defined  a whole new term, [__Connascence__](https://en.wikipedia.org/wiki/Connascence), which is coupling in the context of Object-Oriented design.

__It’s no wonder developers get confused.__

# Coupling and Cohesion Definitions
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/09/CouplingVsCohesion.svg/800px-CouplingVsCohesion.svg.png" alt="Coupling vs Cohesion" title="Image Source: https://en.wikipedia.org/wiki/Coupling_(computer_programming)#/media/File:CouplingVsCohesion.svg" width = "45%" align="right" style="padding-right: 20px;">
 
According to the [Glossary](https://wstomv.win.tue.nl/quotes/structured-design.html#Glossary) provided by Constantine and Yourdon:
* __Coupling__ is a measure of the strength of interconnection between one module and another.
* __Cohesion__ is the degree of functional relatedness of processing elements within a single module.

Their definition and the corresponding diagram to the right represent _Coupling_ and _Cohesion_ between two sets of modules. _Coupling_ represents the number of connections between elements between two modules. _Cohesion_ refers to the connections within a module.

These two concepts are not mutually exclusive. Constantine and Yourdon also wrote 
>Clearly, cohesion and coupling are interrelated. The greater the cohesion of individual modules in the system, the lowering the coupling between modules will be. In practice, these measures are correlated; that is, on average, as one increases, the other decreases, but the correlation is not perfect. Maximizing the sum of module cohesion over all modules in a system should closely approximate the results one would obtain in trying to minimize coupling. However, it turns out to be easier both mathematically and practically to focus on cohesion.

# Coupling and Cohesion - Possibly Two Sides of the Same Coin
I think there's more to Coupling and Cohesion than their interrelatedness. __Coupling and Cohesion represent dependency and knowledge at a distance.__ Coupling represents dependency upon and knowledge of elements at greater distances, i.e., dependency and knowledge beyond the module. Cohesion represents dependency of and knowledge of elements at lesser distances, i.e., dependency and knowledge within the module. Coupling and Cohesion could be thought of describing the same concept but with two opposing terms.

The original definition of Coupling and Cohesion applied to connectivity being inside or outside a module, but I think the principle expands on a spectrum beyond connectivity that is more granular than the module.

Thinking more abstractly, the cubes in the diagram above can be thought of as any _boundary_, which can represent expanding borders, such as: a statement block, method, class, package, service, etc. It depends upon context. Boundaries representing different layers of borders can be nested.

The dots are elements within each boundary. The _Good_ illustration shows high cohesion among the elements in each boundary and relatively little, i.e., loose, coupling between the boundaries. The _Bad_ illustration also shows low cohesion among elements within the boundaries and high coupling among elements in the other boundary.

__Coupling represents dependency and knowledge between boundaries. Cohesion represents dependency and knowledge within a boundary.__

Coupling and Cohesion are on a boundary spectrum. That's why __loose-coupling/high-cohesion__ and __tight-coupling/low-cohesion__ tend to be paired together. They represent the same basic concept of dependency and knowledge at a distance relative to a boundary.

__High Cohesion__ means that most elements that are responsible for a behavior (or feature) reside within a boundary. Elements not responsible for a behavior do not reside within the boundary. Behavior modifications may cause updates to all cohesive elements within the boundary. You or your team should have complete responsibility for the boundary meaning there's little to no coordination with others.

__Loose Coupling__ means that dependency and knowledge among boundaries is limited. Abstractions, such as interfaces and APIs are boundary access ports that encapsulate internal elements within the other boundaries. Interfaces and APIs provide access to behavior provided by other boundaries without requiring external coupling to internal elements of those other boundaries.

<img src="https://codeopinion.com/wp-content/uploads/2021/12/2-1-768x655.png" alt="Big Ball of Mud" title="Image Source: https://codeopinion.com/long-live-the-monolith-monolithic-architecture-big-ball-of-mud/" width = "35%" align="right" style="padding-right: 20px;">

__Low Cohesion__ and __Tight Coupling__ render the boundaries meaningless. Elements have dependency upon and knowledge of other elements anywhere. A modification anywhere can have an impact everywhere. You and your team may have limited responsibility for many of the impacted elements requiring coordination with others. Additionally, updates made by others may impact your team's elements as well. A design like this is often called a [Big Ball of Mud](https://blog.codinghorror.com/the-big-ball-of-mud-and-other-architectural-disasters/).

__Tight Coupling__ also leads to daisy chained dependencies. An update to one element may require an update to a tightly coupled element, which may require an update to another tightly coupled element, etc. This daisy chained dependency can trigger what at first appears to be a simple update to spread like wildfire as cascading updates spread throughout the system.

With this more fluid interpretation of Coupling and Cohesion, we could have a system with __loose-coupling/high-cohesion__ on the macro design level and __tight-coupling/low-cohesion__ on the micro design level. That is, there may be low coupling between packages and high cohesion within a package, but the design inside the package could have tight coupling and low cohesion among the classes and methods within the package.

# Economics of Coupling and Cohesion
Why should we care about Coupling and Cohesion? If we never have to modify our code again, then we don’t care. But as we all know, code is rarely frozen. We almost always need to modify code as business needs change. The degree of Coupling and Cohesion will determine how complex the modification will be and how long it will take.

Kent Beck provides a great review of how coupling and cohesion affect the cost of modification in his [A Daily Practice of Empirical Software Design](https://www.youtube.com/watch?v=yBEcq23OgB4&t=901s) presentation at DDD Europe 2023.

Beck recounts how Constantine and Yourdon examined multiple programs at IBM and compared the ones that were cheap to change against those that were expensive to change. What was common among the cheap-to-change programs, and what was common among the expensive-to-change programs? Their analysis follows this logical conclusion:
* The cost of software is approximately equal to the cost of change.
* The cost of change is approximately equal to the cost of the major changes. That is, the cost of major changes compared to the cost of the minor changes dominates the cost of all changes. Therefore, we can ignore the cost of the minor changes.
* The cost of change is approximately equal to the proximity of cascading dependency in the change, which Constantine and Yourdon called _Coupling_.
* __Therefore, the cost of software is approximately equal to the cost of coupling. The more coupled the system is, the greater its cost. And this is why we care.__

We manage coupling in the system’s design so that we can more easily change it and do so at a reasonable cost.

Kent Beck mentioned Vlad Khononov’s presentation, [Balancing Coupling in Software Design](https://www.youtube.com/watch?v=KTy4rqgPOjg) from the same DDD conference. Khononov provided a review of the coupling, cohesion and connascence and their types and dimensions.

He included a [graph](https://www.youtube.com/watch?v=KTy4rqgPOjg&t=2217s), which inspired my own version to summarize the impact of change as the cost of coupling, cohesion and distance:
 
<img src="/assets/CouplingAndCohesionCost.png" alt="Coupling and Cohesion Cost" width = "80%" align="center" style="padding-right: 35px;">

This graph illustrates how coupling increases and cohesion decreases as the distance between elements expands. An update with a larger boundary footprint will require more time, coordination, buy-in and cost, than an update with a smaller footprint. For example, modifications scoped to a few statements only require the resources of one or two developers. Modifications scoped to systems require multiple teams and possibly multiple companies.

# Coupling and Cohesion Examples
I struggled to provide good coupling and cohesion examples in my previous [blog](https://jhumelsine.github.io/2024/11/07/cohesion-coupling.html). I'll make another attempt here.

## Real World Examples
I used the [screw/screwdriver](https://jhumelsine.github.io/2024/11/07/cohesion-coupling.html#cohesion) example to describe high cohesion, but it’s coupling too. I guess it depends upon how easily you can find a matching screwdriver when you need it. And while I initially liked my [Reese’s Peanut Butter Cup](https://jhumelsine.github.io/2024/11/07/cohesion-coupling.html#coupling) example, I’m not sure what I was trying to convey. Chocolate and peanut butter definitely have separate identities, which implies coupling, but darn it, when combined in a Reese’s Cup, they sure feel like a cohesive unit.

<img src="https://media.freemalaysiatoday.com/wp-content/uploads/2024/03/4d813c58-box.jpg" alt="Meal Kit" title="Image Source: https://www.freemalaysiatoday.com/category/leisure/2024/03/02/meals-in-minutes-cooking-made-simple-for-the-busy-malaysian/" width = "35%" align="right" style="padding-right: 20px;">
 
I think I have another real-world example high cohesion and low coupling: meal kits. Each meal kit has all the ingredients required to make the meal without any extraneous ingredients. You don’t have to purchase different meal kits to make a meal either. If a meal is updated, it only affects that meal’s kit.

## Software Design Examples
Let's consider coupling and cohesion with some real software design examples. I shall return to some examples from previous blogs.

### State Machine
I briefly mentioned __State Machines__ in the previous blog in the [Low Coupling](https://jhumelsine.github.io/2024/11/07/cohesion-coupling.html#low-cohesion) section.

My concern was the distribution of state machine behavior in an application. I've seen too many cases of state machine implementations where `state` is a private attribute accessible via public `get` and `set` accessors. State machine behavior was implemented throughout the application by any code that acquired, changed and saved `state` via the accessors.

Though not obvious at first, this is a highly coupled design. All code that references the accessors indirectly depend upon one another since they are all taking some responsibility for state machine behavior. A behavior change in one section of the code could change an assumption about the state machine in another section of code. Almost as bad, if not worse, no one knows what the actual state machine behavior is, since it's distributed in so many different places.

I would prefer an implementation that keeps state machine behavior cohesive, possibly via the [State Design Pattern](https://refactoring.guru/design-patterns/state). This consolidates the state machine behavior. The state machine could still provide a `get` accessor for its `state`, but it should not provide a `set` accessor. The state machine would define public methods for each `event` that would trigger an internal transition and the implementation to determine the new `state` would occur in the `event` method within the state machine class. The `state` implementation could be enhanced with the [Observer Design Pattern](https://refactoring.guru/design-patterns/observer) so that the applications could subscribe to future `state` change notifications.

I suspect that the coupled design occurs when developers don't realize that there's state machine behavior lurking in their application. If they don't design a cohesive state machine, the design will have a tendence to be distributed.

### Wrong Abstraction
My [Getting the Right Abstraction is Hard](https://jhumelsine.github.io/2023/09/22/right-abstraction-is-hard.html) blog entry is also about coupling and cohesion. In this blog, I described an implementation I inherited. The implementation featured a base class with several extending classes. As I examined the code to understand it, I realized that some behavior implementations were in the wrong places. Some behavior implemented in the base class needed to be moved into the extending classes and some behavior implemented in the extended classes needed to be consolidated in the base class.

In its original design, a change to behavior associated with an extended class, but placed in the base class, could impact the base class as well, which could impact the other extended classes.

Once I redesigned the code, using the [Template Method Design Pattern](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html), the design was less coupled. A change to an extended class would not affect the base class or the other extended classes.

# Contracts
We cannot avoid coupling, in fact we need it. But we want to keep coupling loose. How can we manage that?

A [__contract__](https://jhumelsine.github.io/2025/06/10/contracts.html) declares behavior without defining how that behavior will be implemented. It should also declare the expectations and obligations of both the supplier and consumer of that behavior. Interfaces and APIs are types of contracts.

Many modern software practices, such as [Design Patterns](https://jhumelsine.github.io/2023/08/24/its-your-move.html), [Hexagonal Architecture/Ports & Adapters](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html), focus upon contracts. The first [Design Pattern Principle](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html) addresses this with: __Program to an interface, not an implementation__.

# Context Coupling
There are multiple [__types__](https://en.wikipedia.org/wiki/Coupling_(computer_programming)#Types_of_coupling) and [__dimensions__](https://en.wikipedia.org/wiki/Coupling_(computer_programming)#Types_of_coupling) of coupling along with several [__types__]( https://en.wikipedia.org/wiki/Cohesion_(computer_science)#Types_of_cohesion) of cohesion. Follow the links for definitions or watch some of the presentations in the [References](#references) below which describe some of them as well.

I'll provide a scenario of one, which I think is technically __External Coupling__, based upon the Wikipedia definition:
>External coupling occurs when two modules share an externally imposed data format, communication protocol, or device interface. This is basically related to the communication to external tools and devices.

My scenario fits the spirit of external coupling, but it's not one of the listed cases. In my scenario elements may be coupled to the schema of another element, and then that schema changes. That is, the coupling could be based upon context.

We can still have coupling with a good design with good contracts. Contracts may be [Leaky Abstractions](https://en.wikipedia.org/wiki/Leaky_abstraction). [Joel Spolsky](https://en.wikipedia.org/wiki/Joel_Spolsky) pointed this out in his blog post, [The Law of Leaky Abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/) when he wrote: __All non-trivial abstractions, to some degree, are leaky.__

I didn’t fully appreciate leaky abstractions until writing this blog entry. I had only considered leaking abstracts in the context that implementation details could be leaked in a contract.

We don’t want to leak implementation details in a contract, but most contracts will contain context, and both the consumer and supplier of the contract depend upon and have knowledge of that context. If that context changes, and it may need to change for business reasons, then the consumer and supplier may both need to change as well.

As an example, a contract may include the definition of Person, with a name, address, landline phone number, email address, etc. Later, possibly much later, there’s a business need to update a Person to include a cellphone number or an additional email address.

The original Person attributes, landline phone and only one email, may be a domain invariant that's been incorporated in many parts of the system. Adding a cell phone or additional email address will have an impact upon all of them since the domain invariant has been updated.

Context coupling relates to the [Stable or Fixed Design Elements](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#stable-or-fixed-design-elements) section in my [Hexagonal Architecture – Why it works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html) blog entry from last year. Stable elements, such as interfaces, don't tend to have dependencies upon other elements. However other elements have dependencies upon them. This is visually apparent with a design rendered in a UML class diagram. Stable elements will tend to have the arrowheads on relationship lines pointing into them and very view pointing out of them.

In the example, other elements depend upon Person by referencing it, but Person doesn't depend upon them. Therefore, when Person is updated, this can have a significant impact upon the elements that reference it.

# Summary
Hopefully this second take at _Coupling_ and _Cohesion_ has provided a bit more clarity and not introduced any confusion by me.

The more I considered this addendum, the more I kept thinking about a previous blog, as mentioned above: [Hexagonal Architecture – Why it works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html), which is about dependency and knowledge management.

_Coupling_ and _Cohesion_ are inverse measures of _Dependency_. _Coupling_ is _Dependency_ at great distances that forces components in a system and possibly external systems to maintain lock-step coordination during modifications. _Cohesion_ is _Dependency_ at near distances that allows different components and systems to be modified independently.

Several months ago, I mentioned an elusive grand unified theory of software engineering in [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html):
>I feel there may be a grand unified theory of software engineering that’s still just a bit beyond my grasp. If there is such a grand unified theory, I’d be willing to bet that Dependency and Knowledge Management is part of it.

I now feel that _Coupling_, _Cohesion_ and _Dependency_/_Knowledge_ Management are different facets of the same basic principle.

# References
* [Previous References]( https://jhumelsine.github.io/2024/10/30/abstraction.html#references)
* [A Daily Practice of Empirical Software Design ](https://www.youtube.com/watch?v=yBEcq23OgB4) - Video presentation by Kent Beck at DDD Europe 2023
* [Balancing Coupling in Software Design](https://www.youtube.com/watch?v=KTy4rqgPOjg) - Video presentation by Vlad Khononov at DDD Europe 2023
* [Uncoupling](https://www.youtube.com/watch?v=esm-1QXtA2Q) - Video presentation by Michael Nygard at GOTO 2018
* [Coupling Is The Biggest Challenge In Software Engineering](https://www.youtube.com/watch?v=plMttQWztRM) - Video by Dave Farley in 2024
* [Good Coupling, Bad Coupling, Coupling For The Win](https://www.youtube.com/watch?v=ZffqJxLULx8) - Video by Vaughn Vernon in 2024.
* [Software Design Glossary](https://web.archive.org/web/20150730203016/https://www.facebook.com/notes/facebook-engineering/software-design-glossary/10150309412413920) - by Kent Beck while working at Facebook
