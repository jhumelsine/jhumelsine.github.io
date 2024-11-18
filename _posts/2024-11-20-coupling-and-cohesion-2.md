---
title: DRAFT – Coupling and Cohesion – Take 2
description: Let’s revisit these concepts one more time
unlisted: true
---

# Introduction
I received feedback on my most recent blog, [What Are Cohesion and Coupling?](https://jhumelsine.github.io/2024/11/07/cohesion-coupling.html) The comments mentioned historical references I had missed and questioned some of my content and examples.

I consumed the references provided by the those who had made comments and realized that I do need to make some updates. I don’t think my original blog is wrong, but it’s missing some nuance and context.

I originally planned to update the previous blog and call it a day, but I realized that these updates to the previous blog might change it too much. For example, I should have introduced _Coupling_ before _Cohesion_. Flipping them would too drastically change the original blog.

Therefore, I decided to create a new blog entry. Plus a new blog ensures that everyone knows about the updates.

# Coupling and Cohesion History
[Coupling](https://en.wikipedia.org/wiki/Coupling_(computer_programming)) and [Cohesion](https://en.wikipedia.org/wiki/Cohesion_(computer_science)) were invented by [Larry Constantine](https://en.wikipedia.org/wiki/Larry_Constantine) as part of [Structured Design](https://en.wikipedia.org/wiki/Structured_analysis#Structured_design). The terms gained popularity in [__Structured Design__](https://wstomv.win.tue.nl/quotes/structured-design.html) co-authored with [Ed Yourdon](https://en.wikipedia.org/wiki/Edward_Yourdon). [Chapter 6]( https://wstomv.win.tue.nl/quotes/structured-design.html#6) is devoted to _Coupling_ and [Chapter 7](https://wstomv.win.tue.nl/quotes/structured-design.html#7) is devoted to _Cohesion_.

[Meilir Page-Jones](https://www.dorsethouse.com/authors/page-jones.html) also contributed to the Coupling and Cohesion literature within [What Every Programmer Should Know About Object-Oriented Design](https://www.amazon.com/Every-Programmer-Should-Object-Oriented-Design/dp/0932633315), but I haven’t found many online references with details.

# Coupling and Cohesion Word Salad
__Coupling__ is dependency. __Cohesion__ is dependency as well, but more aggressively so. Coupling is an attribute of a bad design. Given that cohesion is even more aggressive coupling,  one would think that it's a bad thing, but no, cohesion is a good thing. 

Throwing in more terms, such as __loose/tight coupling__ and __low/high cohesion__, adds more confusion. Grouping __loose-coupling/high-cohesion__ together and __tight-coupling/low-cohesion__ together as paired opposites adds more fuel to the fire. Continue the befuddlement by throwing in multiple [__types__]( https://en.wikipedia.org/wiki/Coupling_(computer_programming)#Types_of_coupling) and [__dimensions__](https://en.wikipedia.org/wiki/Coupling_(computer_programming)#Types_of_coupling) of coupling along with several [__types__]( https://en.wikipedia.org/wiki/Cohesion_(computer_science)#Types_of_cohesion) of cohesion. Wrap it up with Meilir Page-Jones, who came up with a whole new term, [__Connascence__](https://en.wikipedia.org/wiki/Connascence), which is coupling in the context of Object-Oriented design.

__It’s no wonder developers get confused.__

# Coupling and Cohesion Definitions
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/0/09/CouplingVsCohesion.svg/800px-CouplingVsCohesion.svg.png" alt="Coupling vs Cohesion" title="Image Source: https://en.wikipedia.org/wiki/Coupling_(computer_programming)#/media/File:CouplingVsCohesion.svg " width = "45%" align="right" style="padding-right: 20px;">
 
According to the [Glossary](https://wstomv.win.tue.nl/quotes/structured-design.html#Glossary) provided by Constantine and Yourdon:
* __Coupling__ is a measure of the strength of interconnection between one module and another.
* __Cohesion__ is the degree of functional relatedness of processing elements within a single module.

The cubes in the diagram define a _module_, but I prefer _boundary_, which could be a method, class, package, service, etc. It depends upon context. The dots are elements within each boundary. The _Good_ illustration shows high cohesion among the elements in each boundary and relatively little, i.e., loose, coupling between the boundaries. The _Bad_ illustration also shows the low cohesion among elements within the boundaries and high coupling among the elements in the other boundary.

Coupling represents dependency and knowledge between boundaries. Cohesion represents dependency and knowledge within a boundary.

High cohesion means that all elements that are responsible for a behavior (or feature) reside within the boundary. Additionally, elements not responsible for a behavior do not reside within the boundary. Behavior modifications may cause updates in all cohesive elements, but those updates should be confined within the boundary.

Loose coupling means that dependency and knowledge among boundaries is limited. Abstractions, such as interfaces and APIs are boundary access ports that encapsulate internal elements within the boundary. Interfaces and APIs provide external access to behavior without requiring external coupling to internal elements.

Low cohesion and tight coupling render the boundaries meaningless. Elements have dependency upon and have knowledge of other elements anywhere. A modification anywhere can have an impact everywhere.

Tight coupling also leads to daisy chaining. An update to one element may require an update to a tightly coupled element, which may require an update to another tightly coupled element, etc. This daisy chained dependency can cause what at first appears to be a simple update to spread like wildfire as cascading updates throughout the system.

# Economics of Coupling and Cohesion
Why should we care about coupling and cohesion? If we never have to modify our code again, then we don’t care. But as we all know, code is rarely frozen. We almost always need to modify code as business needs change. The degree of coupling and cohesion will determine how complex the modification will be and how long it will take.

Kent Beck provides a great review of how coupling and cohesion affect the cost of modification in his [A Daily Practice of Empirical Software Design](https://www.youtube.com/watch?v=yBEcq23OgB4&t=901s) presentation at DDD Europe 2023.

Beck recounts how Constantine and Yourdon examined multiple programs and IBM and compared the ones that were cheap to change against those that were expensive to change. What was common among the cheap-to-change programs, and what was common among the expensive-to-change programs? Their analysis follows this logical conclusion:
* The cost of software is approximately equal to the cost of change.
* The cost of change is approximately equal to the cost of the major changes. That is, the cost of major changes compared to the cost of the minor changes dominates the cost of all changes. Therefore, we can ignore the cost of the minor changes.
* The cost of change is approximately equal to the proximity of cascading dependency in the change, which Constantine and Yourdon called _Coupling_.
* __Therefore, the cost of software is approximately equal to the cost of coupling. The more coupled the system is, the greater its cost. And this is why we care.__

We manage coupling in the system’s design so that we can more easily change it and do so at a reasonable cost.

Kent Beck mentioned Vlad Khononov’s presentation, [Balancing Coupling in Software Design](https://www.youtube.com/watch?v=KTy4rqgPOjg) from the same DDD conference. Khononov provided a review of the coupling, cohesion and connascence and their types and dimensions.

He included a [graph](https://www.youtube.com/watch?v=KTy4rqgPOjg&t=2217s), which inspired my own version to summarize the impact of change as the cost of coupling, cohesion and distance:
 
<img src="/assets/CouplingAndCohesionCost.png" alt="Coupling and Cohesion Cost" width = "80%" align="center" style="padding-right: 35px;">

This graph illustrates how coupling increases and cohesion decreases as the distance between elements expands. An update with a larger footprint will require more time, coordiation, buy-in and cost, than an update with a smaller footprint. For example, modifications scoped to a few statements only require the resources of one or two developers. Modifications scoped to systems require multiple teams and possibly multiple companies.

# Coupling and Cohesion Examples
I struggled to describe coupling and cohesion in my previous [blog](https://jhumelsine.github.io/2024/11/07/cohesion-coupling.html). I used the [screw/screwdriver](https://jhumelsine.github.io/2024/11/07/cohesion-coupling.html#cohesion) example to describe high cohesion, but it’s coupling too. I guess it depends upon how easily you can find a matching screwdriver when you need it. And while I initially liked my [Reese’s Peanut Butter Cup](https://jhumelsine.github.io/2024/11/07/cohesion-coupling.html#coupling) example, I’m not sure what I was trying to convey. Chocolate and peanut butter definitely have separate identities, which implies coupling, but darn it, when combined in a Reese’s Cup, they sure feel like a cohesive unit.

<img src="https://media.freemalaysiatoday.com/wp-content/uploads/2024/03/4d813c58-box.jpg" alt="Meal Kit" title="Image Source: https://www.freemalaysiatoday.com/category/leisure/2024/03/02/meals-in-minutes-cooking-made-simple-for-the-busy-malaysian/" width = "35%" align="right" style="padding-right: 20px;">
 
I think I have another real-world example high cohesion and low coupling: meal kits. Each meal kit has all the ingredients required to make the meal without any extraneous ingredients. You don’t have to purchase different meal kits to make a meal either. If a meal is updated, it only affects that meal’s kit.

# Contracts
We cannot avoid coupling, but we want to keep it limited and loose. How can we manage that?

A __contract__ declares behavior without defining how that behavior will be implemented. It should also declare the expectations and obligations of both the supplier and consumer of that behavior. Interfaces and APIs are types of contracts.

Many modern software practices, such as [Design Patterns](https://jhumelsine.github.io/2023/08/24/its-your-move.html), [Hexagonal Architecture/Ports & Adapters](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html), focus upon contracts. The first [Design Pattern Principle](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html) addresses this with: __Program to an interface, not an implementation__.

# Context Coupling
We can still have coupling with a good design with good contracts. Contracts may be [Leaky Abstractions](https://en.wikipedia.org/wiki/Leaky_abstraction). [Joel Spolsky](https://en.wikipedia.org/wiki/Joel_Spolsky) pointed this out in his blog post, [The Law of Leaky Abstractions](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/) when he wrote: __All non-trivial abstractions, to some degree, are leaky.__

I didn’t fully appreciate leaky abstractions until considering this blog addendum. I had only considered leaking abstracts in the context that it leaked implementation details.

We don’t want to leak any implementation details in a contract, but most contracts will contain context, and both the consumer and supplier of the contract depend upon and have knowledge of that context. If that context changes, and it may need to for business reasons, then the consumer and supplier may both need to change as well.

As an example, a contract may include the definition of Person, who has a name, address, phone number, email address, etc. Later, possibly much later, there’s a business need to update a Person to include a cellphone number or an additional email address.

Person was probably a concept that was incorporated in many parts of the system. It had the business invariant that the Person only had a landline and one email address. Many parts of the system have probably already made decisions based upon this definition of a Person. Adding a cell phone or additional email address will have an impact upon all of them.

# Summary
Hopefully this second take at _Coupling_ and _Cohesion_ has provided a bit more clarity and definitely not introduced any confusion by me.

The more I considered this addendum, the more I kept thinking about a previous blog from last year, [Hexagonal Architecture – Why it works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html), which is about dependency and knowledge management.

_Coupling_ and _Cohesion_ are inverse measures of _Dependency_. _Coupling_ is _Dependency_ at great distances forcing components in a system and possibly systems to maintain lock-step coordination during modifications. _Cohesion_ is _Dependency_ at near distances allowing differnt components and systems to be modified independently.

Several months ago, I mentioned an elusive grand unified theory of software engineering in [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html):
>I feel there may be a grand unified theory of software engineering that’s still just a bit beyond my grasp. If there is such a grand unified theory, I’d be willing to bet that Dependency and Knowledge Management is part of it.

I now feel that _Coupling_, _Cohesion_ and _Dependency_/_Knowledge_ Management are different facets of the same basic principle.

# References
[Previous References]( https://jhumelsine.github.io/2024/10/30/abstraction.html#references)

* [A Daily Practice of Empirical Software Design ](https://www.youtube.com/watch?v=yBEcq23OgB4) - Presentation by Kent Beck at DDD Europe 2023
* [Balancing Coupling in Software Design](https://www.youtube.com/watch?v=KTy4rqgPOjg) - Presentation by Vlad Khononov at DDD Europe 2023
* [Coupling Is The Biggest Challenge In Software Engineering](https://www.youtube.com/watch?v=plMttQWztRM) - Video by Dave Farley
* [Uncoupling](https://www.youtube.com/watch?v=esm-1QXtA2Q) = Presentation by Michael Nygard at GOTO 2018
