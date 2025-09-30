---
title: The Conversion of a Unit Test Denier or …
description: How I Learned to Stop Worrying and Love Unit Testing
---
<img src="https://upload.wikimedia.org/wikipedia/commons/b/b5/Statue_of_Saint_Paul%2C_Damascus.jpg" alt="Blinding and conversion of St. Paul on the road to Damascus" title="Image Source: https://commons.wikimedia.org/wiki/File:Statue_of_Saint_Paul,_Damascus.jpg" width = "50%" align="center" style="padding-right: 35px;">
 
# Introduction
Unit testing was a waste of time. At best, unit tests only confirmed the code that I already knew was working. At worst, they took time to write, and they tended to be brittle. They often stopped working after I updated the implementation. Maintaining the tests wasn’t worth the additional effort.

I don’t think I was unique with this opinion. Unit testing has been and continues to be a contentious topic in our industry. Some love it. Some hate it. There’s not much middle ground.

I was chatting with a former colleague recently. He told me that his shop is advocating [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html) (TDD). He said they were getting pushback from some developers. I chimed in, “Let me guess. The developers fighting the policy are the more experienced ones, right?” His expression indicated agreement. “Yes. The senior developers don’t want to do it. The junior developers are more willing to consider it.” No one seemed enthusiastic about it.

I bucked the trend of my friend’s senior developers’ opinions. Late in my career, I had a complete about-face and embraced unit testing. I not only embraced the practice, but I created and presented unit testing training to other developers at my company.

This blog will serve two purposes:
* Tell a bit of my story from unit test denier to convert.
* Be a springboard for future unit test related blogs.

# Grand Topic
Automated testing is grand topic. Whole books have been devoted to it. I can't fit it all into one blog. I have more to say about testing. I will follow up with a series of blogs about different aspects of automated testing. I’ll provide a link for each on this page when completed:
* [Attributes of Effective Unit Tests](https://jhumelsine.github.io/2024/06/14/unit-test-attributes.html) - Unit Test properties that make them more useful than not
* [Basic Elements of Automated Unit Tests](https://jhumelsine.github.io/2024/06/23/unit-test-elements.html) - Elevating automated tests to first-class citizen status
* [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html) - Emulate dependencies without depending upon dependencies
* [Suril, the Semaphore and Me](https://jhumelsine.github.io/2024/07/08/suril-semaphore.html) - When the theory became practice for me
* [Test-Driven Development - Writing Tests Before the Implementation](https://jhumelsine.github.io/2024/07/15/tdd.html) - I know it sounds completely backwards, but please give it some consideration
* [Yuri, the Programming Assignment and Me](https://jhumelsine.github.io/2024/07/22/yuri-evaluator.html) - My evening introducing [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html) to a young Computer Science student
* [Be On Your Best Behavior](https://jhumelsine.github.io/2024/08/08/bdd.html) - Test-Driven Development - How to create tests; Behavior-Driven Development - What tests to create
* [What is Behavior in Behavior-Driven Development](https://jhumelsine.github.io/2024/08/20/behavior.html) - More details about the nature of behavior
* [Testing Benefits](https://jhumelsine.github.io/2024/08/30/test-benefits.html) - Spoiler Alert – It’s not really about testing the code
* [Testing Concerns](https://jhumelsine.github.io/2024/09/08/test-concerns.html) - Test concerns may be a result of previous bad test experiences; there are ways to accommodate them
* [A House Divided Against Itself Will Not Stand](https://jhumelsine.github.io/2025/03/07/house-divided.html) - Resolving the conflict between [My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html) and [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) blog entries
* [Working Effectively with Legacy Code](https://jhumelsine.github.io/2025/03/24/legacy-code.html) - How to leverage automated test techniques when working with Legacy Code
* [How do you know if your test code is really testing your code?](https://jhumelsine.github.io/2025/03/28/mutation-testing.html) - How Mutation Testing can help test your test suite
* [Approval Testing - A Test Strategy for those who are reluctant to try Test-Driven Development](https://jhumelsine.github.io/2025/04/02/approval-testing.html) - A testint strategy that's in the intersection of Unit Testing and Characterization Testing
* [Mastering Time in Software Testing - Strategies for Temporal Behavior Verification](https://jhumelsine.github.io/2025/04/08/time-lord.html) - Become a Time Lord and control time in your tests
* [Humble Objects - Designing Code You Don’t Hate Testing](https://jhumelsine.github.io/2025/04/14/humble-object.html) - A pattern to help test code that's difficult to test
* [Test Layers: From Unit to System - Building Confidence with the Right Tests at the Right Level](https://jhumelsine.github.io/2025/06/23/test-layers.html) - There's more to testing than Unit Tests.
* [Consumer-Driven Contract Testing - Giving the consumer exactly what they asked for](https://jhumelsine.github.io/2025/07/14/cdc-testing.html) - When the Consumer supplies the Test Specifications to its Provider

Also see [Automated Testing AI Notebook](https://notebooklm.google.com/notebook/cdead562-a40d-4f5c-84cb-4368812f4347?_gl=1*uicl85*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDg2Mjk1NzckbzMkZzAkdDE3NDg2Mjk1NzckajYwJGwwJGgw).

# Formal Proofs
Unit testing was not presented in my academic career. I suspect this was because modern unit testing practices were still a decade or two in the future, and the technology didn’t support it.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/2/26/Punched_card_program_deck.agr.jpg/1180px-Punched_card_program_deck.agr.jpg" alt="Keypunch Card Deck" title="Image Source: https://commons.wikimedia.org/wiki/File:Punched_card_program_deck.agr.jpg" width = "30%" align="right" style="padding-right: 35px;">
 
Our programming assignments in my first two years in college were on punch cards and executed in batch mode. The time from feeding your card deck into the card reader until we got our printout back was usually 5 to 10 minutes. Then we’d look at the output, which was usually wrong, figure out what needed to be changed, type up new cards, insert them into our deck, avoid dropping the deck on the walk back to the card reader and start all over again. Turnaround time for the entire cycle could easily be 20 to 30 minutes.

Our instructors taught us how to prove that our programs were correct using [invariants](https://en.wikipedia.org/wiki/Invariant_(mathematics)#Invariants_in_computer_science). We learned how to prove the correctness of recursive programs using [strong mathematical induction](https://www.codecademy.com/resources/docs/discrete-math/proofs/proof-by-strong-induction).
Our instructors also showed us techniques that can best be described as a debugger on paper while we executed our code in our brains. We’d _think_ through the code while keeping track of the state using pencil and paper. I usually wrote my programming assignments out completely by hand before ever venturing to the Computer Lab.

I still use some of these techniques informally while thinking about my code.

Formal proofs are necessary for algorithms and data structure correctness. But if they really worked for code, then the Computer Lab would not have been more active than the bars downtown at 3:00 AM. Our _proven_ code should have worked on the first syntax-free execution.

Our testing consisted of running our programs on the assignment data until it produced the desired results. Once we saw it work, we’d grab our results, pack up our gear, head home and hope for a few hours of sleep before the next day of classes began.

# Unit … Well Really System Testing
<img src="https://i.imgflip.com/8sgtrw.jpg" alt="Star Wars Meme" title="Image Source: https://imgflip.com/memegenerator" width = "50%" align="right" style="padding-right: 35px;">

Things didn’t change too much once I started my career, except for fewer late nights. Waterfall was all the rage several decades ago. We’d document our analysis and design in massive three-ring binders, killing a lot of trees, and then finally we’d implement. We’d perform some manual testing for basic sanity, and then we’d hand it over to QA. They’d find any real problems with the code, right?

Even basic sanity testing could be difficult. It was often end-to-end testing, sometimes in the lab. It could take a lot of time and effort to even get your code into a test environment, as I described in the [Introduction of Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html). Confirming specific behaviors could be difficult, especially edge cases.
When the code didn’t work, it often became a major debugging effort. Did the problem reside in my code or some other part of the system that my code was calling?

# Test Code, But I Wouldn’t Call It Unit Test Code
I created test code programs with limited success throughout the years. Most were not automated. I had to run them manually. Often, I had to temporarily update the [Factories](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) in the implementation to return [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html) and then change it back again. See [Dependency Injection Introduction](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) for additional details.

The tests rarely told me anything that I didn’t already know. Once I started using [Design Patterns](https://jhumelsine.github.io/table-of-contents#design-patterns) in my designs and implementations, my error rate dropped significantly. I was happy with the quality of my code with or without automated tests. The tests were redundant at best, and they took time away from writing implementation code. That was part of my rationalization, since the tests tended to break as more code was modified.

It’s only in hindsight that I know what I was doing wrong with these earlier testing attempts:
* Tests were written after the code had been written.
* Each test confirmed as much behavior as possible.
* Test environment included system dependencies.
* Setting up a test took a long time, often due to the dependencies.
* Executing a test could take a long time, often due to the dependencies.
* Tests changed state in such a way that they induced dependencies upon one another. I.e., Tests could not be run in isolation, and changing a test could cause others to fail.
* Tests generated extensive and detailed reports, which I had to read and evaluate to determine whether they passed or failed.
* When a test failed, I was never quite sure if the issue was in my code or a dependency.
* As failing tests accumulated, I usually stopped running them. I _knew_ the code was working. Fixing the tests was too much effort.

# The Technical Book Club at Work
I was part of a weekly technical book club at the office.

## The Forgotten Unit Testing Book
We read a Unit Testing book, whose title and author I don’t remember. It didn’t convince me that I should be writing unit tests. I think this may have been the first place where I read about the unit testing process of writing a failing test before implementing the code. _Write a test before writing the code? That’s weird._ And then make the failing test pass by the quickest means possible, such as hardcoding what the test expects. _That makes even less sense._

The only part that made an impression on me was the section on Test Doubles.

__Update:__ The forgotten book has been found while I was cleaning out some old boxes. The book is ___Developer Testing: Building Quality into Software___ by Alexander Tarlinder. I suspect I'd get more out of it if I were to read it now. At a minimum, I'd be able to compare it against what I've since learned.

## Working Effectively with Legacy Code
<img src="https://m.media-amazon.com/images/I/51spmHveKdL._SY445_SX342_.jpg" alt="Working Effectively with Legacy Code Book Cover" title="Image Source: https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052" width = "30%" align="right" style="padding-right: 35px;">
 
Then we read ___Working Effectively with Legacy Code___ by [Michael Feathers](https://www.linkedin.com/in/michaelfeathers/). There are many definitions for legacy code:
* Code that's complicated and difficult to understand
* Code no one wants to touch
* Code without an official owner
* Code that works, is used by the customer and responsible for most of our paychecks
* The only true source of the behavior of our products, even if no one knows exactly what it is or where to find it

I’m currently of the opinion that code becomes legacy as soon as it’s been committed to the main branch.

Feathers defines legacy code as code without unit tests. He describes the typical process of updating legacy code as __edit and pray__. I could relate to that. 

Feathers devotes the first five chapters of his book to the value of unit tests, which can basically be summarized as moving the legacy code modification process from __edit and pray__ to __cover and modify__. Unit tests provide a _safety net_ so that changes can be made confidently. I think unit tests do much more than that, but I was just starting to gain a new appreciation for unit testing at the time.

Legacy code is obdurate to having unit tests added to it. We often must modify legacy code before it can accommodate unit tests. We can’t confidently update legacy code without unit tests, and we can’t add unit tests without updating the legacy code. We have a _Catch-22_ situation.

Feathers devotes the remaining 20 chapters of his book describing techniques that refactor legacy code with minimal risk, so that the legacy code will be more accommodating to additional unit tests.

My unit test epiphany came in __Chapter 13: I Need to Make a Change, but I Don’t Know What Tests to Write__ where Feathers wrote:
> Automated tests are a very important tool, but not for bug finding—not directly, at least. In general, automated tests should specify a goal that we’d like to fulfill or attempt to preserve behavior that is already there. In the natural flow of development, tests that _specify_ become tests that _preserve_.

Automated tests aren’t about testing the code. They’re about specifying and preserving behavior. I can document behavior, assumptions, invariants, etc. via an automated test. They will be confirmed each time the tests are executed. Any deviation will be immediately obvious in a failing test.

A failed automated test indicates an inconsistency between the test and implementation. The inconsistency may be due to a recently updated implementation that violates current behavior, assumption or invariant. Or sometimes it may be a new or updated test for new or updated behavior, et al., that is not supported by the implementation.

When failing, either the implementation or the test must be updated to resolve the inconsistency whether it’s new or updated code that violates a behavior, et al., or the behavior, et al., that no longer applies for an outdated test.

Regardless, the failing test cannot be ignored. This automated inconsistency verification is something our volumes of tree-killing analysis and design documents could never do.

For more details about this, see: [Working Effectively with Legacy Code … with all due respect to Michael Feathers in stealing his book title and a lot of his ideas for this blog](https://jhumelsine.github.io/2025/03/24/legacy-code.html).

# Clean Coder Videos
Soon after reading ___Working Effectively …___ I watched [Bob Martin’s](https://en.wikipedia.org/wiki/Robert_C._Martin) [Clean Coder Video Series](https://cleancoders.com/) on O’Reilly. Bob devotes several videos to unit testing where he provides more arguments for Unit Testing and details on how to create unit tests.

Bob reinforced what I was starting to appreciate from Michael Feathers.

# Put the Tests to the Test
I felt I had a reasonable understanding of unit test techniques. I tried the techniques for some Design Pattern example code I had been writing. I couldn’t believe how quickly I could proceed. I even changed the design midway through, and I was able to pivot seamlessly.

I started to use the techniques with project code going into production. Even though I was learning the techniques, I was happy with the results. I felt a new sense of confidence that I had never felt before.

I was sold.

# Summary
I saw the light. I, a _Unit Test Denier_, became a _Unit Test Evangelist_. I created a __Unit  Test Introduction__ course at work and presented it well over a dozen times.
 
# References
___Working Effectively with Legacy Code___ by Michael Feathers:
* [Amazon](https://www.amazon.com/Working-Effectively-Legacy-Michael-Feathers/dp/0131177052)
* [O’Reilly](https://learning.oreilly.com/library/view/working-effectively-with/0131177052/) 
* [The key points of Working Effectively with Legacy Code](https://understandlegacycode.com/blog/key-points-of-working-effectively-with-legacy-code/), a summary by [Nicolas Carlo](https://x.com/nicoespeon)

Bob Martin’s Test Related Videos:
* TDD Part, Part 1: [Clean Coders](https://cleancoders.com/episode/clean-code-episode-6-p1), [O’Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_01_06_01/)
* TDD Part, Part 2: [Clean Coders](https://cleancoders.com/episode/clean-code-episode-6-p2), [O’Reilly]( https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_01_06_02/)
* Advanced TDD‚ Part 1: [Clean Coders](https://cleancoders.com/episode/clean-code-episode-19-p1), [O’Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_02_19_01/)
* Advanced TDD‚ Part 2: [Clean Coders](https://cleancoders.com/episode/clean-code-episode-19-p2), [O’Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_02_19_02/)
* Clean Tests: [Clean Coders](https://cleancoders.com/episode/clean-code-episode-20), [O’Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_02_20_00/)
* Test Design: [Clean Coders](https://cleancoders.com/episode/clean-code-episode-21), [O’Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_02_21_00/)
* Test Process: [Clean Coders](https://cleancoders.com/episode/clean-code-episode-22), [O’Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_02_22_00/)
* Mocking: Part 1: [Clean Coders](https://cleancoders.com/episode/clean-code-episode-23-p1), [O’Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_02_23_01/)
* Mocking: Part 2: [Clean Coders](https://cleancoders.com/episode/clean-code-episode-23-p2), [O’Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_02_23_02/)
* Transformation Priority Premise‚ Part 1: [Clean Coders](https://cleancoders.com/episode/clean-code-episode-24-p1), [O’Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_02_24_01/)
* Transformation Priority Premise‚ Part 2: [Clean Coders](https://cleancoders.com/episode/clean-code-episode-24-p2), [O’Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_02_24_02/)
