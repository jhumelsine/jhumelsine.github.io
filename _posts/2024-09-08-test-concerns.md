---
title: DRAFT – Testing Concerns
description: Test concerns may be a result of previous bad test experiences. There are ways to accommodate them.
unlisted: true
---

# Introduction
I presented some benefits of automated testing in the previous [Testing Benefits](https://jhumelsine.github.io/2024/08/30/test-benefits.html) blog, but I’m sure there are readers who still have concerns based upon their previous experiences with automated tests.
I’ll address some common concerns with some suggestions on how to accommodate them.

## I Can’t Write Tests Unless I Write The Code First. How Will I Know What To Test?
This is probably the most common reason for not writing tests first. Developers feel like they need to write the code before they can test it. This is the practice that's been familiar to them since the days when they first learned to code. They may still feel that the purpose of the test is to exercise and confirm the code. The purpose of the test is to [specify behavior](https://jhumelsine.github.io/2024/08/30/test-benefits.html#tests-are-codified-specifications). Confirmation is just a nice benefit.

Let’s consider this a bit. How do developers even know what to code? I suspect they have some ideas based upon requirements, user stories, conversations, etc. Rather than expressing those ideas in code, can they express those ideas in words? Can they write an idea down as a sentence or a short paragraph? Can they write a test that defines that idea before trying to code it? I suspect that with some practice, they can.

Don’t try to tackle the entire feature in one test. Write one test that confirms one aspect of behavior. Implement that behavior until the test passes. I like to start with the simplest cases first, i.e., the degenerate and exception cases as described in [Degenerate, Edge and Boundary Tests](https://jhumelsine.github.io/2024/08/08/bdd.html#degenerate-edge-and-boundary-tests). This builds momentum toward specifying more common case behaviors.

Don’t try to tackle too many test cases at the same time. Practice [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html) (TDD) by developing one test and its implementation at a time. If your head becomes filled with too many test case scenarios, jot them down a description of the behaviors separately and develop them one test at a time.

Learning to define behavior via a test before implementing it can take some practice. It may feel awkward at first. Practice the techniques using [Code Katas](https://jhumelsine.github.io/2024/07/15/tdd.html#coding-katas).

# Tests Are Too Hard To Write
I suspect this is based upon bad experiences of having to write tests for existing code. This can be hard. I’ve experienced it myself when writing tests for legacy code (blog TBD) with which I was not intimately familiar.

A test reflects the implementation. If the implementation is complex, ugly, nasty, etc., then the test will be complex, ugly, nasty, etc. Legacy code tends to be complex, ugly, and nasty. If the implementation is clean, then the test will be clean.

If you write the test before the implementation, then you’re more likely to write a clean test, and as a result your implementation will be cleaner too. If you write a complex, ugly or nasty test before implementation, then you’re going out of your way to make yourself miserable.

Another possibly way to make tests hard to write is in attempting to write one test that covers as many behaviors as possible for the entire feature. I used to do this myself. These tests, which tended to come after the implementation, were difficult to write, and they were brittle. In a few weeks, the tests started failing as the feature matured as I made adjustments. The tests were too difficult to maintain, so I abandoned them.

Specify only one aspect of behavior per test. Since a method might handle different behaviors, several tests will be required to ensure each aspect of behavior is specified and covered.

# Writing Tests Is Too Boring
The fun is in writing code and seeing it run. Writing tests is boring, especially if written after the implementation is complete. After you're done with the coding full, all you’re left with is drudgery of writing the tests.

Rather than procrastinate the writing of boring tests, write one test before you implement its behavior. You don’t have to write the complete set of tests at once, nor do you want to.

Write a test. Implement enough to pass the test. Repeat. See: [TDD](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development).

This weaves test and implementation creation together. You still get that jolt of satisfaction when a test passes. I wager that it’s even more fun, since not only is the most recent test passing, but all the previous tests are still passing. You can confirm that in addition to getting new behavior working in the implementation, you also haven’t broken any previous behavior in the process.

# Writing Tests Takes Too Much Time. I’m Paid To Write Code
In my experience, each line of implementation requires about three to four lines of test code. That’s a lot of code that’s never executed by the customers. Many developers, and some managers, may not view this as worth the time.

We’re paid to deliver code, not tests. Really? We’re paid to deliver __working__ code. We have three basic choices before submitting our code changes:
* No Tests
* Manual Tests
* Automated Tests

## No Tests
I doubt that many developers adopt the __No Test__ philosophy. Years ago, code written by one of our contractors failed often in QA. When asked if he was testing his code, his response was, “Of course I tested my code. It compiled.” His contract was terminated soon thereafter.

That’s an extreme case, but how many of us can honestly state that we haven’t done something similar? I know I’ve delivered code that I didn’t fully test. Michael Feathers refers to this technique in ___Working Effectively With Legacy Code___ as [__Edit and Pray__](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#working-effectively-with-legacy-code).

While not testing one’s code saves time in the short run, it probably doesn’t save too much time overall. Consider the contractor’s practice. How much QA time was required to find his faults plus the additional time to cycle back to fix them? What about the faults that QA didn’t find?

## Manual Test Code
For much of my career my projects had dedicated QA teams that tested our system. Developers would manually test recent updates, but we rarely tested the entire feature being updated. QA would catch any of our bugs that slipped through, right? Right?

_When programmers do their jobs, testers find nothing._ ― [Robert C. Martin](https://x.com/unclebobmartin/status/541876357320671232)

Our QA teams were well organized with detailed test plans, which were often manually executed. Running them took time; sometimes more time than would fit into their schedule. QA might only run a subset of their tests, especially for patch updates.

A full regression test of the system could take weeks to complete and evaluate. There were usually a substantial number of faults identified. Tickets would be created and assigned to developers, and the whole cycle would begin once more.

I took a yearlong rotation on the QA team. I got a taste of my own medicine. I felt the pressure of being the last line of defense before subjecting our customers to the system. Development was submitting updates constantly. I felt like I could barely keep up with my testing responsibilities. Sometimes it felt futile. Why devote so much effort to test the latest version of the system when I knew another one is coming soon? And it was soul crushing toil since we would be running the same boring manual tests day after day. 

I learned lessons during that year:
* I never wanted to spend my career in QA.
* I gained more respect for the squeeze the QA team felt as the release date approached.
* QA can’t find every issue.
* Manual testing doesn’t scale.

I consulted more with the requirements author during that rotation than I did with the developer. I needed  to understand what the system should do rather than test what the development team delivered. This planted the first seed in my brain that testing was about behavior and not implementation. It would take years before that seed would fully mature and bear fruit in my mind.

Near the end of my career QA teams started to shrink and even disappear. More testing responsibility was placed upon the developer. At first, I balked at this, not because it was more work, but because it didn’t make sense to me. I already knew that that my implementation worked. I needed the testers to confirm that my implementation matched the requirements. This was when I still viewed tests as exercising the implementation rather than specifying the behavior.

No matter where you try to place them in the software development process, manual tests consume a huge number of resources, and they don’t provide consistent results.

## Automated Tests
As more test responsibility was being shifted to developers, developers had to spend much more time performing their own manual tests or spend that time creating automating tests.

I would argue that taking the time to write the automated test in conjunction with the implementation using TDD and BDD is not a cost, but an investment:
* Automated tests complete quickly, possibly seconds, whereas manual tests require more time, possibly minutes or hours. 
* Automated tests return pass/fail results, whereas manual tests often require additional analysis.
* Automated tests run consistently, whereas manual tests are prone to human error.
* An entire suite of automated tests can be run, often in the background, whereas manual tests require dedicated human resources.

Preferring manual testing over automated testing reminds me of this Neal Ford quote: _When humans do repetitive tasks on behalf of computers, the computers get together late at night and laugh._

# Creating Tests Slow Me Down
_Creating tests slow me down, especially when I’m in the flow._

Flow state is definitely fun. I don’t deny that. But I feel there’s more than one way to achieve flow. Once you get comfortable with TDD, then you start to get into the flow once more. The __Red-Green-Refactor__ TDD cycle is constantly adding new code for specified behavior that’s confirmed at the touch of a button.
And once legacy code (blog TBD) has enough test scaffolding confirming its behavior, then you can fly through refactoring confidently.

I think automated tests made me faster. I didn’t have to pause and contemplate every change to convince myself that it worked. My test suite would do that for me in seconds rather than me having to spend minutes thinking about what I had just updated.

# Tests Require More Maintenance Than The Code
I experienced this. My initial attempts at automated testing were brittle. They stopped working a few weeks after their initial creation. I tried to keep them up to date to match the implementation, but it became too much effort, and I abandoned them.

My tests were exercising the implementation. They were not specifying the behavior. The tests had other issues too but being focused upon the implementation was probably their core issue.

Tests that are written after the code tend to exercise implementation more than confirm behavior specification. When tests exercise implementation, they have a dependency upon the implementation. When the implementation is changed due to a new feature being added, a bug being fixed or code being refactoring, then these brittle implementation dependent tests often fail.

If writing the test before the implementation, then there is no implementation to shape the test. The test is more likely to be based upon behavior and less brittle to subsequent implementation updates.

If tests are scoped to one aspect of behavior per test, so that there are multiple tests that specify behavior as a whole. If an aspect of behavior changes, it’s most likely to only affect the subset of tests that specify that behavior aspect. Hopefully the impact is limited to only one test. You may not have to modify the test either. It may be easier to create a new test for the updated/new behavior aspect and remove the outdated test.

# TDD, Where Did It All Go Wrong?
This provocative header comes from Ian Cooper's DevTernity 2017 presentation: [TDD, Where Did It All Go Wrong?](https://www.youtube.com/watch?v=EZ05e7EMOLM) He argues that we've overdone TDD. He noticed that projects that embraced TDD ran into all sorts of problems when they attempted to refactor their code. And the problems didn't reside in the source code. The problems resided in the unit test cases. They'd refactor the code, and half of the test cases would break. In Cooper's mind, refactoring should update the implementation without changing its behavior, which I believe as well. Unit test cases should rarely fail due to refactoring. What was wrong?

Cooper argues, and I think he's correct, that too many unit test cases were designed to execute the implementation and not specify the behavior. This gets a little tricky to distinguish, since implementation contains behavior and behavior comes from implementation. 

Cooper's presentation is worth watching, but to save you some time, here are his core ideas. If you’re up to date with this testing blog series, you’ll see that I’ve covered most of these already:
* Avoid testing implementation details, test behaviors
* The trigger to implement a new test is when one implements a new requirement, not an implementation detail
* Implementation based tests are brittle
* Test the stable contract public API; it is the first user of that API. Does the API make sense?
    * Don’t test internals
    * Don’t make everything public to test it
    * Don’t use test doubles to _confirm_ implementation details
    * Use test doubles so that tests can be isolated and fast
* Write tests to cover the user cases or user stories
* Use __Given-When-Then__ model
* The software under test is not a class; it’s the _exports_ from a module – its façade
* Do not write tests for implementation details – these change
* Only write tests to cover implementation details for better understanding the implementation for refactoring, but then delete or deactivate tests when done

I received a review comment with a concern about: ___Don't test internals___. Specifically, it may be too difficult to test all scenarios via the public API of a complex system. There are too many cases and the set up would be too complex. I don't disagree with that concern.

I think Cooper's intent about _internal testing_ was: 
* _Internals refers to the encapsulated implementation. Testing internals implies that the test has knowledge of and dependency upon the encapsulated  implementation. Subsequent refactoring would have a high probability of breaking tests that test internals. I.e., they would be brittle tests._
* _Don't change the implementation access so that its internals can be tested. I.e., don't make private methods public so that they can be tested._
* _Create internal implementation only when it's needed to satisfy a test that specifies behavior._

As for ___a complex system___, that suggests to me a design that is comprised of multiple classes. It will be difficult to test all scenarios in that configuration. Nor do we want to. There are different layers of testing will help manage this, which I'll address in a future blog (TBD). 

I view the distinction among the layers of testing as the scope and boundary of the software being tested:
* The smallest scoped is bounded to an individual class.
* The medium scoped is bounded to a set of classes.
* The largest scoped is bounded to the entire system. 

Each layer of testing assembles more components of the system. Each layer has of testing relative advantages and disadvantages compared to the other layers. The smallest scoped testing layers define and confirm the nuts and bolts. The larger scoped testing layers define and confirm that the nuts and bolts fit together.

Smaller scoped testing confirms parts in every scenario imaginable. These tests are less expensive and complete quickly.

Larger scoped testing confirms those parts work together. These tests are more expensive and take longer to complete.

This is where the art of design becomes important. Regardless of the scope and boundary, most Software Under Test will have dependencies. Those dependencies may be replaced by [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html) in the tests. While refactoring may not affect tests and test doubles, redesign may. If we can keep test double dependency to a minimum through good design, then we will have fewer tests breaking during redesign.

# Summary
Concerns associated with automated testing may be a matter of education and practice. The concerns may be a result of not knowing or not following good test practices. This was the case for me when I tried to create automated tests without understanding how best to create automated tests.

Here are some test smells that may indicate good test practices were not followed:
* Tests that take too much time to complete. They are probably coupled to an external dependency.
* Tests that are flaky. That is, they are non-deterministic, sometimes they pass. Sometimes they fail.
* Tests that exercise entire operational scenarios rather than a single behavior.
* Tests that don't test anything. That is, they are false positives.
* Tests that are brittle. They exercise implementation and do not specify behavior. They are tests that start to fail when the code is refactored.
* Tests that are complex. They have lots of Test Doubles, lots of assertions, etc. This reflects problems with the implementation. Test complexity, ugliness, etc. is usually a reflection of implementation complexity, ugliness, etc.

# References
* Previous [blog references](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#references)
* [TDD, Where Did It All Go Wrong?](https://www.youtube.com/watch?v=EZ05e7EMOLM) - Ian Cooper's DevTernity 2017 presentation. I learned of this presentation from a [Tweet](https://x.com/unclebobmartin/status/1032405401009041409) on Twitter/X from Bob Martin.
* [TDD Revisited](https://www.youtube.com/watch?v=IN9lftH0cJc) - Ian Cooper 2023 presentation.
* [TDD Harms Architecture](https://blog.cleancoder.com/uncle-bob/2017/03/03/TDD-Harms-Architecture.html) - blog by Bob Martin
* [When TDD Doesn’t Work](https://blog.cleancoder.com/uncle-bob/2014/04/30/When-tdd-does-not-work.html) – blog by Bob Martin
* [Test Contra-variance](https://blog.cleancoder.com/uncle-bob/2017/10/03/TestContravariance.html) -blog by Bob Martin
* [When Test Driven Development Goes Wrong](https://www.youtube.com/watch?v=UWtEVKVPBQ0) – video by Dave Farley
