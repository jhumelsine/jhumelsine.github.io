---
title: Façade Design Pattern
description: A Façade is just an illusion making a task appear easier than it really is.
unlisted: true
---
![Movie Facade](https://static.ferrovial.com/wp-content/uploads/sites/3/2018/03/13150850/set-for-a-movie.png)
 
# Introduction
I was not impressed with Façade when I first read about it. There didn’t seem to be any repeatable pattern associated with the design. But later I realized it’s such a useful concept that it made my [Essential Design Pattern](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html) list.

# I Have to Implement How Many Classes?
Almost 20 years ago, I was on a project where the primary task was to build a [middleware](https://en.wikipedia.org/wiki/Middleware) platform supporting a huge software ecosystem. We just weren’t creators of the middleware, we were users of it too. Often, we were using other middleware components about as quickly as they were being implemented.

My feature required a Client/Server structure, which would be supported by the Communication Manager system in the middleware. I wasn’t sure how to use it since there wasn’t any user documentation yet. Fortunately, one of the developers of the Communication Manager sat in the office next to mine.

I stopped by and asked him how it worked. I described my relatively simple Client/Server needs, and he rattled off all the classes I would need to implement.
It was somewhere along the lines of:
* `Request` which had to be serializable.
* `Response` which had to be serializable as well.
* `Reader`
* `Writer`
* `Listener`
* There may have been more, but I’ve repressed those memories.

That was only for the Client. I’d need to implement pretty much the same set on the Server too. My head was spinning. I asked if there were any code examples that I could follow to get a sense of what was needed. They were still working on code examples for the documentation, but they did have some tests.

The tests weren’t designed in a _Given/When/Then_ or _Assign/Act/Assert_ structure. They were just pages upon pages of code that tried to cover all possible scenario combinations in as few test methods as possible. I wanted to curl up in a ball in the corner of my office and whimper. In all fairness, this was before most of us had any idea how to design tests properly.

I returned to his office with the test printouts and a pen. We reviewed the test code together. We crossed out parts that I didn’t need. We circled parts that were needed.

Using the test code as a reference, I started to implement my classes by implementing the Command Manager interfaces. There were dozens of methods defined in each interface. But only a few were needed for Client/Server communication. The other methods were for other communication styles, such as Broadcast, Pub/Sub, etc. I had to provide an implementation for all methods even for the communication styles that I was not using. The architect/designer apparently had not heard of the [Interface Segregation Principle](https://en.wikipedia.org/wiki/Interface_segregation_principle).

I implemented the Client/Server methods. I didn’t want a silent no-op implementation for the other communication styles, so each of those threw a `NotImplementedException` in case they were executed.

I hobbled together some code, and my office neighbor gave it the thumbs-up.

# A Lot of Infrastructure Was Required
The size of my communication management implementation rivaled the size of my business logic. All I really wanted or needed was the ability to send a request and get a response. So that’s what I did. I created an interface along these lines:
```java
interface ClientConnection {
    Response send(Request request);
}
```
The `ClientConnection` implementation hid all the complicated coordination interactions among `Reader`, `Writer`, `Listener`, etc. The cumbersome infrastructure was still there, but it was encapsulated within the `ClientConnectionImpl`. It wasn’t visible in my business logic.

**I had created a Façade.** I showed my neighbor what I had created, and said, “You should provide a Façade to do this too.” He just stared at me. It never materialized.

The online documentation was eventually written, but long after I needed it. We had the ability to generate a PDF file of the documentation for the entire middleware. It was over 6,000 PDF pages! That’s enough to contain the sacred texts of several major religions with room left over for a Stephen King novel or two. The Communication Manager section alone was over 1,000 PDF pages.

# Structure
There is no structure with Façade. There are no nifty UML class diagrams. It’s all about intent. It’s about creating a Façade interface that presents more clarity for the Client Application.

It’s just an implementation that delegates to other classes/objects. Each Façade implementation will be different, because the delegate classes and objects will differ in each scenario.

Here’s a sketch of the `ClientConnection` Façade design from my story:
![ClientConnection UML Class Diagram](/assets/ClientConnectionUML.png)
 
# Isn’t This the Adapter Design Pattern?
Façade looks very much like the [Adapter Design Pattern](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html).
Both solve similar problems in that they both allow delegation when the contract interfaces in dependencies don't align with busines logic needs.

Adapter bridges a communication gap.
Façade bridges a complexity gap. 

Adapter allows one class to interact with another class.
Façade allows one class to interact with many different classes.

Adapter implementations tend to be small. 
Façade implementations tend to be larger, since they coordinate delegation among multiple classes and objects.

# Summary
Façade is like a magic trick that’s all showmanship. The Façade illusion doesn’t involve a hidden gimmick. It’s an illusion that focuses upon the Client Application’s needs regardless of how many classes and objects are required to satisfy those needs.

I used to be frustrated when class designers hoisted complicated interfaces upon me, such as ones that require over 1,000 pages to document. I wanted them to provide a Façade that met my needs. I never got one. I found that when I needed a Façade, I was the developer who wrote it. I still couldn’t divorce myself from the complicated interfaces, but I was able to isolate the complications from the rest of my code.

Later I realized that in most cases, this is pretty much what must happen. The developers of the complicated class have no idea what I need. Admittedly, they could have probably done a better job in designing a better interface based upon what the client application may need rather than what they provide. But they can never provide an interface that meets all client application needs precisely.

**Creating your own Façade is a bit of an illusion, but it is also the great power of this design pattern.** You can design a bespoke Façade interface that meets your client application needs precisely. You can put as much or as little into your Façade as you desire. Start with a Façade interface first and then figure out later how to implement it. If the Façade interface gets too large or loses cohesion, then split it into separate Facades interfaces that each retain their own cohesion.

If contract interfaces for the dependencies change, then you may be able to absorb those changes in the Façade implementation without having to change the Façade interface or the Client Application.

**This power is why Façade is on my [Essential Design Pattern](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html) list.**

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* [Wikipedia Façade Design Pattern](https://en.wikipedia.org/wiki/Facade_pattern)
* [Source Making Façade Design Pattern](https://sourcemaking.com/design_patterns/facade)
* [Refactoring Guru Façade Design Pattern](https://refactoring.guru/design-patterns/facade)
* [DoFactory Façade Design Pattern](https://www.dofactory.com/net/facade-design-pattern)
* [Project Management Institute Façade Design Pattern](https://www.pmi.org/disciplined-agile/the-design-patterns-repository/the-facade-pattern)
* and for more Google [Adapter Design Pattern](https://www.google.com/search?q=adapter+design+pattern)

Here are some resources that can be purchased or are included in a subscription service:
* [Gang of Four Adapter Design Pattern](https://learning.oreilly.com/library/view/design-patterns-elements/0201633612/ch04.html#page_192)
* Agile Principles, Patterns, and Practices in C#, Chapter 33 ([O'Reilly](https://learning.oreilly.com/library/view/agile-principles-patterns/0131857258/) and [Amazon](https://www.amazon.com/Agile-Principles-Patterns-Practices-C/dp/0131857258))
* Clean Code: Design Patterns, Episode 33 video ([Clean Coders](https://cleancoders.com/episode/clean-code-episode-33) and [O'Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_03_33_00/))
* Head First Design Patterns, Chapter 7 ([O'Reilly](https://learning.oreilly.com/library/view/head-first-design/9781492077992/ch07.html#facade_pattern_defined) and [Amazon](https://www.amazon.com/Head-First-Design-Patterns-Object-Oriented-ebook/dp/B08P3X99QP))
