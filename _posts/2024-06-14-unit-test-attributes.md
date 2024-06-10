---
title: Attributes of Effective Unit Tests
description: Unit Test properties that make them more useful than not
unlisted: true
---

# Introduction
I listed some of the issues I had with my unit tests in my previous [blog](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#test-code-but-i-wouldnt-call-it-unit-test-code). My early attempts at unit testing were ineffective, and I soon abandoned them.

This blog will focus upon the attributes of effective unit tests. This is not an exhaustive list, and other references will have different attributes as well.

Future blogs will present practices that help achieve and maintain these attributes.

# Behavior Based
Unit Tests should define and confirm observable behavior, not implementation. This can be tricky, since behavior comes from the implementation, and implementation is based upon the behavior.

A good unit test should define the behavior. It should not indicate or depend upon the code. We should be able to refactor the code without breaking the unit tests, where [refactoring](https://refactoring.com/) is defined as changing the structure of the code without changing its behavior.
Unit tests that depend upon the implementation will be more brittle. They will fail as the implementation is updated. They will become a maintenance burden.

# Fine Grained
A unit test should define and confirm a single observable behavior. Since methods often contain multiple behaviors, such as validation and edge cases, a method often requires a suite of multiple unit tests.

When behavior is violated when the implementation changes, then ideally only the unit tests that are associated with that behavior should fail.

# Declarative
Unit tests should declare the observable behavior. They provide context. Not only does this help limit one test to one behavior, but it also documents intent.

When a test fails, it provides context as to where the failure might reside within the implementation. Context may be as mechanical as running the failing test in isolation with coverage activated and then reviewing the statements that were executed.

# Complete
A suite of unit tests should declare and confirm all possible behaviors that the software may encounter. This includes the this-should-never-happen edge cases.

___Hard in Training; Easy in Battle___

I want to subject my code to every conceivable scenario I can imagine, even those that I think are rare or should never happen. I don’t want the first execution of an infrequent edge case to happen in production and ___hope___ that it will work. I want to document and confirm how my code will respond via a unit test.

This is like subjecting astronauts to every conceivable scenario in training so that should they encounter it on a mission, it won’t be their first experience.

Covering all behavior is as much art as science. Desired behavior is often an evolutionary and exploratory process. It won’t all reveal itself on the first and possibly even subsequent iterations.

# Simple
A unit test should be simple. It should declare expected behavior and be obvious in how it will confirm that behavior. Most unit tests should require no more than 20 lines of code.

Large, complicated or ugly unit tests are an indication of a large, complicated or ugly implementation. The problem is not in the test. The problem is in the code.
If unit tests are an issue, then focus upon the implementation.

# Future Proof
Unit tests don’t find bugs now. They help prevent bugs from being introduced in the future.
Code tends to work when we first write it and we’re paying attention to everything. Bug are introduced when the code is subsequently updated and we’re not quite as vigilant. Unit tests confirm that implementation updates are consistent with the existing unit test specified behaviors. Any inconsistencies will result in a failed unit test.

For example, if a method is updated, but that update inadvertently violates existing behavior, then the test that confirms that behavior will fail. We want to see that failing test as soon as possible, ideally in the developer’s environment within seconds or minutes of the inconsistency failure first being introduced. If not immediately, then a bit later during automated testing in the pipeline.
The worst-case scenario is when the user encounters the issue and reports it to customer support.

# Scoped
The software being tested within a unit test is referred to as the [Software Under Test](https://en.wikipedia.org/wiki/System_under_test) (SUT).

The SUT should be tested as if it had been plucked from the codebase and tested in isolation without the SUT knowing that it had been plucked from the codebase. This was a difficult concept for me to understand. I had long considered testing an all-or-nothing situation, especially when dependencies were coupled to the code, and there are always dependencies coupled to the code.

SUT can only be isolated when there are well defined boundaries between software elements that allow us to isolate the SUT from its dependencies via [Test Doubles](https://en.wikipedia.org/wiki/Test_double).

The boundaries can vary for some SUT. For example, there may be rings of boundaries around a class. We could limit the SUT to the first ring or possibly extend the ring to include other classes.

The more expansive the boundary, the greater the scope of the SUT. This is a tradeoff. We exercise more of the implementation in a larger scoped SUT, but when there’s a failure, it may be more difficult to determine the cause. We exercise less of the implementation in a smaller scoped SUT, limiting the space where a bug resides, but we may have to emulate more via Test Doubles.

I feel that unit tests should have smaller scooped SUTs. But [integration tests](https://en.wikipedia.org/wiki/Integration_testing) will have larger scoped SUTs.
Unit tests confirm the nuts-and-bolts of your system. Integration tests will confirm that those nuts-and-bolts work together.

# Isolated
The SUT should be isolated from its external dependencies, such as: Databases, File Systems, Internet, Clocks, and other components of the project. Test Doubles will replace external dependencies by emulating their behaviors.

Any developer should be able to clone a repository, compile the code and the unit tests should execute without environment issues. I.e., the SUT should have no dependencies upon the original developer’s development or test environment.

# Fast
Unit tests should be fast. The test suite for a method or class may contain dozens or even hundreds of unit tests. They should complete in no more than a few seconds. We want developers to run unit tests early and often so that any issues are found and addressed as quickly as possible.

# Independent
Unit tests should be independent of one another. Any subset should be able to run in any order, even simultaneously, without affecting each other.

Some argue that testing independence is the _Unit_ of Unit Testing.

# Pass/Fail
Unit test results should not generate a report or require analysis. The test passes or fails. It’s green or red. The aggregate result of a test suite is the Boolean __AND__ of all the tests within that suite. A test suite passes when all of its individual tests pass. One quick glance should indicate that they all passed.

Additional analysis should only be needed when a test fails so that the cause of the failed test can be understood and addressed.

# Deterministic
Unit tests should produce consistent and deterministic results. There’s nothing quite as frustrating as a flaky test, which sometimes passes and sometimes fails even when nothing has changed in the interim.

On the surface, flaky tests cause process problems when they fail in pipeline builds because the commit will be aborted. You may have to resubmit multiple times until all flaky tests pass. Sometimes it feels like rolling the dice and hoping for a Yahtzee!

But there’s a more insidious problem with flaky tests. We can’t trust them. Are they exhibiting false positive or false negative behavior. Is the code sound, except that the test sometimes fails, or is the code faulty, except that sometimes the test passes? The latter is a more serious issue, since it provides a false sense of security.

The flakiness might be in the test, but often, it’s in the implementation. Flaky tests are often the result of temporal coupling in the code. It could be a timing issue with async processes. It could be a concurrency issue. It could be something else.

Resolving a flaky test may involve updates in the test, but it’s probably going to require updates to the implementation as well. Resolving a flaky test is probably going to take some time to track down and resolve.

# Summary
I’ve listed a dozen attributes that yield effective unit tests. I haven’t specified how to achieve these attributes. I’ll present practices in future blogs.

# References
These references go beyond the scope of this blog, but I’ll address most of the concepts in future blogs about testing.
* Previous [blog references](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#references)
* [Unit Test](https://www.martinfowler.com/bliki/UnitTest.html) blog by Martin Fowler
* [Unit Testing](https://en.wikipedia.org/wiki/Unit_testing) on Wikipedia
* [Unit Test](https://wiki.c2.com/?UnitTest) on wiki.c2.com
* [What is Unit Testing](https://smartbear.com/learn/automated-testing/what-is-unit-testing/) on smartbear.com
* [Unit Testing: Definition, Examples, and Critical Best Practices]( https://brightsec.com/blog/unit-testing/) blog by Nickolay Bakharev
* [You Still Don’t Know How to Do Unit Testing (and Your Secret is Safe with Me)](https://stackify.com/unit-testing-basics-best-practices/) on Stackify
* [What Is Unit Testing? Types, Tools, and Best Practices](https://www.spiceworks.com/tech/devops/articles/what-is-unit-testing/) blog by Chiradeep BasuMallick 
* [Unit Testing](https://tuskr.app/learn/unit-testing) on tuskr
* [Unit Testing Best Practices: 9 to Ensure You Do It Right](https://www.testim.io/blog/unit-testing-best-practices/) on testim
* [What Is Unit Testing?](https://builtin.com/software-engineering-perspectives/unit-testing) blog by Artem Oppermann
* [Unit Testing: Principles, Benefits & 6 Quick Best Practices](https://codefresh.io/learn/unit-testing/) on codefresh
* [Unit Testing: What it is, How it Works, Types & Top Benefits](https://testsigma.com/blog/unit-testing/) blog by Diane Wong
* [Unit Testing](https://www.agilealliance.org/glossary/unit-test/) on agilealliance.org/glossary
* [Unit Test – Definition](https://www.artofunittesting.com/definition-of-a-unit-test) on The Art of Unit Testing
* [What Is Unit Testing?](https://www.youtube.com/watch?v=x95ez7_V7rA) video on Surfing Scratcher
* Playlists on Dave Farley’s [video channel](https://www.youtube.com/@ContinuousDelivery/):
    * [Automated Testing](https://www.youtube.com/watch?v=fPlBLlE8vOI&list=PLwLLcwQlnXBzwEqy9R3odTJKURxfwqDXa)
    * [TDD – Test Driven Development](https://www.youtube.com/watch?v=llaUBH5oayw&list=PLwLLcwQlnXByqD3a13UPeT4SMhc3rdZ8q)
    * [Acceptance Testing & BDD](https://www.youtube.com/watch?v=llaUBH5oayw&list=PLwLLcwQlnXByqD3a13UPeT4SMhc3rdZ8q)
