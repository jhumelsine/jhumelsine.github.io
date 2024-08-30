---
title: Testing Benefits
description: Spoiler Alert – It’s not really about testing the code
---

# Introduction
In previous [blog posts]( https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#summary), I have described the testing process. I have mentioned some of the benefits and concerns in passing.

This blog post will list some of those benefits here, and I’ll cover concerns in the next blog (TBD).

__Spoiler Alert: Testing the code isn’t the main benefit.__

Here are some of the benefits I perceive with [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html) (TDD) and [Behavior-Driven Development](https://jhumelsine.github.io/2024/08/08/bdd.html) (BDD). These are not all exclusively my original ideas. I learned many of these from Bob Martin's TDD videos, which are [referenced](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#references) in my initial testing blog.

Many of these benefits are variations on the same theme: __A failing test indicates an inconsistency between the behavior specification test and the implementation.__ The variations listed below are the different contexts in which this can happen.

# Tests are Codified Specifications

<img src="https://www.thebluediamondgallery.com/handwriting/images/specification.jpg" alt="Specification" title="Image Source: https://www.thebluediamondgallery.com/handwriting/s/specification.html" width = "35%" align="right" style="padding-right: 20px;">
 
I’ve mentioned this several times in previous blog posts, and I’m sure I’ll mention it again. __The purpose of tests is not to confirm the implementation, but to specify behavior.__ Taken from [Behavior-Driven Development]( https://jhumelsine.github.io/2024/08/08/bdd.html#behavior-driven-development):
> __BDD tests specify behavior for the [Software Under Test](https://en.wikipedia.org/wiki/System_under_test) (SUT), whose implementation is challenged and confirmed each time the tests are executed.__ The consistency between the behavior specification defined in the test and the observed behavior implemented in the code is confirmed each time the tests are executed.

And in [What Is Behavior](https://jhumelsine.github.io/2024/08/20/behavior.html#what-is-behavior):
>With BDD, we specify behavior and codify it within a test, which becomes a living dynamic specification that is reconfirmed with each test execution.

Throughout most of my career, requirements were defined in natural language and stored in three-ring binders, Word Documents, PowerPoint Slides and of course our favorite, Jira.

Sometimes these requirement documents extended beyond the behavior requirements by dictating architecture and sometimes even defining the design.

Natural language is interpreted in the mind of the reader. The reader’s interpretation may not align with the writer’s intent. As [Alan Greenspan](https://www.goodreads.com/quotes/9347-i-know-that-you-believe-you-understand-what-you-think), former Chairman of the US Federal Reserve, famously said: _I know that you believe you understand what you think I said, but I'm not sure you realize that what you heard is not what I meant._

There’s no internal consistency checking in a natural language document unless someone catches it in review. Subsequent requirement updates to the document might not make it into the code.

These issues disappear when behavior specifications are codified in the form of automated tests. Test defined behaviors cannot be vague or ambiguous. If tests specify conflicting behavior among themselves, then when executed against the implementation, at least one will fail. Additional behavior updates added as automated tests will fail until implemented, meaning they won’t be overlooked.

This clarity hinges upon well implemented automated tests that are clear and concise in specifying behavior. [__Given-When-Then__](https://en.wikipedia.org/wiki/Given-When-Then) structure is one means to achieve this by documenting that:
* __Given__ the state of the system before the SUT is executed
* __When__ the SUT is executed
* __Then__ this is the state of the system after the SUT has been executed

Developers should execute and confirm automated tests associated with their code changes every several minutes to ensure that the specification tests and implementation don’t diverge from each other for too long.

The complete suite of automated tests for the system should be executed and confirmed via the CI/CD pipeline upon each commit pushed to the repository.

When a specification test fails, it means there’s an inconsistency between the specification and the implementation. The implementation may violate the specification. Or the specification may be out of date. Resolution will depend upon context.
But most importantly, the developer will almost immediately know when there’s an inconsistency.

# Tests are Experiments
Testing cannot prove that our code is correct especially for large and complex software projects. We can use the [Scientific Method](https://jhumelsine.github.io/2024/08/08/bdd.html#scientific-method) to gain confidence in our code to determine when it is good enough.

Specification tests define behavior, but we should also consider tests that challenge behavior. Tests are experiments. We should always be thinking of ways to specify behavior in a test that challenges the implementation and attempts to make it fail. When we can no longer create failing tests, then our code is probably good enough to release.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fa/Suvorov_by_Schteiben.jpg/741px-Suvorov_by_Schteiben.jpg" alt="Alexander Suvorov" title="Image Source: https://en.wikipedia.org/wiki/File:Suvorov_by_Schteiben.jpg" width = "35%" align="right" style="padding-right: 20px;">
 
___Hard in training; easy in battle.___ — [Alexander Suvorov](https://en.wikipedia.org/wiki/Alexander_Suvorov), 18th century Russian General.

Live rounds are often fired over recruits training at boot camp, but this training and hardening applies beyond the military. Football coaches will play deafening music and spray water in a placekicker's face as he's practicing for a game winning field goal. Pilots encounter failure conditions in the flight simulator before taking to the skies. Astronauts train for every imaginable scenario before launching into space.

Testing is our training. We want to stress our implementations in as many hard and difficult situations as we can. [Test doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html) help accomplish this by becoming the training adversaries. Test doubles can easily throw exceptions, return quirky results, return no results, refuse to work, etc.

Be sadistic. Adverse testing in a test environment is a safe way to _train_ and _harden_ your code. We want to ensure what our code really does, not what we think it does.

_Tests don't break your code; they break your illusions about your code._ — [Llewellyn Falco](https://x.com/llewellynfalco/status/540084786128498688).

Make sure your code is ready for every challenge it could possibly encounter, including _this should never happen_ cases. If the code has encountered every possible condition in testing and responded as expected, then it should _easily_ be able to handle what it encounters in the production.

# Tests Can Document Developer Assumptions, Intentions and Expectations
All developers make assumptions and have intentions and expectations in their code. These invariants might be mentioned in design documents or in code comments, but design documents and code comments never stay up to date with the implementation. Assumptions, intentions and expectations may never leave the developer's head when considering the design and implementation. When the developer leaves the project or enough time has passed, that knowledge is gone. Fresh eyes may not notice the subtle invariants and introduce an invariant violating feature updates with dire consequences. 

Automated tests can document the developer’s assumptions, intentions and expectations. Should any developer update the code such that it violates a test specified assumption, intentions or expectations, then that test will fail. The failing test or tests may provide additional context for the developer to better understand the code that’s being modified.

The developer may need to take a new approach to accommodate the original assumptions, intentions and expectations, or those original invariants may no longer apply, which means the test may need to be modified or deleted.

Tests declared invariants never stray out of date with the implementation as design document or code comment invariants can.

# Tests Help Find and Prevent Future Bugs
Most new code doesn’t tend to have too many bugs in it regardless of whether automated tests have been provided or not. Developers are thinking through the scenarios and edge cases as they implement their code. They make sure those scenarios are handled appropriately. Developer assumptions, intentions and expectations are fresh in their minds. Developers who don’t create automated tests at the time are usually running informal and manual tests to confirm the new behavior they expect to see.

Let’s continue with the __No Automated Test__ scenario. Days, weeks or even months go by without an incident. And then there’s a ticket. Maybe it’s a missing edge case that wasn’t originally considered. Maybe new functionality needs to be added. Someone must modify the code.

If we’re lucky, the original developer is still on the project. How much do they recall? Do they remember their original assumptions, intentions and expectations? Probably most, but maybe not all. Do they remember their informal tests? Probably some, but maybe not all.

What if the original developer has left the project, and the ticket has been assigned to a new developer? Those fresh eyes may not notice the original developer’s intent from the implementation alone. The new developer modifies the code for the issue in the ticket but unknowingly also introduces a bug that violates one of the original developer’s undocumented invariants. If we’re lucky, spot testing will catch it, but there’s also a good chance that the bug will slip through, be delivered and released to the user.

Let’s pivot to the __Automated Test__ version of this story. The original developer created automated tests that specified behavior. The tests defined the original developer’s assumptions, intentions and expectations.

When the code is updated, and a bug violating a previous invariant is unknowingly introduced, then the test that specifies that invariant will fail. Not only will the failing tests prevent the new code from being delivered, but the failing tests will document the original invariant. The developer will hopefully have enough context to resolve the inconsistency and squash that bug.

<img src="https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExZTRqcWhyMjF3bWZpd3AxbzFxdTE3Njg2bDYybWNkcDFrdThwNzduaCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/UeK5obZpDdYjPKnIli/giphy.gif" alt="Bug Stomping" title="Image Source: https://media.giphy.com" width = "25%" align="right" style="padding-right: 20px;">

__Automated tests don’t find bugs now. They find and prevent future bugs.__

I'm a member of a developer Slack group, and here's a post from a contributor that highlights why we write automated tests:
> A story from a few years ago: a colleague of mine was maintaining some code I had originally written. He noticed an asymmetry in the persistence layer: one field was persisted only when the entity was created but not when it was updated. So he added that field to the list of updated fields. Then a test failed with the message “creation timestamp cannot be changed after the entity is created”. He undid his change and told me this story.

# Tests May Identify Concurrency Issues
Flaky tests are the worst. Flaky tests produce inconsistent results. Sometimes they pass. Sometimes they fail. While it’s annoying that they can foul up CI/CD pipeline submissions, the real issue is that we can’t trust them. Are their results false positives or false negatives?

If behavior is inconsistent in testing, it will be inconsistent in production.

I don’t think most flaky tests are a result of the test being flaky. I think it’s a result of the implementation being flaky. Therefore, a flaky test may be an indication of issues in the implementation.

I suspect that most flaky issues are due to concurrency issues in the implementation such as non-deterministic race conditions, which are difficult to resolve in deterministic tests. When tests are flaky reexamine the implementation to understand why they are flaky and make adjustment. This may not be easy. It may take some time to figure out. It may require some redesign, possibly with the [Humble Object](https://jhumelsine.github.io/2024/07/15/tdd.html) pattern.

# Tests Reduce Debugging
I don’t know how to use the debugger on my IDE.

_If you're good at the debugger it means you spent a lot of time debugging. I don't want you to be good at the debugger._ — [Bob Martin](https://www.goodreads.com/quotes/812031-if-you-re-good-at-the-debugger-it-means-you-spent)

If practicing TDD, you’re in a tight __Red-Green-Refactor__ cycle, which should take no more than a few minutes to complete. The code should never be more than a few minutes from having been in a working state.

<img src="https://i.giphy.com/media/v1.Y2lkPTc5MGI3NjExdG84NDFhaGVnNHEwYmFsODJocmh2OGxiMm8ybnJ1MHRvdTIxdHBxYSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/NHIecaiSc7YjK/giphy.gif" alt="Robot Debugging" title="Image Source: https://media.giphy.com" width = "25%" align="right" style="padding-right: 20px;">

When I find that my tests are failing, I give myself a few minutes to spot the issue. If I can’t find it easily, then I `undo` changes until I’m back into a state where all tests are working once more, and I start anew. Hopefully I don’t have to `undo` too many changes.

There have been a few times when I’ve gotten lax in running the test suite while working on the code, and when I do run the tests, at least one fails. The bug isn’t so obvious after several sets of changes. It can be a bit painful to watch code disappear that I wrote 15, 30 or even 60 minutes ago as I `undo` the recent changes. However, after I have gotten back to a passing state and start again, I run the test suite more frequently, and I find that progress goes much faster since it’s now the second time that I’m addressing the issue.

I never had to `undo` changes for the same task more than once.

# Tests Provide a Safety Net for Refactoring
Let’s assume we have tests that specify and challenge the behaviors in our implementation sufficiently.

Those tests provide a safety net that allows us to refactor our code with confidence. Recall that _refactoring_ is changing the structure of the code without changing its behavior. Since good tests are behavior based, we should be able to refactor without fear of introducing a bug. If refactoring does introduce a bug, then at least one test will fail, and we’ll be able to address it quickly.

Why would we want to refactor our code? Code can almost always be made cleaner. Refactor it to make it cleaner for the next developer. It's like picking up litter to make the sidewalks cleaner for everyone.

We often make implementation decisions before we fully understand the customer’s domain. As we get feedback from previous releases, we may learn that some of our architecture, design or implementation decisions do not align with the customer’s domain. This misalignment might introduce friction making it more difficult to add new behaviors or enhance existing ones.

If we knew then what we know now, would we have proceeded with the current implementation? If the answer is __No__, then we may have identified a refactoring candidate.

Sometimes the changes are more than just localized refactoring. We gain new insights into the customer’s domain, such as emerging  abstractions we had not realized previously. Enhancements at this scale affect class design. Classes may be split. Classes may be merged. New classes may be created. Some existing classes may be removed.

I consider this redesign rather than refactoring, or at least it is refactoring at a larger scale.

Unit tests tend to specify and confirm behavior scoped at classes. Since classes are being redesigned, their previous automated tests may need to be redesigned as well. Some tests will remain as is. Some tests may need to be modified. Some tests may need to move to another class if the behavior moves to that class as well.

# Tests Lead Toward Better More Modular Designs
The complexities of unit tests and implementations tend to correlate. The complexity of the first one implemented will influence the complexity of the second one implemented.

If implementation comes before the test, then the complexity of the implementation tends to show up in the test. If the test comes before the test, then the complexity of the test tends to show up in the implementation.

When practicing TDD, tests come before the implementation. When combined with BDD, those tests specify behavior in a straightforward __Given-When-Then__ structure. These straightforward tests tend to create a better more modular implementation than writing the implementation first. As I wrote in the [BDD](https://jhumelsine.github.io/2024/08/08/bdd.html) blog:
>Developers, who practice TDD/BDD and create complex tests and implementations, are taking extraordinary efforts to make their lives more miserable.

<img src="https://upload.wikimedia.org/wikipedia/commons/9/99/Rubber_duck_at_work.jpg" alt="Rubber Ducking" title="Image Source: https://commons.wikimedia.org/wiki/File:Rubber_duck_at_work.jpg" width = "35%" align="right" style="padding-right: 20px;">
 
I’m not sure why test first tends to produce better code. I think it’s similar to [Rubber Duck Debugging](https://en.wikipedia.org/wiki/Rubber_duck_debugging). It changes our perspective. From the Rubber Duck Debugging Wikipedia page:
>In describing what the code is supposed to do and observing what it actually does, any incongruity between these two becomes apparent.

That sounds sort of familiar to our testing practices too.

If automated tests are complex, messy or nasty, it’s probably an indication of an implementation that’s complex, messy or nasty. Ugly tests will identify implementation candidates for refactoring or redesign.

# Tests Allow Me To Code Faster

While I like time to ___think___ about my design, I’ve found that once I’m in the refactoring phase, I don’t need to ___think___ nearly as much. When I used to make code updates before I understood how to leverage automated tests, I’d fret over my updates. I'd consider mini-proofs in my head. I’d always be concerned that I had missed something. Had I introduced a hidden bug lurking in the code?

With automated tests I don’t worry about that nearly as much. I don’t need to overly think about a refactoring choice. I don’t run through every scenario in my head. The automated tests already cover them. If the tests pass after a refactoring step, then I move forward. If they fail, then I give myself a few minutes to spot the bug. If I can’t find it in a few minutes, I _undo_ my changes until the tests pass once more and start anew.

# Tests Produce Less Dead Code

TDD/BDD produces less dead code. Code should only be written only if it’s needed to make failing tests pass. Refactored code may cause some previously alive code to become dead code. It can be removed.

Dead code would remain only when the tests that cover it are not realistic scenarios. Should unrealistic tests be identified, then they can be deactivated. Coverage would identify any newly revealed dead code, which can be removed as well as the deactivated tests.

While I’m not a fan of minimum code coverage quality gates, developers should strive toward 100% code coverage for their own aspirations. Code that’s not covered is code that has no automated confirmation. It may work now, but should it be changed, there’s no guarantee that a bug might slip through.

Code created via TDD/BDD will tend to have high code coverage by default via the process. There will be exceptions. For example, [Humble Object](https://jhumelsine.github.io/2024/07/15/tdd.html) code doesn’t tend to have coverage, since that code has been quarantined due to its testing difficulties.

# Tests Lead Toward Better Public APIs
Have you ever encountered an interface or class API and been absolutely baffled by it? What were the API developers thinking? I suspect that many bad APIs happen when the initial focus is upon __how__ the software is implemented rather than __what__ it does or __what__ the customer needs. It’s an API designed from the inside out. That is, the API indicates __what__ the software does rather than __how__ it is to be used.

TDD/BDD specify behavior via a test before the code is implemented. The test accesses the SUT via its public APIs as the user would access it. We consider the public API methods from the user point of view before we consider the implementation.

The test becomes the first user of the public API. The developer creating the test must think about how it represents behavior rather than how it represents the eventual implementation. If the public API is awkward in the test, then it will be awkward in production. This is the perfect time to reconsider the public API to make it more comprehensible.

# Tests Provide Working Reference Model Documentation
Sometimes developers don’t have control over the public APIs that they implement. The public methods may have already been defined by the architects or team leads. Or the SUT’s public API may be a pluggable class that’s part of an already defined [Composable](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) design that’s using [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html), [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html), [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html), etc.

Regardless of the developer’s flexibility in shaping the public API declaration, the tests still serve as reference model documentation to the user demonstrating how to access those public APIs. This is why I like to keep the Given, When and Then portions of my tests as separate sections within the test. The tests serve as a reference model documenting how the SUT is accessed through its public API methods. If written with the intent of being documentation, then tests can also provide additional context for the public API methods.

Unlike more traditional reference model documentation, which sometimes does not work, tests as documentation are guaranteed to work.

# Tests Explore, Discover and Document Legacy Code
All the benefits above are based upon the assumption that tests specify behavior which shapes the implementation.

But what about legacy code? There’s a good chance that legacy code:
* Has been around for a long time
* Has few or no automated tests
* Was designed and written by someone who is long gone
* Is full of cruft and technical debt obscuring its behaviors and invariants
* Is critical for the business and responsible for our paychecks

Most developers shutter when plunged into legacy code for the reasons listed above. I will address legacy code soon (future blog TBD). While we can’t practice TDD as defined with legacy code that’s missing tests, we can use automated tests to explore, discover and document the behavior that lies within the legacy code.

Automated tests created after the code has been implemented are characterization tests (TBD). They define and lock down current behavior in legacy code without too much concern as to whether the current behavior is correct or not. It's probably correct, else we'd hear about it from users.

Characterization tests still follow the __Give-When-Then__ structure. We assume the legacy code works for non-error reporting scenarios in the legacy code. We start with the __Given-When__ portion of the tests that access the legacy SUT. Then we observe and codify what the legacy code does via the test. For example, if the SUT’s method returns a `boolean` then we assert in the __Then__ portion of the test based upon which `boolean` value is actually returned.

Characterization tests tend to depend more upon the implementation than behavior specification tests, since we may not know behavior when we’re writing them. They tend to be more brittle than the behavior specifying characterization tests. However, characterization test coverage provides a safety net providing us confidence to refactor the legacy code. Hopefully the embedded true nature of the code will begin to emerge through refactoring. As more behavior emerges, we can continue to refactor and redesign the legacy code so that it’s more aligned with its behaviors as specified in new or updated tests that will be created during the refactor and redesign phase as well.

# Tests Provide Working Reference Model Documentation for External Dependencies
APIs for external dependencies can be confusing. The contract behavior of the external dependency may not be obvious in the documentation. There might not even be any documentation.

Just as we can explore and document legacy code with tests, we can do the same with external dependency APIs in a variation of Consumer-Driven Contract (CDC) Testing (future blog TBD).

Tests that interact with external dependency APIs not only confirm and document their behavior, but they also serve as a developer reference model. And should the external dependency behavior change, then these tests will fail, and we may be able to adjust before the customer notices the changes in behavior.

Failing CDC Tests will also save us time from going down the rabbit hole in our own code when investigating a customer support ticket. Once we know that the undesired behavior is due to a change in behavior by the external dependency then we can decide whether to update our own code to adjust for that change, file a ticket with the external dependency vendor or both.

Since these tests interact with external dependencies, they may require more configuration. They may take longer to run. They may cost resources to execute. Therefore, these tests should probably not be executed as part of unit testing. We may not even want to execute them in the CI/CD pipelines. But we may want to execute them nightly or weekly to confirm that our expectations of external dependency behavior, as specified in our tests, is still observed with the external dependencies.

# Summary
Almost all these benefits are a result of test and implementation consistency. Inconsistencies between the two are manifested in failed tests. The rest of context determines why or how test and code are inconsistent.

# References
* Previous [blog references](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#references)
* [Does TDD really lead to good design?](https://www.codurance.com/publications/2015/05/12/does-tdd-lead-to-good-design) Blog by Sandro Mancuso
* [Testing Yourself – Growing Through Refactoring](https://www.linkedin.com/pulse/testing-yourself-growing-refactoring-michael-feathers/) Linkedin article by Michael Feathers
* [Do you-ou-ou a-wanna test like  me-e-e?](https://tabsandspaces.libsyn.com/2022/02) Tabs and Spaces Podcast with me as the featured artist with the three regular hosts discussing some of the topics I've listed here
