---
title: Hexagonal Architecture – An Introduction
description: Despite its name, it really isn’t about hexagons.
---
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/7/78/Hexagon_windows_%28Unsplash%29.jpg/1185px-Hexagon_windows_%28Unsplash%29.jpg?20170903070532" alt="Building with Hexagons" title="Image Source: [https://pixabay.com/photos/hexagon-building-pattern-abstract-6233333/](https://commons.wikimedia.org/wiki/File:Hexagon_windows_%28Unsplash%29.jpg)" width = "70%" align="center" style="padding-right: 35px;">

# Introduction to Hexagonal Architecture
I’m introducing a new blog series about _Hexagonal Architecture_ (also known as _Ports and Adapters_). My previous blog posts have featured specific design patterns. This series is, in a way, continuing the design pattern theme. But instead of focusing upon specific design patterns, it will show how we can apply the design patterns that have already been presented. I will return to more design patterns once this _Hexagonal Architecture_ series has been completed.

The internet is swarming with information about _Hexagonal Architecture/Ports & Adapters_ (HexArch/P&A). I don’t want to replicate content that others have already provided. I’ll try to focus upon what I find interesting about HexArch/P&A that may not get as much coverage.

The series continues with:
* [Hexagonal Architecture - Structure](https://jhumelsine.github.io/2023/10/28/hexagonal-architecture-structure.html) - This describes the structure of the Hexagonal Architecture design, and how it's really a pattern or design patterns.
* [Hexagonal Architecture - Why it works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html) - This describes why I feel that the Hexagonal Architecture design works so well. It's based upon dependency and knowledge management.
* [Hexagonal Architecture - How it compares and contrasts with Clean Architecture](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html) - The two designs are mostly the same, but there are some differences worth pointing out.
* [Hexagonal Architecture - Adapter Flexibility](https://jhumelsine.github.io/2023/11/30/hexagonal-architecture-adapter-flexibility.html) - We have quite a bit of flexibility with this design in the Adapter layer.
* _NOTE: The Hexagonal Architecture series will be on hiatus. As of the time of this update, Advent of Code 2023 is about to start, and I want to move onto several other topics. I'll return back to Hexagonal Architecture, but it will probably be a few months._

# What is a Name?
I first heard of _Hexagonal Architecture_ about 6 or 7 years ago. The name sounded funny. Later I heard that it’s also called _Ports and Adapters_. The same design has two names. _Sigh_. But that’s not all. The design basics are extremely similar to other designs, such as _Clean Architecture_ (CleanArch) and _Onion Architecture_ (OnionArch).

Don’t be overly concerned about the different names. It’s their shared core concepts that are important.

# HexArch/P&A History
The HexArch design was conceived by [Alistair Cockburn](https://en.wikipedia.org/wiki/Alistair_Cockburn) in the 1990s to decouple software components. He chose hexagons in his diagrams since he wanted to avoid rectangles. Rectangles tend to have preconceived notions in diagrams such as:
* They’re classes or objects
* They’re part of a layered architecture
* They have a fixed number of access points

He wanted to convey something different. His design focuses upon design boundary regions and their constraints. 

The 6-sided hexagon has no architectural/design implications. Any number of sides would have sufficed. He chose the hexagon mainly because it was the easiest non-rectangular polygon to draw. Here's 1-minute [video](https://www.youtube.com/watch?v=jkZiEtvjL-g) where Cockburn talks about his choice of a hexagon.

Cockburn did want a shape with sides rather than a circle, which distinguishes HexArch from CleanArch in the diagrams. He wanted each side of a polygon to represent a _facet_ as an access point. A polygon could have any number of facets or access points that it needed.

The polygon, whether a hexagon or other shape, isn’t an element in the implementation. It’s not a class, object, or other implementation concern. The hexagon is a design concept that defines a closed boundary and dependency and knowledge constraints associated with those boundaries. I’ll address the boundaries and constraints in future blog posts.

Several years after his first designs, Cockburn was also learning more about design patterns, He realized that a _facet_ is really a _port_, and he changed the name to _Ports and Adapters_. He still prefers this new name since it’s more descriptive of the pattern. But by then, many had been calling it _Hexagonal Architecture_ and the original name still stuck. So now we have both names.

# Design Patterns
The HexArch/P&A design is a _pattern of design patterns_, which only requires knowledge of the [7 Essential Design Patterns](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html) to understand.

I’m taking this detour into HexArch/P&A because it will reinforce those design patterns. I also think that HexArch/P&A is a fantastic way to design and implement a component without a huge investment. It can be applied on a large or small scale. It is not the kind of architecture/design choice that causes a major upheaval to the current code base. It can also be introduced into an existing design without having to restructure the entire code base.

# Pluggable Design
Regardless of the name, HexArch/P&A, CleanArch, and OnionArch are all based upon a pluggable design.

<img src="https://images.rawpixel.com/image_800/czNmcy1wcml2YXRlL3Jhd3BpeGVsX2ltYWdlcy93ZWJzaXRlX2NvbnRlbnQvbHIvcHg3NjQ3NjUtaW1hZ2Uta3d2dXpmZ3EuanBn.jpg" alt="Cocoons" title="Image Source: https://www.rawpixel.com/image/5905381/photo-image-flower-public-domain-green
https://images.rawpixel.com/image_800/" width = "35%" align="right" style="padding-right: 35px;">
 
The common core theme is that Business Domain implementation is sheltered in its own cocoon protected from external dependencies. I’ll spend more time in future blogs about what an external dependency may be, but to make it a bit more concrete here, one common example of an external dependency is the database.

The Business Domain implementation depends upon access points, which are the _Ports_ in Cockburn’s nomenclature. In the implementation, these _Ports_ will usually be interface contracts. Pluggable elements resolve the interface contract port references, which allows the Business Domain implementation to interact with external dependencies without depending upon those external dependencies nor having any knowledge of them.

Using the Gang of Four nomenclature, _Ports_ will tend to be [Strategies]( https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html). The pluggable elements will be [Adapters]( https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) or [Façades]( https://jhumelsine.github.io/2023/10/03/facade-design-pattern.html). Object resolution will occur via [Dependency Injection]( https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html).

Since the pluggable design isolates the Business Domain from external dependencies, the design allows us to delay dependency decisions. It allows us more flexibility to change external dependencies. I know that many projects will argue that they’re never going to change their external dependencies. **Is it a case that they are never going to change to those external dependencies because they choose not to, or that they never can change them because they’ve become too tightly coupled to them?** 

And sometimes projects don’t have a choice. A dependency may be an external vendor who goes out of business. A project may be forced to change external dependencies even if that was never the plan.

Even if a project is 100% certain that an external dependency will never change, another reason to favor a pluggable design is for testing. A pluggable design accommodates test doubles more easily.

# Boundaries and Constraints
I’m convinced that HexArch/P&A works because of boundaries and constraints it imposes. The constraint almost screams where implementation should reside and why it should reside there.

I will provide a separate [blog](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html) about the boundaries and constraints.

# That Hexagon Really Pulled the Room Together
<img src="https://media.makeameme.org/created/that-hexagon-really.jpg" alt="The Dude" title="Image Source: https://makeameme.org/meme/that-hexagon-really" width = "30%" align="right" style="padding-right: 35px;">
 
HexArch/P&A pulls a lot of ideas together. It’s a structure which can be layered on top of other software engineering concepts, such as: Domain-Driven Design, Agile, Framework management, Form Factor management, Internationalization (I18n), Localization (L10n) and Accessibility (A11y).

And once we see how these different concepts relate to HexArch/P&A, we can see how they relate to each other. For example, Anti-Corruption Layer and Adapter are the same concept in two different contexts.

# Summary
This has been a brief introduction to HexArch/P&A. Hopefully it has whetted your appetite for more. I’ll focus upon different aspects of HexArch/P&A in subsequent blogs posts. I’ll add forward links when those blogs are posted.

If you want to learn more about HexArch/P&A in the interim, please do so with some of the references provided below.

# References
Here are some free resources:
* [Hexagonal Architecture on Wikipedia](https://en.wikipedia.org/wiki/Hexagonal_architecture_(software))
* [Alistair Cockburn's Hexagonal Architcture page](https://alistair.cockburn.us/hexagonal-architecture/)
* [Alistair Cockburn's Hexagonal Architecture reference page](https://hexagonalarchitecture.org/)
* [Hexagonal Architecture on Wiki C2](https://wiki.c2.com/?HexagonalArchitecture)
* [Hexagonal Me](https://jmgarridopaz.github.io/) - Great source of Hexagonal Architecture information
* [Hexagonal architecture – is it for me? A no-nonsense overview](https://tsh.io/blog/hexagonal-architecture/)
* [HEXAGONAL ARCHITECTURE - WHAT IS IT? WHY SHOULD YOU USE IT?](https://www.happycoders.eu/software-craftsmanship/hexagonal-architecture/)
* [Hexagonal Architecture Design Pattern](https://www.mitrais.com/news-updates/hexagonal-architecture-design-pattern/)
* [Hexagonal Architecture and Clean Architecture (with examples)](https://dev.to/dyarleniber/hexagonal-architecture-and-clean-architecture-with-examples-48oi)
* [HEXAGONAL ARCHITECTURE](https://www.qwan.eu/2020/08/20/hexagonal-architecture.html)
* [DDD, Hexagonal, Onion, Clean, CQRS, … How I put it all together](https://herbertograca.com/2017/11/16/explicit-architecture-01-ddd-hexagonal-onion-clean-cqrs-how-i-put-it-all-together/)
* [Organizing Layers Using Hexagonal Architecture, DDD, and Spring](https://www.baeldung.com/hexagonal-architecture-ddd-spring)
* [Hexagonal Architecture: What Is It and How Does It Work?](https://dzone.com/articles/hexagonal-architecture-what-is-it-and-how-does-it)
* [Architectural Approaches: Clean And Hexagonal Architecture](https://apiumhub.com/tech-blog-barcelona/clean-and-hexagonal-architecture/)
* [A Color Coded Guide to Ports and Adapters](https://8thlight.com/insights/a-color-coded-guide-to-ports-and-adapters)
* [Ports & Adapters (aka hexagonal) architecture explained](https://codesoapbox.dev/ports-adapters-aka-hexagonal-architecture-explained/)
* [Ports & Adapters Architecture](https://herbertograca.com/2017/09/14/ports-adapters-architecture/)
* [Ports-And-Adapters / Hexagonal Architecture](https://www.dossier-andreas.net/software_architecture/ports_and_adapters.html)
* [Ports-and-Adapters Architecture: The Pattern](https://embeddeduse.com/2023/08/24/ports-and-adapters-architecture-the-pattern/)
* [Hexagonal Architecture - Ports and Adapters](https://journal.optivem.com/p/hexagonal-architecture-ports-and-adapters)
* [The Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) - The first Clean Architecture blog by Bob Martin from 2012
* [Screaming Architecture](https://blog.cleancoder.com/uncle-bob/2011/09/30/Screaming-Architecture.html) - Early Clean Architecture blog by Bob Martin from 2011, which focuses upon obvious architectures
* [Summary of book "Clean Architecture" by Robert C. Martin](https://gist.github.com/ygrenzinger/14812a56b9221c9feca0b3621518635b)
* [Clean architecture for the rest of us](https://pusher.com/tutorials/clean-architecture-introduction/)
* [Use Case Diagrams Wikipedia](https://en.wikipedia.org/wiki/Use_case_diagram) - Use Cases are referenced quite a bit in Clean Architecture and sometimes in Hexagonal Architecture
* and for more Google [Hexagonal Architecture Ports and Adapters Clean Architecture Onion Architecture](https://www.google.com/search?q=Hexagonal+Architecture+Ports+and+Adapters+Clean+Architecture+Onion+Architecture)

Here are some free online video resources:
* ["Hexagonal Architecture & Legacy Code" by Jim Humelsine](https://www.youtube.com/watch?v=aayl6FysZ_U)
* [Alistair in the "Hexagone" 1/3](https://www.youtube.com/watch?v=th4AgBcrEHA)
* [Alistair in the "Hexagone" 2/3](https://www.youtube.com/watch?v=iALcE8BPs94)
* [Alistair in the "Hexagone" 3/3](https://www.youtube.com/watch?v=DAe0Bmcyt-4)
* [Hexagonal Architecture: What You Need To Know - Simple Explanation](https://www.youtube.com/watch?v=bDWApqAUjEI)
* [Hexagonal Architecture and Clean Architecture (with examples)](https://www.youtube.com/watch?v=bRl-sTvLbsI)
* [Hexagonal Architecture (Ports and Adapters) with Alistair Cockburn](https://www.youtube.com/watch?v=AOIWUPjal60)
* [Structure your microservice using a hexagonal architecture by Fredrik Lindgren](https://www.youtube.com/watch?v=QCOMeiMYb6I)
* [The Principles of Clean Architecture by Uncle Bob Martin](https://www.youtube.com/watch?v=o_TH-Y78tt4)
* [ITkonekt 2019 Robert C. Martin (Uncle Bob), Clean Architecture and Design](https://www.youtube.com/watch?v=2dKZ-dWaCiU)
* [Robert C Martin - Clean Architecture and Design](https://www.youtube.com/watch?v=Nsjsiz2A9mg)
* [More Testable Code with the Hexagonal Architecture](https://www.youtube.com/watch?v=ujb_O6myknY))

Here are some resources that can be purchased or are included in a subscription service:
* **Clean Architecture** by Bob Martin ([Amazon](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164) and [O'Reilly](https://learning.oreilly.com/library/view/clean-architecture-a/9780134494272/))
* **Get Your Hands Dirty on Clean Architecture** by Tom Hombergs ([Amazon](https://www.amazon.com/Your-Hands-Dirty-Clean-Architecture/dp/180512837X) and [O'Reilly](https://learning.oreilly.com/library/view/get-your-hands/9781805128373/))
* **Architecture, Use Cases and High Level Design**, Video by Bob Martin ([O'Reilly](https://learning.oreilly.com/videos/clean-coder-clean/9780134843803/))
