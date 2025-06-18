---
title: DRAFT – Test Layers
description: How test layers confirm the behaviors of software components individually, in parts and as a whole
unlisted: true
---

<img src="https://upload.wikimedia.org/wikipedia/commons/f/f7/Bolt_and_nut%2C_annotated.jpg" alt="Nut and Bolt Mismatch" title="Image Source: https://commons.wikimedia.org/wiki/File:Bolt_and_nut,_annotated.jpg" width = "50%" align="center" style="padding: 35px;">

# Introduction
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/19/Mars_Climate_Orbiter_2.jpg/1024px-Mars_Climate_Orbiter_2.jpg" alt="Mars Climate Orbiter" title="Image Source: By NASA/JPL/Corby Waste - http://www.vitalstatistics.info/uploads/mars%20climate%20orbiter.jpg (see also http://www.jpl.nasa.gov/pictures/solar/mcoartist.html), Public Domain, https://commons.wikimedia.org/w/index.php?curid=390903" width = "40%" align="right" style="padding: 35px;">
 
On September 23, 1999 NASA lost contact the [Mars Climate Orbiter](https://en.wikipedia.org/wiki/Mars_Climate_Orbiter) as it was establishing its orbit around Mars. The [cause of failure](https://en.wikipedia.org/wiki/Mars_Climate_Orbiter#Cause_of_failure) was Lockheed Martin's ground control software using the imperial measuring system; whereas, NASA's orbital software was using the metric measuring system. Ground control was sending thruster firing instructions as pound-force seconds, but the orbiter was expecting them as newton-seconds.

This failure cost 286 days of mission time and $327.6 million.

The discrepancy had previously been noticed, but the warning was not heeded:
>_The discrepancy between calculated and measured position, resulting in the discrepancy between desired and actual orbit insertion altitude, had been noticed earlier by at least two navigators, whose concerns were dismissed because they "did not follow the rules about filling out [the] form to document their concerns"._

This was a [contract](https://jhumelsine.github.io/2025/06/10/contracts.html) failure. The two teams had either been given two different contract definitions, or they had different interpretations or assumptions of the same contract.

My [Automated Test Series](https://jhumelsine.github.io/3000/01/01/preface.html#automated-testing) has mostly focused upon Automated ___"Unit"___ Testing via [Behavior-Driven Development](https://jhumelsine.github.io/2024/08/08/bdd.html) (BDD) and [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html) (TDD) practices.

<img src="https://live.staticflickr.com/3719/9051059498_a84dfa949f_b.jpg" alt="Nut and Bolt" title="Image Source: https://www.flickr.com/photos/tudedude/9051059498" width = "25%" align="right" style="padding: 35px;">

Unit testing is like confirming what stresses a nut and bolt can withstand separately. While this is important, we also need to confirm that the nut and bolt are the same size with the same thread count. If the nut and bolt don’t screw together securely, it doesn’t matter how much stress each can withstand individually. 

Software is the same. Unit tests are myopic. They don’t see the bigger picture. I don’t think that unit testing alone would have identified the units of measure discrepancy in the Mars Orbiter software, since both software teams would have based their unit tests upon the assumption that they were each using the appropriate units of measure. The unit tests for both teams could have passed with flying colors. The discrepancy would have been easily observed when both sets of software were part of the same [Software Under Test](https://en.wikipedia.org/wiki/System_under_test) (SUT) in a test.

# The Bigger Picture
<img src="https://images.stockcake.com/public/4/8/9/48984547-b30d-40c4-994c-6a04e6fc6278/analyzing-complex-diagram-stockcake.jpg" alt="Analyzing a Complex Diagram" title="Image Source: https://stockcake.com/i/analyzing-complex-diagram_1386954_681864" width = "40%" align="right" style="padding: 35px;">
 
Acquiring the bigger picture often requires a tradeoff. Scope and detail are often inversely proportional. If we don’t reduce detail when expanding scope, then the resulting picture becomes so complex that we can’t comprehend it. We can't see the forest for the trees.

Consider online maps. Street view provides many details, but we can only see at most a block or two in any direction. Zoom out, and we can see neighborhoods, which provide a wider view, but at the loss of details. For example, rather than seeing images of the actual buildings, they become geometric shapes on the neighborhood map. Continue zooming out and we get a wider view of the city, region, country and world, but details disappear. We quickly lose buildings, local landmarks, smaller roads and even small communities. Each time we zoom out, we see more _scope_, but we also see fewer _details_.

## Scope vs Detail in Urban Planning
<img src="https://images.stockcake.com/public/5/4/f/54f8bdb5-84f3-41b8-98b5-41f4d746943c_large/team-analyzing-model-stockcake.jpg" alt="Team Analyzing a City Model" title="Image Source: https://stockcake.com/i/team-analyzing-model_1359011_1134250" width = "40%" align="right" style="padding: 35px;">

Let’s continue the scope/detail tradeoff with urban planning and see how it applies to three different types of civil servants in their roles and responsibilities.
 
### Building Code Inspector
Building code inspectors make sure that buildings are safe for occupancy. They check that the construction, plumbing, electrical systems, etc. are up to code.

Their scope is limited to the building they are expecting, and their only concerns are the infrastructure within the building they are inspecting. Their role is to ensure the safety of the building by being up to code, and their responsibility is only that building they are inspecting.

### Utility Manager
Utility managers ensure that the utility services for a community are working properly. This could include infrastructure systems for community, such as water, power, sewage, etc.

Their scope is the management of infrastructure among the buildings in the community and the buildings’ connections to utility infrastructure. Utility manager scope is not necessarily the buildings themselves. Utility managers’ concerns are the details of the utility infrastructure.

### City Planner
City planners monitor the entire community. 

Their scope is the entire community. They aren’t necessarily concerned about the utility details and building codes so long as they provide a strong foundation for the safety and welfare of the residents of the community. Their concerns are fuzzier. Are areas zoned properly? Are the residents happy? Is business thriving?

## The 30,000 Foot View
While the scope vs detail tradeoff can be viewed horizontally as scope expands, we tend to use language that frames it vertically, such as the 1,000-foot view, 10,000-foot view or the 30,000-foot view. We can view more of the landscape at higher elevations, but we see fewer details. These terms probably developed with the aviation industry, but you don’t need a plane to envision it. Zooming in and out of an online map provides the same effect.

The three civil servants are not literally at different elevation levels, but we can think of: 
* City planners at the highest level viewing the entire community
* Utility managers at a middle level focused upon infrastructure of the community
* Building code inspectors at the lowest level confirming building safety at the smallest details for the community

# Test Tactics
Just as the civil servants assess different aspects of the community based upon scope, software testing can assess different aspects of a system based upon the [Software Under Test](https://en.wikipedia.org/wiki/System_under_test) (SUT) that is declared for each test.

## Software Under Test
Different types of tests are often characterized in layers as higher-level, mid-level and lower-level testing, but like our civil servants, there’s no literal change in elevation.

Before I dive into test layers, I will describe different tests horizontally via the SUT in each test type.

Consider this design:

<img src="https://source.roboflow.com/RAVs9fdslLftww6gGNs37Usnnlx2/0Ux4Vt6NXOwkdLU5lOwS/original.jpg" alt="UML Class Design" title="Image Source: https://universe.roboflow.com/uml-class-diagram-classification/class-diagram-classification" width = "60%" align="center" style="padding: 35px;">
  
## Narrowly Scoped Unit Tests
We can design tests narrowly scoped to only one class, such as: `Transaction`, `Session`, `Tutor`, etc. Any dependencies upon other classes would be replaced by emulating [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html).

These tests specify and confirm the behavior of the class, which includes not just the expected scenarios, but the edge cases too. These tests stress our code. We don’t write tests to _show_ that the code works. We write these tests as experiments to try to break our code. The more test stress our code withstands, the greater confidence we gain in our code. This quote, featured in [Test are Experiments](https://jhumelsine.github.io/2024/08/30/test-benefits.html#tests-are-experiments), describes it well:

_Tests don't break your code; they break your illusions about the quality of that code._ — [Maaret Pyhäjärvi](https://www.pinterest.com/pin/597149231821381509/).

These tests are generally considered __Unit Tests__. Using a previous analogy, each class being tested is like a nut or bolt in the system.

## Moderately Scoped Integration Tests
We design tests scoped to the package, such as: `leisure program`, `leisure facility` and `member`. The SUT will include more classes in the design. Any dependencies upon classes in other packages would be replaced by emulating Test Doubles.

These tests specify and confirm the behavior of the package. These tests might describe scenarios similar to the Unit Tests above; however, they focus upon confirming that the classes work together cohesively. These tests don’t cover every nook and cranny in the implementation, since the Unit Tests cover these ases. These tests favor expected scenarios and bypass edge cases. Some edge cases may be too difficult to set up with these tests.

These tests are generally considered [__Integration Tests__]( https://en.wikipedia.org/wiki/Integration_testing), since they confirm the integration of components. They would represent screwing the bolt into the nut to ensure they fit.

There’s a subtype of Integration Test: [__Acceptance Tests__](https://en.wikipedia.org/wiki/Acceptance_testing). Acceptance Tests focus upon user specific desired behavior. They derive from [Acceptance Criteria](https://cio-wiki.org/wiki/Acceptance_Criteria), which are artifacts in [User Stories](https://en.wikipedia.org/wiki/User_story). An Acceptance Test isn’t a scenario conceived by a tester or developer. It’s defined by the domain expert.

## Broadly Scoped System Tests
We design tests scoped to the entire system. Any dependencies would be external to the system. They might be replaced by emulating Test Doubles, or they might be the actual dependency. These tests specify the behavior of the whole system.

These tests are generally considered __System Tests__, since they confirm the entire system. The concept of _nut_ and _bolt_ may no longer apply at this level. These tests are concerned with the entire system regardless of the implementation. If desired, we could replace the nuts and bolts of the system with welded joints, and the System Tests should still pass. These tests are often executed manually, since they may involve the UI. Some test frameworks support UIs, but the tests can still be brittle and break with UI changes.

These tests are sometimes called __End-to-End Tests__ (E2E).

<img src="https://upload.wikimedia.org/wikipedia/commons/a/a7/Citigroup_Center_%281977%29_%28A._Moore%29_with_frame_-_agr_ink2.png" alt="Citicorp Building" title="Image Source: https://commons.wikimedia.org/wiki/File:Citigroup_Center_%281977%29_%28A._Moore%29_with_frame_-_agr_ink2.png" width = "25%" align="right" style="padding: 35px;">

__NOTE:__ Replacing a system’s nuts and bolts with welded joints actually happened as described in this [99% Invisible Episode: Structural Integrity](https://99percentinvisible.org/episode/structural-integrity/)/[Transcript](https://99percentinvisible.org/episode/structural-integrity/transcript/).

An architectural engineering undergraduate student’s class assignment was analyze the stresses on the recently constructed Citicorp building in New York City. The analysis revealed a 1-in-16 chance of the building collapsing when subjected to hurricane force winds directed upon its corners. For three months in the late 1970s, workers clandestinely replaced the nuts and bolts with welded joints during the overnight hours so that the unaware office workers would not know of the potential danger of their building during their daytime working hours.

# Testing Layers
Though System, Integration/Acceptance and Unit Tests are scoped by the horizontal Software Under Test enclosed within them, they are almost always described vertically with System Tests being on the top, Integration/Acceptance Tests in the middle and Unit Tests at the bottom.

There is not always a clear boundary between these layers. It can be a bit fuzzy. A limited behvior specifying test might involve several classes. Some people might consider it a Unit Test, since it's limited behavior, and others might consider it an Integration Test, since it involves several classes. It really doesn’t matter. All automated tests should specify and confirm behavior using the same [__Give/When/Then__](https://en.wikipedia.org/wiki/Given-When-Then) structure.

The major distinction between test layers is how much of the actual system is under test. While this doesn’t sound like much of a distinction, it creates subtle differences among the testing layers:
* Lower-level tests tend to take less time to create than higher-level tests.
* Lower-level tests tend to be easier to automate than higher-level tests.
* Lower-level tests tend to be less fragile than higher tests, since higher-level tests may depend upon UIs, which often change.
* Lower-level tests tend to be replaced more than higher-level tests during refactoring and redesign.
* Lower-level tests tend to run faster than higher-level tests.
* Lower-level tests tend to cover more code in the aggregate than higher-level tests, specifically the edge cases. An individual lower-level test will cover less code than an individual higher-level test, but the set of lower-level tests will tend to cover more code than the set of higher-level tests.
* Lower-level tests tend to be easier to set up than higher-level tests.
* Lower-level tests tend to be more aligned with the design than higher-level tests, which tend to be more aligned with the system architecture.
* Lower-level tests tend to provide greater confidence in the individual software components.
* Higher-level tests tend to provide greater confidence in the overall system.
* The software responsible for newly introduced errors tends to be easier to isolate when found in failing lower-level tests than higher-level tests, since there is less SUT in lower-level tests and therefore fewer places where the error may reside.
* Higher-level tests tend to find errors between software components more than lower-level tests.

Each test layer is tactical. Let’s examine several test strategies that favor layers differently.

# Test Strategies
Test tactics define test layers, each of which tests the software with different emphasis. Should we use a strategy that focuses upon one tactic more than another?

## Snow Cone Test Strategy
<img src="https://openclipart.org/download/282183/Shaved-Ice.svg" alt="Snow Cone" title="Image Source: https://openclipart.org/detail/282183/hawaiian-shaved-ice" width = "30%" align="right" style="padding: 35px;">
 
This was the test strategy for most of my career. There’s an emphasis upon System Testing, as represented by the broad top of the snow cone. There are fewer Integration Tests in the narrow middle and even fewer Unit Tests at the almost non-existent bottom.

We had dedicated QA testers whose mission was to test our system to ensure it was ready for release. QA testers were the final quality gate before deploying the system and releasing it to the customer. There was often an adversarial relationship between developers and testers. Developers would often become annoyed when testers found a bug in their code. Would developers prefer for users to have had find their bugs instead? 

### Pros
Manual System Testing was all we had before we had test frameworks that supported automated testing. We didn’t have the insights to understand the concept of automated testing either. Since so much software production was modeled upon manufacturing processes, QA was the final step before sending it to the customer. It’s all we knew at the time. It wasn’t perfect, or even great, but it was _mostly_ good enough for its day.

While System Testing exclusively may no longer be in vogue, there can be some benefits; mainly, interacting with the system like the user would. The goal isn’t to test the system to find bugs, even if we might find a few that have slipped through the automated testing. The goal of this exploratory manual testing is to allow anyone to tap into their human intuition when interacting with it. Explore the system. Is it easy to navigate? Does it make sense? Is it clunky?

Since this isn’t QA focused, everyone should consider doing this: domain experts, developers and testers. It should expand beyond individual contributors to management and executives too. If you can’t easily use your system, your customer and user won’t be able to easily use it either.

### Cons
So much garbage was thrown over the wall by developers to QA, usually to meet an artificial deadline, in the hope that any serious problems in that garbage would be found by QA. Testers would also be annoyed when they found obvious bugs that the developers should have found on their own. This was during the days of the Waterfall Methdology, and a lot of garbage was thrown over almost every wall in the process, not just developers to QA.

_When programmers do their jobs, testers find nothing._ — [Bob Martin](https://x.com/unclebobmartin/status/541876357320671232)

Most System Testing was manual. Manual testing is slow, error prone and mind-numbing. Sometimes QA would get last minute updates with barely any time to run tests before release. There was a lot of sitting around waiting for code and then a mass rush to execute as many tests as possible to meet the delivery date.

_Computers are designed to do simple repetitive tasks. As soon as you have humans doing repetitive tasks on behalf of computers, they all get together late at night and laugh at you._ — [Neal Ford](https://nealford.com/memeagora/2015/09/02/simple-repetitive-tasks.html)

There’s a limit to what System Tests can achieve. While System Tests can confirm many behaviors, it can be difficult to set up edge case scenarios.

The feedback loop for developers is too long. It could take several days before QA received a new version of code, then a few more days until an error was revealed in a manually executed test scenario. It could take a week or more before a bug was identified, investigated and assigned to a developer.

Automated System Testing frameworks are becoming more prevalent, but they can still be brittle when they depend upon the UI.

## Pyramid Test Strategy
<img src="https://www.worldhistory.org/img/r/p/1000x1200/12087.gif" alt="Egyptian Pyramid" title="Image Source: https://www.worldhistory.org/Great_Pyramid_of_Giza/" width = "30%" align="right" style="padding: 35px;">
 
The [Pyramid Test Strategy](https://martinfowler.com/bliki/TestPyramid.html) flips the Snow Cone Test Strategy. In Pyramid Testing, there are many Unit Tests at the bottom, representing the base of the pyramid, with fewer Integration/Acceptance Tests representing the middle of the pyramid and finally the fewest System Tests at the apex.

### Pros
Lower-level tests tend to accommodate automated tests, so this strategy tends to work well with CI/CD pipelines. An entire test suite can be executed on each commit to a major branch easily.

The software elements should have nearly complete coverage. Working elements don’t guarantee a working system, but flawed elements will almost certainly guarantee a flawed system.

Since Unit Tests tend to specify units of behavior, each Unit Test tends to execute a limited number of statements. Therefore, when a Unit Test fails, run the failing tests in isolation with code coverage activated, and the limited number of covered lines should be the first place you look for the error.

### Cons
Since there’s more emphasis on the lower-level tests than the mid-level tests, we can run into the situation of having great confidence in the software elements, but not as much confidence in the overall system.

Since tests specify behavior with a class, any refactoring or redesign that affects classes will tend to affect tests. When implementation is moved among classes, then the tests that defined those behaviors will probably need to be decomposed, moved and reassembled with the refactored and redesigned implementation as well.

## Test Trophy
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/c/c7/Covered_cup%2C_Paul_de_Lamerie%2C_1737_-_Nelson-Atkins_Museum_of_Art_-_DSC08875.JPG/960px-Covered_cup%2C_Paul_de_Lamerie%2C_1737_-_Nelson-Atkins_Museum_of_Art_-_DSC08875.JPG" alt="Trophy Cup" title="Image Source: https://commons.wikimedia.org/wiki/File:Covered_cup,_Paul_de_Lamerie,_1737_-_Nelson-Atkins_Museum_of_Art_-_DSC08875.JPG" width = "25%" align="right" style="padding: 35px;">
 
The [Trophy Test Strategy](https://kentcdodds.com/blog/the-testing-trophy-and-testing-classifications) focuses upon Integration/Acceptance Tests. This is represented in the silhouette of Trophy Cup with few System Tests at the top, most tests in the middle with Integration/Acceptance Tests and a few Unit Tests underneath. The Trophy Test Strategy also includes a pedestal, which represents Static Analysis Tests, which are linters that check the implementation for coding standards and practices. The other test strategies tend to include them as well, but they don’t tend to show up in their definitions.

### Pros
The theory behind the Trophy Test Strategy is that we want to focus upon customer and user desired behaviors, which would be defined in Acceptance Tests. The customer doesn’t care about the implementation, so we shouldn't dedicated resources that are aligned with the design. The customer only cares about the features they will be using, and Acceptance Tests describe the scenarios for those features independent of the implementation.

The Integration/Acceptance Tests would tend to cover a significant amount of code. Unit Tests would be added to cover the remaining code. These tests would tend to be edge case code that can’t easily be covered via Integration/Acceptance Tests.

Since Acceptance Tests describe feature behavior, not design behavior, refactoring and redesign should have minimal impact upon Acceptance Tests. And since there are fewer Unit Tests, there should be fewer Unit Tests that need to be updated.

### Cons
An Integration/Acceptance Test will tend to execute more lines of code than a Unit Test. When an Integration/Acceptance Test fails, it will take more effort to find and fix the problem. Debugging will mostly likely be required. When a Unit Test fails, it tends to execute less code, possibly fewer than ten lines. While some degree of debugging may be required, the search area for that bug is smaller.

Since Integration/Acceptance Tests aren't scoped to a specific class, there may not be a quick suite of tests to confirm a class while you're working on it.

## Yes, And
Test Strategies are not mutually exclusive. It’s not a case of either/or. It’s yes, and.

I tend to prefer the Pyramid Test Strategy, but I’m not averse to the Trophy Test Strategy. You can leverage both. Use Test Strategies where they best fit. Some classes may tend to lean toward Pyramid. Some may tend to lean toward Trophy. And then with the confidence you gain in the code from Pyramid/Trophy Testing, you can always add exploratory manual System Testing on top not to look for errors, but to make sure that the system still feels like a cohesive product.

It can be the best of all worlds.

# The Achilles Heel of Higher-Level Testing
Higher-level testing cannot easily test one type of design paradigm: [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html).

Behavior emerges from a composable design via the interaction of objects, which are assembled at run-time. Unit Tests can specify and confirm behavior for individual classes in isolation. Unit/Integration Tests can also confirm that the objects can be created and assembled in various combinations. Unit/Integration Tests can confirm the behaviors that emerge from test specified object compositions.

However, developers and testers won’t be able to cover all possible scenarios. This is not a case of developer or tester incompetence. Object composition may be unlimited. It’s impossible to cover all possible scenarios when there’s a potentially infinite number of them.

For some designs, the configuration of compositions might remain within the definition of development. This limited set of compositions can be tested via Integration Tests. But when behavior composition be made available to users, customers or even customer support, they are free to define any compositions. These cannot be tested, since they may not be defined until long after the software has been released.

The more composition options a design supports, the quicker the composition combinatorics explode. The [Interpreter Design Pattern](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) supports the most composition options. We can test each of the elements of an Interpreter design easily. We can’t test all the composition possibilities.

This is not a new problem. Compiler developers have always had this issue. Compiler developers can confirm that their compilers generate object code from the source code correctly, but they can’t predict every program that could be written in the compiler’s language and test it. No one blames JVM developers when there’s a logic bug in their own Java code.

While we can’t confirm all possible compositions, we can confirm all the individual elements, just as the compiler developer can confirm all possible language features. While composable designs may be unbounded in the size their assembly, their assembly is often restricted by guardrails defining how the elements can be assembled. While users and customers may be able to define compositions that don’t define exactly what they wanted, they cannot define compositions that are illegal in the domain. For example, a Java developer can write a Java program with a logic error, but they cannot write a compliable Java program with a syntax error.

# Summary
__TBD__

# References
 __TBD__
* https://www.google.com/search?q=consumer+driven+contract+testing
* https://microsoft.github.io/code-with-engineering-playbook/automated-testing/cdc-testing/
* https://pactflow.io/what-is-consumer-driven-contract-testing/
* https://docs.pact.io/
* https://en.wikipedia.org/wiki/Acceptance_testing
* https://www.youtube.com/watch?v=38egQLsbgKk
* https://www.youtube.com/watch?v=qMXsTb_rPpA
* https://www.youtube.com/watch?v=V-OV6lRwhYA
* https://dev.to/muratkeremozcan/my-thoughts-and-notes-about-consumer-driven-contract-testing-11id
* https://thoughtworks.github.io/pacto/patterns/cdc/
* https://www.hypertest.co/contract-testing/what-is-consumer-driven-contract-testing-cdc
* https://testsigma.com/blog/consumer-driven-contract-testing/
* https://www.youtube.com/results?search_query=Consumer+Driven+Contract+Testing
* https://jhumelsine.github.io/
* https://jhumelsine.github.io/2023/10/28/hexagonal-architecture-structure.html
