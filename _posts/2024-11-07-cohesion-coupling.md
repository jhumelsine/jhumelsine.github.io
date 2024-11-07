---
title: DRAFT – What Are Cohesion and Coupling?
description: Cohesion and coupling are about when things should and should not be too sticky with one another.
unlisted: true
---

# Introduction
This blog was originally going to be a continuation of the previous [Abstract](https://jhumelsine.github.io/2024/10/30/abstraction.html) blog where I planned to follow up with _Cohesive Abstractions_. 

I was close to publishing it, but I realized that I was devoting almost 1,000 words to _Cohesion_ and _Coupling_, which started to feel like its own separate blog. I'm making a last minute decision to blog exclusively on _Cohesion_ and _Coupling_ first, and then follow up with _Cohesive Abstractions_ shortly thereafter.

# Cohesion and Coupling
_Cohesion_ and _Coupling_ both deal with how software elements are connected together. The former case is about elements that should be connected but often aren't connected. The latter case is about elements that should not be connected but often are connected.

## Cohesion
It took me a while before I had a solid understanding of [Cohesion](https://en.wikipedia.org/wiki/Cohesion_(computer_science)).

Elements are cohesive when there’s an intrinsic relationship that connects them. The relationship is often gestalt in that the elements only have meaningful utility when combined as a whole. When an update is needed, most or all elements in the cohesive relationship will require updating.

<img src="https://i0.pickpik.com/photos/475/455/659/screwdrivers-screws-red-black-preview.jpg" alt="Screw and Screwdriver" title="Image Source: https://www.pickpik.com/screwdrivers-screws-red-black-tools-work-6612" width = "35%" align="left" style="padding-right: 20px;">
 
The intrinsic relationship between __screw__ and __screwdriver__ is an example. Neither is functional without the other. The screwdriver must also match the type of screw. For example, a flathead screwdriver won’t work on a Phillips head screw. If the design changes from a flathead screw to a Phillips head screw, then the screwdriver will need to change from a flathead to a Phillips head likewise.

<img src="https://live.staticflickr.com/4102/4780764147_ca8650f6c4_b.jpg" alt="Nuts and Bolts" title="Image Source: https://www.flickr.com/photos/tudedude/4780764147" width = "35%" align="right" style="padding-right: 20px;">
 
__Nuts__ and __bolts__ are cohesive. They only function when screwed together, and when their sizes match.

We want our designs to have high cohesion. That is, software elements with intrinsic relationships should be near one another in the design.

Low cohesion occurs when related software elements have been distributed across the design. For example, you’re enhancing an existing behavior, and it requires changes to a dozen files distributed throughout the design. Additionally, the files being updated also contain code that’s not directly related to the behavior that you’re updating.

Low cohesion has at least two concerns:
* Did you update all the files that need to be updated?
* What if your update breaks the other content that’s not directly related to the behavior you’re updating?

Another common example of low cohesion I’ve seen is __state machine__ behavior (blog TBD) spread across the implementation.
This usually happens when a class has a `private` attribute named something like `status` or `state`.
<img src="https://upload.wikimedia.org/wikipedia/commons/f/fd/State_Machine.jpg?20090820151828" alt="State Machine Diagram" title="Image Source: https://commons.wikimedia.org/wiki/File:State_Machine.jpg" width = "35%" align="left" style="padding-right: 20px;">
Its type is often an `enum`, but I've also seen it as a `boolean` and even a `String`. Its implied privacy is violated with `get` and `set` accessors, which provide direct access to the `private` attribute.

<img src="https://live.staticflickr.com/2605/3937095282_ab2e98471d_b.jpg" alt="The Lost Symbol by Dan Brown" title="Image Source: https://www.flickr.com/photos/billstrain/3937095282" width = "25%" align="right" style="padding-right: 20px;">

Rather than keeping the state machine behavior encapsulated within the class, the class becomes the place where `state` or `status` reside. Other classes will access `status`/`state` via the `get` accessor, update `status`/`state` with a new value via the `set` accessor based upon their own business logic code. Not only does this low cohesive design distribute the __state machine__ implementation across far flung regions of the design, it distributes the ability to understand the behavior. It's like being Robert Langdon in one of Dan Brown's novels running around despirately looking for clues to see the bigger picture. This makes it more difficult to know what the __state machine__ will do as a whole when the `status`/`state` is updated from any place in the implementation.

With cohesive software elements, a change to one element may require a change to the other related cohesive elements, like how a change in screw head causing a change in the screwdriver. Highly cohesive software elements are close to one another, such as in the same package, making it more likely that all required updates will occur consistently.

I consider the methods of an `interface` cohesive when:
* The methods are functionally related and support each other.
* The removal of one of the methods diminishes the whole.

Traditional [Create, Read, Update and Delete](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (CRUD) operations are cohesive in my mind. Remove any of them, and the remaining operations are incomplete. The requirements for one of my features contained Create, Update and Delete requirements for a set of domain elements, but there were no Read requirements. I asked my requirements creator why I was implementing features to manage data when there were no requirements to read the data. _Oops._

A cohesive interface tends to follow the [Interface Segregation Principle](https://en.wikipedia.org/wiki/Interface_segregation_principle) (ISP). An interface that violates ISP contains methods that can be separated into their own cohesive interfaces.

## Coupling
[Coupling](https://en.wikipedia.org/wiki/Coupling_(computer_programming)) is often uttered in the same breath with _cohesion_. It took me a while to gain a solid understanding of coupling, especially when used with cohesion. They seemed similar yet different, and I wasn’t sure how to distinguish them.

Coupling is also about connected elements. However, unlike cohesive elements, coupled elements do not have intrinsic relationships.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/2021-05-15_14_56_22_A_Reese%27s_Peanut_Butter_Cup_broken_into_two_pieces_in_the_Franklin_Farm_section_of_Oak_Hill%2C_Fairfax_County%2C_Virginia.jpg/1200px-2021-05-15_14_56_22_A_Reese%27s_Peanut_Butter_Cup_broken_into_two_pieces_in_the_Franklin_Farm_section_of_Oak_Hill%2C_Fairfax_County%2C_Virginia.jpg?20210515225457" alt="Reece's Cup" title="Image Source: https://commons.wikimedia.org/wiki/File:2021-05-15_14_56_22_A_Reese%27s_Peanut_Butter_Cup_broken_into_two_pieces_in_the_Franklin_Farm_section_of_Oak_Hill,_Fairfax_County,_Virginia.jpg" width = "40%" align="right" style="padding-right: 20px;">
 
A Reese’s Peanut Butter Cup _couples_ chocolate and peanut butter together. I know for many, a peanut butter cup would be an excellent example of cohesion, but I'm the odd duck who doesn't like peanut butter cups, even though I like chocolate and peanut butter separately.

We want our designs to have loose coupling. That is, we don’t want unrelated elements to be stuck together too tightly.

A Reese’s Cup would be tightly coupled if it were the only form from which we could get chocolate or peanut butter. Imagine scraping the insides from a Reese’s Cup as the only means to make a peanut butter sandwich.

Tight coupling occurs when disparate software concepts are stuck together. For example, the communication framework, business logic and persistence implementations are intertwined in the same method. A change to one of these elements runs a risk of affecting or breaking one of the other non-related coupled elements. A tightly coupled method is often a violation of the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single-responsibility_principle), since there's more than reason for it to change. Reusing the business logic in another context becomes impossible with it's tightly couplied with other elements.

Many modern software practices, such as [Design Patterns](https://jhumelsine.github.io/2023/08/24/its-your-move.html), [Hexagonal Architecture/Ports & Adapters](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html), and others address tight coupling. They promote loose coupling. The first [Design Pattern Principle](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html) addresses this, even if it doesn’t mention coupling directly: __Program to an interface, not an implementation__.

## Coupling and Cohesion
Tight coupling and low cohesion, as described above, are undesirable design traits.

A good design features __loose coupling__ of software elements that don’t change together and __high cohesion__ for those software elements that do change together.

# Summary
TBD

# References
TBD
