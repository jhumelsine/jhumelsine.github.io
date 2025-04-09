---
title: DRAFT – Humble Object Pattern
description: A pattern to test code that's difficult to test
unlisted: true
---

# Introduction
[_Oh Lord, it’s hard to be humble when you’re perfect in every way._](https://www.youtube.com/watch?v=0WTrMuZOZvM) – [__Mac Davis__](https://en.wikipedia.org/wiki/Mac_Davis): American songwriter, singer, performer, and actor

I first encountered the Humble Object Pattern in Bob Martin’s _Clean Architecture_ book in [Chapter 23 - Presenters and Humble Objects](https://learning.oreilly.com/library/view/clean-architecture-a/9780134494272/ch23.xhtml#ch23), which starts with:
>The _Humble Object_ pattern is a design pattern that was originally identified as a way to help unit testers to separate behaviors that are hard to test from behaviors that are easy to test. The idea is very simple: Split the behaviors into two modules or classes. One of those modules is humble; it contains all the hard-to-test behaviors stripped down to their barest essence. The other module contains all the testable behaviors that were stripped out of the humble object.

In hindsight it makes perfect sense to me now, but it left me a bit baffled at the time. Bob moves immediately in the next paragraph showing how the Humble Object can be used to help test GUIs. Not being a front-end developer, I didn’t quite grasp the general nature of the Humble Object Pattern at the time.

In the spirit of this pattern's humble nature, I’m going to try to keep this blog entry humble and concise.

# The Humble Object Pattern
Most patterns I’ve presented have featured structure or behavior. They have featured UML class diagrams and/or code examples. The Humble Object Pattern tends to be more conceptual than structural or behavioral patterns making it somewhat similar to the [Façade Design Pattern](https://jhumelsine.github.io/2023/10/03/facade-design-pattern.html) in that aspect.

This pattern is about making code easier to test especially when coupled to a software element, often an external dependency, that makes testing challenging in its original form. Unlike most of my previous pattern blog entries, this one will not include a UML class diagram or code examples. 

## The Humble Object Process
[_I use the Humble Object pattern. Push as much intelligence as possible out into a testable component. Humiliate the [remaining small, humble and] dirty component to the point that it is obviously correct and requires no tests._]( https://x.com/unclebobmartin/status/1122722265651720198) - __Bob Martin__ on X/Twitter

Introducing the Humble Object Pattern to an implementation consists of basically these steps:
1. Recognize that a section of code is difficult to test because of a tight dependency that cannot be easily be emulated via a [test double](https://jhumelsine.github.io/2024/07/02/test-doubles.html).
2. Isolate the smallest portion possible portion of the dependency via [extract method](https://jhumelsine.github.io/2025/03/24/legacy-code.html#legacy-code-lacks-seams). 
3. The extracted method cannot be easily unit tested, even after extraction. Therefore, it’s kept as small as possible, i.e., ___humble___. Correctness usually depends upon visual inspection of the humble code and how it functions when integrated with other parts of the system.
4. The extracted method will be __package-private__ or possibly extracted as an implementation to an interface so that it can be replaced with a [test double](https://jhumelsine.github.io/2024/07/02/test-doubles.html) in a unit test.
5. The original code can be unit tested with test doubles emulating the humble object's behavior.

The Humble Object Pattern is introducing a [seam](https://jhumelsine.github.io/2025/03/24/legacy-code.html#seams), but in the context of isolating code that cannot be unit tested and keeping it as small as possible.

## Humble Object Examples
Previous blog entries have already illustrated the Humble Object Pattern even though I had not introduced the term yet. I won’t repeat details here, since they can be obtained in the original referenced posts.

## Suril, the Semaphore and Me
In [Suril, the Semaphore and Me](https://jhumelsine.github.io/2024/07/08/suril-semaphore.html), Suril and I resolved a challenging unit test problem by extracting a `Semaphore` call as a __package-private__ method, and then we injected test double behavior to inject `Semaphore` behavior in the test cases.

## Approval Testing
In [Some Behaviors Require Observation](https://jhumelsine.github.io/2025/04/02/approval-testing.html#some-behaviors-require-observation), I described how the Humble Object Pattern can be used to separate the element that renders the GUI from the ASCII representation of the content to be rendered.

The element that renders the GUI, which could be a browser, is the Humble Object. We want to test the ASCII content that’s rendered by the browser, but not the browser itself.

In one of Bob Martin’s videos, he provides another version of this where he tests the rendering itself. His demo features Java Swing code that contains just enough code to render a small window with a few elements on it. All functional behavior is delegated to an interface. In his GUI test, he provides a [Test Double Fake](https://jhumelsine.github.io/2024/07/02/test-doubles.html#fake) that provides just enough behavior to render the GUI realistically.

Then Bob confirms via his own visual inspection that the Test Double Fake supported GUI displays the faked behavior as desired.

## Time Lord Clock
In [Time Lord Clock](https://jhumelsine.github.io/2025/04/08/time-lord.html#time-is-an-external-dependency), I described how to take control of time in unit tests by introducing a `Clock` interface, which would be resolved with the humble system clock in production and a stubbed clock in the unit test.

## Adapters
[Adapters](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) are often Humble Objects. Their implementations tend to be small, and they tend to be coupled with external dependencies, making them difficult to test, but relatively easy to override with test doubles.

# Flaky Tests
Flaky tests are the worst. A flaky test is a unit test that sometimes passes and sometimes fails. These are non-deterministic tests. We want our tests to be deterministic. They should always render consistent results.

What’s the true nature of a flaky test? When it passes, is it representing the true nature of the software being tested or is it a false positive? When it fails, does the code really have an error or is it a false negative?

A failing flaky test can abort a CI/CD pipeline build delaying delivery. When that happens, we often start the pipeline again, hoping that all of the flaky tests will pass. I’ve done this myself.

If the implementation is flaky enough exhibit inconsistent behaviors in our tests, what’s to prevent the same code from exhibiting inconsistent behaviors in production?

While the Humble Object Pattern won’t be a panacea curing all flaky tests, it can probably provide quite a bit of relieve. In the [Semaphore with Suril](https://jhumelsine.github.io/2024/07/08/suril-semaphore.html#the-semaphore), I mused several possibly approaches before stumbling upon the Humble Object Pattern. I was contemplating multiple Threads, forced Timeouts, etc. I can almost guarantee that a non-Humble Object Pattern test would have been flaky.

# Summary
__TBD__

# References
* https://learning.oreilly.com/library/view/clean-architecture-a/9780134494272/ch23.xhtml#ch23
* https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_02_23_02/
* https://www.youtube.com/watch?v=G08FxxwPjXE&t=2042s
* https://www.youtube.com/watch?v=-M0301nxE4Q
* https://martinfowler.com/bliki/HumbleObject.html
* https://www.codingblocks.net/podcast/clean-architecture-what-is-the-humble-object-pattern/
* https://mercury-leo.github.io/teaching/HumbleObjectPattern/
* https://feedback.refactoring.guru/communities/3/topics/4949-add-the-humble-object-design-pattern
* https://blog.cleancoder.com/uncle-bob/2016/03/19/GivingUpOnTDD.html
* https://github.com/mistermicheels/learning-notes/blob/master/architecture-design/Humble-Object-pattern.md
* https://cleancoders.com/episode/clean-code-episode-23-p1
* https://www.youtube.com/watch?v=bI2HQ2N_gwY
* https://www.youtube.com/watch?v=vHBzZHE4tJ0
* https://unity.com/blog/technology/unit-testing-part-2-unit-testing-monobehaviours
* https://ieftimov.com/posts/tdd-humble-object/
* http://xunitpatterns.com/Humble%20Object.html
* https://martinfowler.com/eaaDev/uiArchs.html#HumbleView
* https://elecodev.wordpress.com/2015/08/10/testing-multithreaded-code/
* https://www.google.com/search?q=flaky+tests
* https://www.google.com/search?q=Humble+Object+Pattern 
