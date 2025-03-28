---
title: How do you know if your test code is really testing your code?
description: Mutation testing tests your tests
---

<img src="https://live.staticflickr.com/538/19402967595_48500c3be5_b.jpg" alt="The Night Watch" title="Image Source: https://www.flickr.com/photos/snarfel/19402967595" width = "50%" align="center" style="padding-right: 35px;">

# Introduction
[_Quis custodiet Ipsos custodes?_](https://en.wikipedia.org/wiki/Quis_custodiet_ipsos_custodes%3F) – [Juvenal](https://en.wikipedia.org/wiki/Juvenal)

___Who watches the watchers?___

Imagine you deploy a new feature, and despite a fully passing test suite, a critical bug makes it into production. How did this happen? The answer might lie in how your tests are written; not just how many you have.

Tests are code. How do we know that our tests provide accurate results? What if our test code has errors? 

<img src="/assets/SkepticalFry.jpg" alt="Skeptical Fry" title="Image Source: [https://www.flickr.com/photos/snarfel/19402967595](https://imgflip.com/memegenerator/11327935/skeptical-fry)" width = "35%" align="center" style="padding-right: 35px;">

How do we test our test code? How do we test the tests that our test code? How do we test the tests that test the tests that our test code?

Mutation testing helps ensure that your test suite is actually doing its job—by trying to break it.
 
# TDD
Practicing [Test-Driven Design](https://jhumelsine.github.io/2024/07/15/tdd.html) (TDD) should minimize concerns about your test suite.

## Tests the Tests
With TDD, we start with a failing test before implementing the feature. This ensures that we don’t create a false-positive test by accident that passes regardless of the implementation.

Then we provide an implementation, with a quick possibly hardcoded known correct implementation, that passes the test. This ensures that the test passes when it should.

The first two steps of TDD focus upon a correct and accurate test. We can then safely refactor the code making it cleaner knowing that any accidental diversions in behavior will be caught by the tests.

## Double-Entry Bookkeeping
<img src="https://www.financestrategists.com/uploads/Dual-Aspect-Concept-of-Accounting-Example-2.png" alt="Double-Entry Bookkeeping" title="Image Source: https://www.financestrategists.com/accounting/accounting-concepts-and-principles/dual-aspect-concept-of-accounting/" width = "35%" align="right" style="padding-right: 20px;">

Test and implementation confirm each other. Behavior resides in two independent places. The test defines the behavior specification, and the code implements the behavior. Both test and implementation are executable, and any test/implementation behavior inconsistencies should be identified when tests are executed in CI/CD pipeline builds. A change of behavior to one without a corresponding change to the other is likely to cause a failure.

This is a form of [double-entry bookkeeping](https://en.wikipedia.org/wiki/Double-entry_bookkeeping) where a mistake on one side of the ledger is caught by the inconsistency with the other side of the ledger. Bob Martin points out the similarities between double-entry bookkeeping and TDD in his [Excuses](https://blog.cleancoder.com/uncle-bob/2017/12/18/Excuses.html) blog entry.

## Alas, it’s not perfect
[Testing can show the presence of bugs, but not their absence!](https://www.goodreads.com/quotes/506689-program-testing-can-be-used-to-show-the-presence-of) — Edsger W. Dijkstra

The test/implementation partnership only works when tests accurately define the behaviors that the code implements. Tests pass by default unless a behavior enforcing assertion or verification violation catches the problem. A test might be missing an invariant assertion or verification that allows undesired or unexpected behavior to pass through undetected.

It’s possible to achieve 100% code coverage without a single assertion or verification. This can happen when there’s pressure from upper management to reach a code coverage goal, or an unexperienced developer resolves a blocking failing test by removing the failing assertion or verification. Haphazard false-positive tests provide code coverage, but all they provide is a false sense of security.

It would be better for these false-positive tests if they had never been created. Not only do they test nothing, but they make it more difficult to ascertain which parts of the codebase are actually being tested.

# Mutation Testing
Mutation Testing is a tool in helping us identify false-positive tests and untested code.

## Prepping for a Demo
When I was first learning about TDD, I added unit tests to my Design Pattern Examples, like a code kata. It was a way for me to become familiar with TDD practices with a domain in which I already had familiarity and comfort.

I was presenting design pattern lunch-and-learn sessions to our summer interns at around the same time. I was pleased with my recent unit test results, and I wanted to share my newfound unit test knowledge with them. I thought I'd demonstrate the power of unit testing with my recent design pattern tests by introducing an error manually and watch one of my unit tests fail.

I tried the following before showing this to them:
* I randomly chose a design pattern
* I randomly chose a file
* I randomly chose a method
* I randomly chose a functional line, which had code coverage

I commented out the line and reran my unit test cases ... and they all passed. What?! I went back to the file I had modified. Why had removing that line allowed the test cases to still pass? I thought through the code, and I realized that I was missing an assertion invariant. I added the missing assertion, which failed the test. I put the randomly commented out line back in, and the new assertion passed, which confirmed that the randomly selected line was important.

I had fixed my test case, but this exercise left me with a queasy feeling in the pit of my stomach. What else had I missed? I had thought that I had sufficient code coverage and assertions. Was this my only missing behavior test specification? I had a couple thousand lines of design pattern code. What were the odds that I found the _only line_ that didn't have sufficient testing?

How do I know whether my test cases are correct and complete? Tests are code too after all. Do I write test cases for my test cases? And what about the validity of those test case confirming test cases? I had been retrofitting test cases into design pattern examples, which had been previously implemented. Had I used TDD, I suspect I would not hit this problem. When test and implementation code is written together, the two tend to reinforce each other. But how could I be sure?

## Pure Serendipity
The software gods may have filled me with dread that day, but the next day they smiled down upon me. I was working my way through some of Bob Martin's [Clean Coders](https://cleancoders.com/) videos on [O’Reilly](https://www.oreilly.com/), and the day after my unsettling testing doubt, I watched: [___Life, The Universe, and Everything: Part 2___](https://learning.oreilly.com/videos/clean-coder-clean/9780134843803/9780134843803-CODR_E42_P2/).

Here's a short snippet from the beginning of the transcript. (Uncle Bob portrays himself as well as all the _Humorous Characters_ including: a __Space General__  barking out orders, __Danny__ the flighty Microsoft developer, __Ruby__ the hippy-dippy Ruby developer, __Minecraft Guy__ living in the pixelated application and several _Star Trek_ characters including: __Lieutenant Commander Data__ and __Mr. Spock__. Bob really likes to dress up as other characters in his videos.)
>__Uncle Bob:__ _Welcome, welcome, to part two of Life, the Universe and Everything, episode 42._
>
>__Space General:__  _... you will fearlessly and relentlessly improve your code! You will never allow it to degrade!_
>
>__Danny:__ _Oh gosh, Uncle Bob, I don't understand why you think [modifying this code] is risky. After all, you got the coverage up to 100% of methods, and 96% of lines, and 100% of branches. I mean, what else could you have done? That module is covered._
>
>__Uncle Bob:__ _Slow down, guys. Cool your jets. Just what do you think those high coverage numbers actually prove?_
>
>__Minecraft Guy:__ _They prove you tested everything._
>
>__Uncle Bob:__ _No, that's not what they prove at all. Let me show you. I have now removed every assertion from this test. And now, let's run the test coverage! And what do you think's gonna happen here? Well, hmm hmm hmm, look at that! All the tests passed! Oh, we've got really high coverage here, don't we? 88% of the methods, 94% of the lines, here, let's look at the source file. Oh look, it's all covered, isn't it? We're perfectly safe! There's nothing wrong! Except we didn't assert anything. That's really the problem here, isn't it? Do you see the problem?_
>
>__Danny:__ _Yeah, Uncle Bob, I do. I mean, even if you take all the asserts out of the tests, the tests still execute and pass, and the coverage tools still reports high coverage._
>
>__Uncle Bob:__ _And that should worry you, because it implies something pretty frightening._
>
>__Mr. Spock:__ _The fact that the unit tests pass even when coverage is high does not mean that the code is tested._
>
>__Lieutenant Commander Data:__ _In fact, it only proves that the code was executed._
>
>__Uncle Bob:__ _And that's a really big problem._
>
>__Minecraft Guy:__ _Big problem, so like, what do we do?_

Bob was describing my scenario from the previous day. I had passing test cases. I had high code coverage. But I was missing an assertion. But how would I know that I was missing it? Test drivers assume that tests pass until a violated assertion reports otherwise. What if your test case contains two assertions that confirm two invariants, but the scenario really involves three invariants? You could break the third invariant without knowing it. What if a naïve developer got a unit test case to pass by removing an assertion that always fails?

I had found my missing assertion only by dumb luck, and even then, the test cases always passed whether that line in question was present or not. I only realized I had a problem when I expected to see a failed test case, but they had all passed. Randomly touching source code hoping to find a failure isn't scalable, and it's not very safe either. What if I forget to change it back?

Let's return to the video transcript:
>__Uncle Bob:__ _So given that the asserts are all in place, our contention is that this code is tested. Now to prove that, what we ought to do is modify the code and see if it still passes the test. So let's just get rid of that little bit of code and see if the tests still pass. Oh heavens, the test fails! Well alright then, that means that that code must be tested, let's put it back. Let's take this part of the test out, or that part of the code out, and run our tests again. Nah, they still fail, excellent. So now we know that this code is necessary. Let's comment out another block of code. No, that fails too, heavens! Alright, well, I guess this code here must be necessary. So let's make sure our tests all pass now. So, what I'm doing there, is I'm changing the code, and every time I make a change to the code, the tests fail. This shows that the code, is being tested. This kind of testing is called __mutation testing. If the unit tests really cover everything, then any semantic change to the code should cause those tests to fail.___
>
>__Ruby:__ _Wait, wait wait wait, are you trying to tell us, that you want us to walk through the code, changing each line of code and then running all the tests to make sure they fail?! I mean, that's just getting a bit nuts._
>
>__Uncle Bob:__ _Oh no, I'm not telling you [that] you should do that manually. Heavens, that would be a nightmare. There are tools that will do this for you. They're called mutation test tools. Here's one called __Pitest__, and you can get it at [Pitest.org](https://pitest.org/), it works with Java. What it does is it runs your test suite over and over again, and every time it runs it, it makes another semantic change to the byte code of your application, not the source code. And then it remembers which semantic changes caused the tests to fail, and which caused them to pass._

## Mutation Test Frameworks
<img src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/38cb0567-1f6d-4e22-ac04-42f48ee60ba0/d1kizwb-f628e672-700b-4b2f-ad85-648b6c12f9fc.jpg/v1/fit/w_423,h_600,q_70,strp/mutant_chiwawa_rabbit___detail_by_claytonbarton_d1kizwb-375w-2x.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7ImhlaWdodCI6Ijw9NjAwIiwicGF0aCI6IlwvZlwvMzhjYjA1NjctMWY2ZC00ZTIyLWFjMDQtNDJmNDhlZTYwYmEwXC9kMWtpendiLWY2MjhlNjcyLTcwMGItNGIyZi1hZDg1LTY0OGI2YzEyZjlmYy5qcGciLCJ3aWR0aCI6Ijw9NDIzIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmltYWdlLm9wZXJhdGlvbnMiXX0.jmw1WixGZ3szua9o5CxQnxZvMgQBTIILqaOYx2FxhAg" alt="Mutant" title="Image Source: https://www.deviantart.com/claytonbarton/art/Mutant-Chiwawa-Rabbit-Detail-94944827" width = "25%" align="right" style="padding-right: 20px;">

The timing for Bob’s video could not have been more perfect to soothe my testing malaise. Pitest is a free plug-in on Eclipse and other IDEs (look for PIT in the marketplace). Pitest is for Java. Other languages may have their own mutation test frameworks, such as [Stryker Mutator](https://stryker-mutator.io/) for JavaScript, C# and Scala.

Here’s a summary of what Pitest does. I’m sure other mutation testing frameworks work similarly:
* It _mutates_ your byte-code by flipping logic, skipping lines, altering return values, etc. By changing the byte-code, it changes behavior and therefore injects a bug, known as a mutation.
* It runs your unit tests on the mutated byte-code. If a test fails, then your unit test has found that mutation and _killed_ it. This is what you want. A behavior change in your code was detected by your test suite.
* If an assertion does not fail, then your test suite has failed to kill the mutation. This is not what you want. A change in your code was completely ignored by your current unit tests. Pitest only informs you of a potential problem, but not how to resolve it. You'll need to do a little analysis to address it:
    * Are you missing a unit test case or just an assertion in an existing unit test case?
    * Is a test case incorrect with its Given/When/Then paradigm?
    * Do you have redundant code that accommodates the mutated line?
    * Was the mutated line dead code that's never executed or is not covered by the unit tests?

A uncaught mutation does not mean that your implementation is incorrect. It only means that the behavior within your implementation does not have a test specification.

The correction that allows the mutation to be found could be in your test code, your implementation or both.

## Double-Entry Bookkeeping Revisited
Previously I wrote:
> The test/implementation partnership only works when tests accurately define the behaviors that the code implements. Tests pass by default unless a behavior enforcing assertion or verification violation catches the problem. A test might be missing an invariant assertion or verification that allows undesired or unexpected behavior to pass through undetected.
>
>It’s possible to achieve 100% code coverage without a single assertion or verification. This can happen when there’s pressure from upper management reach a code coverage goal, or an unexperienced developer resolves a blocking failing test by removing the failing assertion or verification. Haphazard false-positive tests provide code coverage, but all they provide is a false sense of security.

Mutation testing helps to address this. If the test/implementation partnership is out of alignment because of missing assertions or verifications, then there’s a good chance that mutation testing will identify the test/implementation misalignment.

__Mutation testing doesn’t test your implementation. Mutation tests your tests.__

I installed Pitest. It found a memory/threading problem in my unit test code immediately. It would not even execute until I fixed it. Once that was fixed, it only reported uncaught mutations in code that tended to be at the edges of the implementation, such as [Adapters](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html), which don’t tend to have much code coverage, since they tend to be coupled to external dependencies.

Pitest takes longer to execute than the unit tests. But it still wasn't bad. I think it took about 3 or 4 minutes to test several thousand lines of code. Kick off Pitest and then take a coffee break.

Pitest won't guarantee that your unit test assertions are 100% complete, but it will help you identify any that may be missing and maybe a few other problems.

[_My favourite thing to do when devs tell me that TDD is overrated is to run mutation testing on their repo. When they see a huge army of surviving mutants buzzing around in their code base, they have no explanation as to why do their tests remain silent._](https://x.com/alexbunardzic/status/1205917249745256453) — Alex Bunardzic

Having found this new exciting test tool, I ran Pitest on my project, and I learned:
* I could only execute Pitest on the entire project repo. I could not scope the test to an individual folder or class as I could do with my unit test framework. There may have been a way to do this, and I couldn’t find it on the GUI, or maybe it’s been added since.
* Since it executed against the entire repo, it took an extraordinary amount of time to complete. Therefore, if Mutation Testing can only be performed upon the entire repo, it may make more sense add Mutation Testing to the pipeline builds.
* The project did not have good code coverage at the time. Any non-covered mutated lines would not be killed, since there are no unit tests that would find those mutations. Since coverage was so low, Pitest reported a huge number of surviving mutants. It was impossible to identify useful mutations, unless the Mutation Testing Framework provides reports that distinguished uncaught covered mutations from uncovered mutations. I feel that mutation testing is only useful when the repo already has good test coverage. Once you have decent coverage, then mutation testing will evaluate how effective it is.

# Summary
Mutation testing isn't just about breaking your code—it's about strengthening your tests. By identifying gaps in your test coverage and ensuring your tests are truly effective, this technique helps developers build more robust, maintainable, and resilient software. Incorporating mutation testing into your workflow can lead to higher-quality code and greater confidence in your test suite.

# References
Here are some free resources:
* [Wikipedia Mutation Testing](https://en.wikipedia.org/wiki/Mutation_testing)
* [Awesome Mutation Testing](https://github.com/theofidry/awesome-mutation-testing) - GitHub repo with Mutation Test resources
* [A Comprehensive Guide to Mutation Testing in Java](https://bell-sw.com/blog/a-comprehensive-guide-to-mutation-testing-in-java/) - blog by Pasha Finkelshteyn
* [State of Mutation Testing at Google](https://research.google/pubs/state-of-mutation-testing-at-google/) - by Marko Ivankovic for
Proceedings of the 40th International Conference on Software Engineering 2017
* [mutation testing](https://www.techtarget.com/searchitoperations/definition/mutation-testing) - blog definition by Alexander S. Gillis
* [Mutation Testing: Automate the Search for Imperfect Tests](https://www.oracle.com/corporate/features/mutation-testing.html) - Oracle article by Henry Coles
* [Better testing through mutations](https://www.thoughtworks.com/en-us/insights/podcasts/technology-podcasts/better-testing-through-mutations) - Thoughtworks Technical Podcast
* [5 Best Mutation Testing Tools](https://www.youtube.com/watch?v=c8hBoc3akLU) - Video by ZAPTEST
* [PITest](https://pitest.org/) - Mutation Test Tool for Java
* [Stryker Mutator](https://stryker-mutator.io/) - Mutation Test Tool for JavaScript, C# and Scala
* and for more, Google: [Mutation Testing](https://www.google.com/search?q=mutation+testing)
* and: [Mutation Test Tools](https://www.google.com/search?q=mutation+testing+tools)
  
Here are some resources that can be purchased or are included in a subscription service:
* ___Life, The Universe, and Everything: Part 2___ ([O'Reilly](https://learning.oreilly.com/videos/clean-coder-clean/9780134843803/9780134843803-CODR_E42_P2/) and [Clean Coders](https://cleancoders.com/episode/clean-code-episode-42-p2))
