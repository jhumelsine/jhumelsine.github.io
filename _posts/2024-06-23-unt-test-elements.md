---
title: Basic Elements of Automated Unit Tests
description: Elevating automated tests to first-class citizen status
unlisted: true
---

# Introduction
I’ve posted two blogs about unit testing with the first being about my [becoming a Unit Test advocate](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html) and the second about the [attributes of effective unit tests](https://jhumelsine.github.io/2024/06/14/unit-test-attributes.html). Yet I haven’t even hinted as to what a unit test is.

I’ll devote this blog to the fundamental elements of unit tests. These fundamental elements tend to apply to all automated tests.

__Caveat__ – I’m going to focus upon the fundamental elements of unit/automated tests. I’m not going to provide specific unit test examples. Unit test examples for different languages are abound upon the internet.

# We’ve Always Been Testing Our Code
We’ve always tested our code, even when the code has been proven correct.

_Beware of bugs in the above code; I have only proved it correct, not tried it._ — Donald Knuth

I referenced my testing experiences from my college years in [Formal Proofs](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#formal-proofs):
> Our testing consisted of running our programs on the assignment data until it produced the desired results. Once we saw it work, we’d grab our results, pack up our gear, head home and hope for a few hours of sleep before the next day of classes began.

In most courses, we never had to look at the code again. Those programs had no future. They were one-off assignments. We’d get a fresh programming assignment and start the process anew all over again.

While most of us got better at testing in our careers, it was probably a refinement of the techniques we picked up in college. We’d inject more ad-hoc tests as we were writing the code, but testing often wasn’t much fun. The tests might be difficult to set up. We might only be able to run them with the entire product possibly in the [lab](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#introduction):
> About a dozen years ago, I worked on a tightly knit small team. I wrote some of the best code of my career while on that team. It just wasn’t the team that was tight. Our code was tightly coupled as well. It was modular, but it was still tightly coupled.

> Often when we needed to test new behavior, we’d compile the entire product on our desktop machines, copy the executable to a thumb drive, walk it into our __QA lab__, copy the executable from the thumb drive to a server and then try to observe the new or updated functionality through the user interface.

Our tests were probably not documented. Any insights residing in those tests probably existed only in our heads. The tests weren’t consistently executed upon subsequent updates, and if so, probably only by us. The tests tended to be manual and subject to human error. And as the number of tests grew, their execution wasn’t scalable.

One-off testing is adequate for one-off programs, such as college assignments. But software engineering involves programs that will be in production for months and years, possibly decades. Production code doesn’t have to just work now. It must continue working in the future. One-off testing is inadequate.

__I advocate elevating testing to first-class citizen status as part of the software development process.__ The first step in this journey will be to automate tests so that they can be executed frequently and consistently to ensure that our programs remain in working order.
I will mostly focus upon unit testing here, but I believe that the principles apply to other layers of testing as well.

# It’s All Code
A unit test is code that executes with the scope of the [Software Under Test](https://jhumelsine.github.io/2024/06/14/unit-test-attributes.html#isolated) (SUT). It confirms that the SUT’s behaviors match what’s expected.

No special tooling is technically required. I’ve been working through the [Advent of Code](https://adventofcode.com/) backlog between blog posts, and all my tests are homespun. They are methods that execute and confirm the behavior of the code I’m writing to solve each day’s coding challenge. The tests run automatically when I compile.

The advantage of using homespun techniques is that you’ll understand every piece of your testing environment. There are no hidden mysteries of a separate test harness package.

Homespun testing is fine for learning with small personal projects, such as Advent of Code, but it won’t scale to larger projects. Homespun techniques don’t tend to have many bells-and-whistles. The tests in my Advent of Code homespun environment are executed in order until one fails. None of the subsequent tests are executed after the first failed test.

Here are links to some of my homespun testing, which I featured in previous blog posts. Search for __assertEquals__ in the posts:
* [Interpreter Design Pattern – Design to Implementation](https://jhumelsine.github.io/2024/04/17/interpreter-design-pattern-implementation.html#the-complete-implementation)
* [Interpreter Design Pattern – Scanner and Parser Implementation]( https://jhumelsine.github.io/2024/04/30/interpreter-design-pattern-parser-implementation.html#the-complete-implementation)
* [Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html#references)

# Test Frameworks
Tests for more serious projects and production code should leverage the power and support of test frameworks. There are many supporting [Unit Test Frameworks](https://en.wikipedia.org/wiki/List_of_unit_testing_frameworks). Most major programming languages have a root test framework with [xUnit](https://en.wikipedia.org/wiki/XUnit). Each programming language in the xUnit family has notations that match its specific programming language guidelines.

Test frameworks manage the testing environment. We’re still responsible for writing the tests, which are usually methods within a test class. Each test is a method, which is identified as a test via framework annotations, such as `@Test`. There are often annotations for other test related elements, such as those for common set-up and clean-up methods.

Test frameworks support test execution by allowing us to run the tests with the press of a button in our IDEs. They will execute all tests in scope, whether one test or all, and indicate which passed and which failed. Frameworks allow the entire test suite for the project to be executed in each CI/CD pipeline build.

# Test Elements
Each test focuses upon a specific SUT. The SUT may be limited to an object or set of cohesive objects. Each test confirms one aspect of a method/function in the SUT. Multiple tests are often required to confirm the entire SUT.

Ideally, we’d like to treat the SUT as a black box. The test should not know SUT implementation details. The test should not know the number of objects in the SUT. The test should only know the public method/function, i.e., the API, being invoked and the expected behavior. We should be able to [refactor](https://en.wikipedia.org/wiki/Code_refactoring) the SUT implementation without causing any unit tests to fail.

Automated tests for the SUT typically consist of three (sometimes four) parts. I will use generic names here. However, the first three parts are often called [__Arrange/Act/Assert__](https://wiki.c2.com/?ArrangeActAssert) or [__Given/When/Then__](https://en.wikipedia.org/wiki/Given-When-Then). I’ll present these practices in greater detail in further blogs with additional context as to how they apply to a test strategy.

## Set Up – I.e., Arrange/Given
We want to test the SUT in [isolation](https://jhumelsine.github.io/2024/06/14/unit-test-attributes.html#isolated) from the rest of the codebase without the SUT knowing that it’s been isolated. Isolation focuses and limits the scope of the test. When the test fails, debugging tends to be easier, since the failure often resides within the bounds of the isolated SUT.

But code rarely exists in isolation. Most code has dependencies on other elements including other components of the product, databases, file systems, internet, clocks, etc.

To isolate the SUT from its dependencies, we override these dependencies with [Test Doubles](https://en.wikipedia.org/wiki/Test_double). I’ll provide more details about Test Doubles in the next blog. For now, a Test Double emulates dependency behavior with the SUT being none to the wiser. Test Doubles tend to be small snippets of code that emulate a specific dependency behavior that’s only needed within the context of each test.

We replace production dependencies with Test Doubles mostly because Test Doubles tend to be easier to configure and execute faster in the test. We also have complete control over dependency behaviors via Test Doubles.

A test only needs Test Doubles if the flow of execution through the SUT interacts with the dependency. Different tests may have different Test Doubles emulating different behaviors depending upon the test. For tests that do not reference dependencies, Test Doubles are not needed.

Test Doubles shed a little light upon the SUT black box. The dependencies they override are SUT design details, which cannot be ignored in the test. The SUT is still a black box, but it’s a box for which the test knows a few details about the exterior of the box. It’s like electronic equipment. We may not know the circuitry in an electronic component, but we do need to know how to plug the stereo components together.

The SUT’s design and implementation usually need to accommodate a Test Double easily that can override the production dependency. This means that the SUT should not be tightly coupled to its dependencies. I’ll blog more on this in the future.

When tests require a lot of Test Double configuration, and especially if it’s complex, then this may be an indication that SUT may be a good candidate for refactoring or redesign.

## Execution – I.e., Act/When
A test executes the SUT’s method or function. This is often the smallest part of the test, usually one statement, but it’s the most important part. This is where SUT is invoked and executed. A little or a lot can occur in the SUT before it returns.

## Confirmation – I.e., Assert/Then
Without confirmation, we can only know that the SUT doesn’t crash when it is executed. The test confirmation ensures that the SUT is operating as expected. The test must confirm the SUT’s operations without peering into its black box.

There are at least three mechanisms to determine whether the SUT is working as expected without cracking the black box open. A test may use one, two or all three mechanisms:
* __Return Value__ — Many methods/functions return values. Assert that the value returned from the SUT is the expected value. For example, if a function reverses a String, then assert this behavior via a statement like: `assertEquals("cba", reverse("abc");`
* __State Change__ — The test may invoke state changes in the SUT. Assert the state changes via an SUT accessor method. I often return the aggregated state of an object via its `toString()` method and assert the returned String. This is useful when multiple attributes change state in the test.
* __Dependency Interaction__ — The SUT may interact with its dependencies … I mean with its Test Doubles. Verify those interactions with Spy Test Doubles. A Spy records its interactions with the SUT and returns an account of those interactions via a query. E.g., a Database Spy can verify that the SUT requested data persistence via a Database API call when data persistence expected behavior. Dependency verifications via Spies peek a bit behind the SUT black box veil much like the Test Double set up can.

A test passes when all assertions and verifications pass; otherwise, the test fails.

If a test requires many assertions and verifications, then this may be an indication that the SUT is taking on too much responsibility, and it may be a good candidate for refactoring or redesign.

## Clean Up
Clean up is the fourth element that’s not featured often.

Higher-level automated tests may require clean up. If any shared resources are allocated in the set up, then they should be released in a clean up section.

Unit tests don’t tend to require clean up, since unit test resources tend to be ephemeral. Test Doubles tend to clean up automatically.

## Bugs May Linger
Passing tests does not guarantee bug free code.

_Testing can show the presence of bugs, but not their absence!_ — Edsger W. Dijkstra

A test case scenario may be missing. A test may need additional assertions or verifications. I’ll address some of these issues in subsequent blog posts.

# Summary
This is the first step toward making tests first-class citizens. These are foundational elements automated tests. Subsequent blogs will add more context with these automated test elements.

# References
* Previous [blog references](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#references)
