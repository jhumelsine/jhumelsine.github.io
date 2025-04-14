---
title: DRAFT – Humble Objects - Designing Code You Don’t Hate Testing
description: A pattern to help test code that's difficult to test
unlisted: true
---

# Introduction
[_Oh Lord, it’s hard to be __humble__ when you’re perfect in every way._](https://www.youtube.com/watch?v=0WTrMuZOZvM) — [__Mac Davis__](https://en.wikipedia.org/wiki/Mac_Davis): American songwriter, singer, performer, and actor

When tests are painful, it's often because our code wasn’t built for testing. That’s where [seams](https://jhumelsine.github.io/2025/03/24/legacy-code.html#seams) come in — and sometimes, the best seam is a __Humble Object__.

I first encountered the Humble Object Pattern in Bob Martin’s _Clean Architecture_ book in [Chapter 23 - Presenters and Humble Objects](https://learning.oreilly.com/library/view/clean-architecture-a/9780134494272/ch23.xhtml#ch23), which starts with:
>The _Humble Object_ pattern is a design pattern that was originally identified as a way to help unit testers to separate behaviors that are hard to test from behaviors that are easy to test. The idea is very simple: Split the behaviors into two modules or classes. One of those modules is humble; it contains all the hard-to-test behaviors stripped down to their barest essence. The other module contains all the testable behaviors that were stripped out of the humble object.

In hindsight it makes perfect sense to me now, but it left me a bit baffled at the time. Bob moves immediately in showing how the Humble Object can be used to help test GUIs. Not being a front-end developer, I didn’t quite grasp the broad applicability of the Humble Object Pattern at the time.

In the spirit of this pattern's humble nature, I’m going to try to keep this blog entry humble and concise.

# The Humble Object Pattern
Most patterns I’ve presented have featured structure or behavior. They have included UML class diagrams and/or code examples. The Humble Object Pattern tends to be more procedural and conceptual than structural or behavioral making it somewhat similar to the [Façade Design Pattern](https://jhumelsine.github.io/2023/10/03/facade-design-pattern.html) in that aspect.

This pattern is about making code easier to test especially when coupled to a software element, often an external dependency, that makes testing challenging in its original form.

## The Humble Object Process
[_I use the Humble Object pattern. Push as much intelligence as possible out into a testable component. Humiliate the [remaining small, humble and] dirty component to the point that it is obviously correct and requires no tests._]( https://x.com/unclebobmartin/status/1122722265651720198) — __Bob Martin__ on X/Twitter

Introducing the Humble Object Pattern to an implementation consists of these basic steps:
1. Recognize that a section of code is difficult to test because of a tight dependency that cannot be easily be emulated via a [test double](https://jhumelsine.github.io/2024/07/02/test-doubles.html).
2. Isolate the smallest portion as possible of the dependency via [extract method](https://jhumelsine.github.io/2025/03/24/legacy-code.html#legacy-code-lacks-seams). Since the extracted method cannot be easily unit tested, even after extraction, keep it as small as possible, i.e., ___humble___. Correctness is usually verified upon visual inspection of the humble code and how it functions when integrated with other parts of the system. This is fine — because humble objects deliberately avoid logic worth unit testing.
3. Declare the extracted method as __package-private__ or possibly extract it as an implementation to an interface so that it can be replaced with a test double in a unit test.

The original code can be unit tested with test doubles emulating the Humble Object's behavior.

The Humble Object Pattern is introducing a seam, but in the context of isolating the code that cannot be unit tested and keeping it as small and humble as possible.

## Humble Object Examples
Several previous blog entries have already illustrated the Humble Object Pattern even though I had not introduced the term yet. I won’t repeat details here, since they can be obtained in the original referenced posts.

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
Flaky tests are the worst. A flaky test is a non-deterministic unit test that sometimes passes and sometimes fails. We want our tests to be deterministic. They should always render consistent results.

We can't trust flaky tests. What’s the true nature of a flaky test? When it passes, is it representing the true nature of the software being tested or is it a false positive? When it fails, does the code really have an error or is it a false negative?

A failing flaky test can abort a CI/CD pipeline build, delaying delivery. When that happens, we often start the pipeline again, hoping that all of the flaky tests will pass. I’ve done this myself. Are we really confirming that the code works, or are we just using the retry as a workaround to detour an inconvenience that's preventing us from delivering our changes?

If the implementation is flaky enough to exhibit inconsistent behaviors in our tests, what’s to prevent the same code from exhibiting inconsistent behaviors in production?

Any form of non-deterministic dependency can cause tests to be flaky. Race conditions are a common culprit. In [Semaphore with Suril](https://jhumelsine.github.io/2024/07/08/suril-semaphore.html#the-semaphore), I had been contemplating tests with multiple Threads, Sleeps, Timeouts and more before considering the Humble Object Pattern. I can almost guarantee that any non-Humble Object tests would have been flaky, slow or both.

While the Humble Object Pattern won’t be a panacea for curing all flaky tests, it can provide quite a bit of relieve. Isolate the non-determinist elements of the implementation into a Humble Object and replace them with a deterministic Test Double in the tests.

# Summary
Most code is testable — until it isn’t. GUIs, APIs, random numbers, clock time: they fight back. The Humble Object pattern is a reminder that good design cares about your tests too.

Try to write code you don’t hate testing.

# References
Here are some free resources:
* [Humble Object](https://martinfowler.com/bliki/HumbleObject.html) - Blog by Martin Fowler
* [Humble View](https://martinfowler.com/eaaDev/uiArchs.html#HumbleView) - Blog by Martin Fowler using the Humble Object Pattern with a GUI.
* [Humble Object Pattern](https://mercury-leo.github.io/teaching/HumbleObjectPattern/) - Blog by Mercury Finnegan
* [Humble Object pattern](https://github.com/mistermicheels/learning-notes/blob/master/architecture-design/Humble-Object-pattern.md) - GitHub page by mistermicheels
* [Unit testing part 2 - Unit testing MonoBehaviours](https://unity.com/blog/technology/unit-testing-part-2-unit-testing-monobehaviours) - Blog by TOMEK PASZEK featuring the Humble Object Pattern
* [TDD Patterns: Humble Object](https://ieftimov.com/posts/tdd-humble-object/) - Blog by Ilija Eftimov
* [Humble Object](http://xunitpatterns.com/Humble%20Object.html) - Article by xUnit Patterns
* [Testing multithreaded code](https://elecodev.wordpress.com/2015/08/10/testing-multithreaded-code/) - Blog by elecodev, java fanboy, featuring the Humble Object Pattern when testing multithreaded code
* [Clean Architecture - Robert (Uncle Bob) Martin](https://www.youtube.com/watch?v=G08FxxwPjXE&t=2042s) - Video presentation at DevTernity Conference queued to where Bob makes a passing mention of the Humble Object Pattern technique, but doesn't name it as such.
* _Coding Blocks Episode 76: Clean Architecture – What is the Humble Object Pattern?_ ([Landing Page](https://www.codingblocks.net/podcast/clean-architecture-what-is-the-humble-object-pattern/) and [Video](https://www.youtube.com/watch?v=-M0301nxE4Q))
* and for more, Google: [Humble Object Pattern](https://www.google.com/search?q=Humble+Object+Pattern )

Flaky Tests:
* [Why Are My Test Flakey?](https://www.industriallogic.com/blog/why-are-my-tests-flakey/) - Blog by Tim Ottinger
* [Your Tests Are Failing YOU!](https://www.youtube.com/watch?v=bI2HQ2N_gwY) - Video by Trisha Gee
* [5 Reasons Your Automated Tests Fail](https://www.youtube.com/watch?v=vHBzZHE4tJ0) - Video by Dave Farley
* and for more, Google: [Flaky Tests](https://www.google.com/search?q=flaky+tests)
  
Here are some resources that can be purchased or are included in a subscription service:
* _Clean Architecture: A Craftsman's Guide to Software Structure and Design_ ([O'Reilly](https://learning.oreilly.com/library/view/clean-architecture-a/9780134494272/) and [Amazon](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164/))
* _Clean Code: Advanced TDD, Episode 23, Part 2, Mocking_ ([O'Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_02_23_02/) and [Clean Coders](https://cleancoders.com/episode/clean-code-episode-23-p2)) - This video includes The Humble Object Pattern
