---
title: DRAFT – Working Effectively with Legacy Code
description: … with all due respect to Michael Feathers in stealing his book title and a lot of his ideas for this blog
unlisted: true
---

# Introduction
I wrapped up the first part of my [Automated Testing](https://jhumelsine.github.io/3000/01/01/preface.html#automated-testing) series primarily focused upon [Test-Driven Development]( https://jhumelsine.github.io/2024/07/15/tdd.html) (TDD) and [Behavior-Driven Development](https://jhumelsine.github.io/2024/08/08/bdd.html) (BDD).

BDD and TDD are great practices to drive and shape a new implementation. However, many software developers maintain existing legacy code more often than they write new code. Since BDD and TDD practices focus upon writing tests before implementing the code, and legacy code has already been implemented, these practices, by definition, cannot support legacy code.

However, we can use concepts from BDD and TDD to help us maintain legacy code.

# What is a Legacy?
Lin Manuel-Miranda had Alexander Hamilton ponder the meaning of legacy in _Hamilton: The Musical_ with:
> [Legacy. What is a legacy? It’s planting seeds in a garden you never get to see.](https://www.goodreads.com/quotes/7882391-legacy-what-is-a-legacy-it-s-planting-seeds-in-a)

Legacy tends to have positive connotations. It’s passing something valuable to the next generation. However, legacy code tends to have negative connotations.

Planting a seed in a garden isn’t enough to ensure a valuable legacy. One must nurture and maintain the garden by watering it and keeping it free of weed and pests. All too often, legacy code isn’t well maintained.

I’m generalizing a bit, but legacy code tends to:
* Have large classes
* Have long methods
* Have deep nesting
* Have years of undocumented user-requested patches
* Have little nuggets of important and hidden behaviors that have long since been forgotten
* Depend upon many other software elements, often external dependencies
* Lack refactoring or redesign throughout its lifecycle
* Lack separation of concerns and appropriate levels of abstraction
* Lack automated tests

I do not wish to overly malign legacy code. We do not know what pressures the developer was under, nor do we know the experience level of the developer when the legacy code was implemented. No one sets out to create legacy code.

Regardless of the state of legacy code, it has one critical characteristic: __Legacy code is the reason why many developers receive a regular paycheck.__

# Legacy Code’s Legacy
<img src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/d04a19a7-6e8c-4647-a3c4-e852a1ee59f1/d8vndh6-aa7fdb32-d041-4a5a-bc28-1b3cc4623fbf.png?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7InBhdGgiOiJcL2ZcL2QwNGExOWE3LTZlOGMtNDY0Ny1hM2M0LWU4NTJhMWVlNTlmMVwvZDh2bmRoNi1hYTdmZGIzMi1kMDQxLTRhNWEtYmMyOC0xYjNjYzQ2MjNmYmYucG5nIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmZpbGUuZG93bmxvYWQiXX0.n7VgHxcdZFB87NwW9XNKTTO9V5KOxlfRRUFQ5YMr2YI" alt="Here Be Dragons" title="Image Source: https://www.deviantart.com/bluebottleflyer/art/Art-Jam-Here-Be-Dragons-logo-536888058" width = "25%" align="right" style="padding-right: 20px;">

For all its maintenance issues, legacy code tends to work. It’s tested everyday by our best and most expensive testers – our users. If a serious problem is encountered by users, they tend to report it.

Given that our paying customers depend upon working legacy code regardless of its maintainability, we mostly want to leave working code well enough alone. If it ain’t broke, don’t fix it.

We don’t always have the luxury of letting sleep dogs lie when it comes to legacy code. Sometimes a user unveils a dormant bug that’s been lurking in the code for years. New feature requests may require us to update legacy code.

Few things strike fear in the hearts of developers more than being assigned a ticket that forces them to descend into the bowels of unfamiliar legacy code.

# Working Effectively with Legacy Code
<img src="https://cdn2.picryl.com/photo/2012/02/16/an-explosive-ordnance-disposal-specialist-with-56th-464bc7-1024.jpg" alt="Bomb Squad" title="Image Source: https://nara.getarchive.net/media/an-explosive-ordnance-disposal-specialist-with-56th-464bc7
" width = "35%" align="left" style="padding-right: 20px;">

Working with legacy code is challenging. The implementation may be opaque. Many behaviors may not be well understood or documented. The challenge isn’t so much in updating legacy code to support new or updated behaviors. The challenge is in doing so without breaking any existing behaviors that the user depends upon. Updating legacy code can feel like the software equivalent of being on the bomb squad and hoping that you don’t cut the wrong wire.

[Michael Feathers](https://michaelfeathers.silvrback.com/bio) provided a hope of salvation with: ___Working Effectively with Legacy Code___. I briefly referenced [his book](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#working-effectively-with-legacy-code) in [The Conversion of a Unit Test Denier or … How I Learned to Stop Worrying and Love Unit Testing](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html). Here’s an excellent [summary](https://understandlegacycode.com/blog/key-points-of-working-effectively-with-legacy-code/) of the key points of book by [Nicolas Carlo](https://bsky.app/profile/nicoespeon.com).

Feathers contends that all too often developers rely upon the __edit and pray__ process when updating legacy code. Modify legacy without understanding the dependencies, and how likely are you to break it? Though a bit dated, this 2012 paper, [SOFTWARE DEFECT ORIGINS AND REMOVAL METHODS](https://insights.cermacademy.com/6-software-defect-origins-and-removal-methods-c-capers-jones-technologyrisk/) by [Capers Jones](https://en.wikipedia.org/wiki/Capers_Jones) states that the odds of a modification introducing a new bug can be as high as 25%. He calls these _bad fixes_:
>_Bad fixes are inversely proportional to cyclomatic complexity, and also inversely proportional to experience. Bad fixes by a top-gun software engineer working with software with low cyclomatic complexity can be only a fraction of 1%._
>
>_At the other end of the spectrum, bad fixes by a novice trying to fix a bug in an error-prone module with high cyclomatic complexity can top 25%._

Feathers advocates __cover and modify__, where developers cover the legacy code with tests, and then modify it with the confidence that existing behavior hasn’t broken unintentionally.

Legacy code doesn’t tend to have tests. Feathers defines legacy code as code without tests. Unfortunately, code that was implemented without tests tends to be obdurate to having tests added to it later.

We can’t easily add tests to obdurate code, until we’ve updated the code to make it more accommodating to test. But we can’t modify legacy code confidently unless we have tests to ensure we haven’t broken something. We’re in a Catch-22 situation.

Feathers devotes the first 20% of his book to convincing developers to add tests to legacy code. He devotes the remaining 80% to relatively safe refactoring techniques one can apply to obdurate non-tested code safely making it more accommodating to tests.

I'm not going to sugar coat this. Adding tests to legacy code is hard work. Sometimes you can spend a day or more and your only progress is adding coverage to a handful of lines of legacy code.

__TIP:__ _If your unit tests are nasty and/or difficult to implement, the issue is probably not the test. It is an indication that code those tests confirm is nasty and in need of refactoring and/or redesign._

That previous __TIP__ is going to haunt you. Legacy code wasn't created using TDD. It is probably nasty and in need of refactoring and/or redesign. The unit tests will be nasty and difficult to implement, at least at the start. Well designed and implemented code tends to have straightforward test cases. Therefore, test cases that are messy, confusing, complicated, etc. may be an indication that the code they confirm is messy, confusing, complicated, etc. This could be an indication of a redesign opportunity, but I’m getting a bit ahead of myself.

## Seams
<img src="https://upload.wikimedia.org/wikipedia/commons/9/97/Skr%C3%A4ddare_-_Tailor_-_Malm%C3%B6_1993.jpg" alt="Tailor" title="Image Source: https://commons.wikimedia.org/wiki/File:Skr%C3%A4ddare_-_Tailor_-_Malm%C3%B6_1993.jpg " width = "35%" align="right" style="padding-right: 20px;">

One of my previous officemate’s father was an old-school tailor. He could deconstruct a suit into its individual parts and reassemble them into a customed fit for his clients. He didn’t disassemble the suit by cutting randomly into the cloth. He deconstructed the suit at its seams.

Code is most easily tested along its seams. Seams allow us to disassemble and isolate pieces of code much like my officemate’s father was able to disassemble and isolate the pieces of a suit. Once code is isolated from its dependencies, then it can be more easily tested.

Seams reside along boundaries and tend to be [Stable or Fixed Elements](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#stable-or-fixed-design-elements) within the code, such as interfaces and method signatures. Ideally, seams lie along [Natural Boundaries](https://jhumelsine.github.io/2024/05/28/design-process.html#natural-boundaries).

## Legacy Code Lacks Seams
Legacy code tends to bloat as it ages, one small modification at a time, without much concern toward maintaining natural boundary seams. It’s the lack of seams in legacy code that makes it obdurate to tests. To make legacy code more test accommodating, we may need to add seams so that we can isolate pieces of code making them more manageable for testing.

[Extract Method](https://refactoring.guru/extract-method) is one of the most valuable refactoring tool to safely introduce seams to legacy code. The steps of extract method are basically:
* Select a block of statements in the code that ideally encapsulates a single intent or responsibility
* Copy those statements into their own newly named method where the new name indicates the intent or responsibility
* Replace the original block with a method call to the new extracted method

There are several advantages of Extract Method:
* Larger methods are decomposed into smaller more manageable methods
* New method names provide additional context
* Extracted methods introduce seams making the code more accommodating to tests

Extract Method is a refactoring tool in most IDEs. These refactoring tools manage all the details except the name choice of the new method. If the new method’s intent is unclear, then choose a name that best describes what it’s doing, even if the name describes the method’s implementation. The name can be updated as the code reveals itself, and intent is better understood.

IDE tools usually declare extracted methods as `private` by default. This is usually what we want when refactoring code that already has test coverage. However, in the case of introducing seams to legacy code for testing, we will probably need to declare the extracted method as __package-private__. I’m using this Java term to convey that the method has scope to code within the same namespace, and test code is often in the same namespace as the implementation being tested, without exposing the method as `public`.

A `package-private` declaration allows us to:
* Override the extracted method with a [Test Double](https://jhumelsine.github.io/2024/07/02/test-doubles.html)
* Test extracted methods separately in isolation

Though a __package-private__ declaration couples the tests with the implementation making the tests a bit more brittle, we may need to do this until we have added sufficient test coverage.

I described how Suril and I [introduced a `package-private` seam](https://jhumelsine.github.io/2024/07/08/suril-semaphore.html#a-minor-refactoring-to-introduce-a-test-seam) to code via Extract Method to make testing easier.

## Characterization Tests
Introducing seams allows us to isolate pieces of the legacy code so that they can be tested. But what are we testing? TDD and BDD start with the assumption that you know the expected behavior, but that may not be the case for legacy code. We may have a notion of some of the behavior, but there’s a good chance we don’t know all the behavior that’s lurking in legacy code.

Feathers describes [Characterization Tests](https://michaelfeathers.silvrback.com/characterization-testing) in his book. Characterization Tests look like TDD/BDD tests since they still maintain the [__Give/When/Then__](https://en.wikipedia.org/wiki/Given-When-Then) format.

Characterization Tests differ from TDD/BDD tests in that TDD/BDD tests specify behavior, whereas __Characterization Tests reveal existing behavior__. Since users execute legacy code regularly, any Characterization Test revealed behaviors will be correct, dead code or unobservable by the user. The goal of Characterization Tests is to lock down existing behavior so that additional refactoring can be done to clean up the code making it maintainable without breaking any existing behavior.

Characterization Tests reveal behavior after the implementation. How do we create a test to reveal behavior when we don’t know what behavior is being revealed? We will need to reference the implementation.

Here are some suggestions when writing Characterization Tests:
* Focus upon a public method at a time.
* Write a test that calls the public method in the __When__ section of the test.
* The test will probably throw an exception because a dependency has not been initialized. __NOTE:__ This could be a complete test as is since it reveals that the code throws an exception with uninitialized dependencies, but that’s probably not the main concern yet.
* Declare and inject __Test Doubles__ in the __Given__ section of the tests to resolve dependencies. This may require more seams via method extraction.
* Once the test no longer throws exceptions, focus upon the __Then__ portion of the test by adding __Assertions__ and __Verifications__. For example, if the public method returns a String, then add an assert declaring that the expected return value is something outrageous, such as: _Fred Flintstone_. This will fail, but it will also identify the actual String that was returned by the legacy code. Replace _Fred Flintstone_ with what the public method returned. This has revealed the legacy code’s behavior. It’s documented in an automated and repeatable test.
* Run the test with code coverage activated. This will identify the lines of code that were executed in test. Put on your detective hat and look for other behaviors that can be asserted or verified. Add them in the __Then__ section of the test.
* While the Characterization Test documents revealed behavior, we may still not understand what is being revealed. You may not be able to provide a test name that indicates behavior specification or intent. Don’t be concerned about this. Choose a name that describes what the test is doing instead.
* Repeat with a new test until you the tests cover as much of the scoped legacy code as possible.

Characterization Tests are an exploratory tool. We use them to reveal behavior, not define it. We never know when all of the behaviors have been revealed. Creating enough Characterization Tests is a judgement call. Code coverage is probably our greatest tool in determining if we have enough Characterization Tests. When statements have no coverage, then we know that there is more work to do. Complete coverage does not guarantee that all behaviors have been covered, but uncovered code is a clear indication that there's uncovered behavior.

Characterization tests are a behavior exploration, discovery and revealing technique.

## Iterate All Combinations
Most legacy code I’ve encountered was a [Big Ball of Mud](https://blog.codinghorror.com/the-big-ball-of-mud-and-other-architectural-disasters/). Methods contained too many paths of execution through the code to create individual Characterization Tests. In these cases, I would design a Characterization Test that would iterate through all possible configuration combinations in the method being tested.

Consider this contrived method, which I’ll use as an example of iterating all combinations. While this is a small example, consider code with a similar lack of context, except it’s 100 lines long with nesting depth up to five levels deep, and up to ten independent variables.
```java
    public static String doThis(boolean a, boolean b, boolean c) {
        String returnValue = "";
        if (a && !b) {
            returnValue += "Fizz";
            if (c) returnValue += "Buzz";
        } else if (b || c) {
            returnValue = "Beep";
            if (a || b) returnValue += "Boop";
        } else {
            returnValue += "Splat";
        }
        return returnValue;
    }
```

Configurable entities would usually include Test Double behaviors and arguments with multiple behaviors and parameter values for each coving as many [equivalence partitions](https://jhumelsine.github.io/2024/08/08/bdd.html#equivalence-partitioning) as I could. The test would consist of nested `for` loops for each configurable entity, and then at the deepest nesting the configurable entities would be passed as arguments to a driver method which contained the test in __Given/When/Then__ format. The driver method would initialize the Test Doubles in the Given section with the passed parameter values and call the public method being tested with passed parameter values as well.

It doesn’t take too many nested configurable entities iterating through their behaviors for the number of calls to the test driver becomes huge. It was not uncommon for the number of test driver calls to reach hundreds or thousands of potential configurations. Rather than completing in seconds, these tests could take minutes – many minutes. It was the price I was willing to pay.

While the test driver method was organized in the __Given/When/Then__ structure, it didn’t tend to look like the traditional unit tests. Traditional unit tests tend to be straight to the point. Test driver code included more nuance and logic to cover all possible combinations.

I would usually start the test driver without parameters where the configuration resided in the test driver itself, like it does in traditional unit tests. Then I’d declare a configurable element in the test driver method signature and iterate through a few possible values in the test that delegated to it. One of the configuration argument values would cause the test to fail on one of its iterations. I would have to include logic by assertions and verifications in the __Then__ portion of my tests to consider different possible configuration values.

I would add one more configurable entity at a time until I had all possible configuration combinations were covered. This often required more sophisticated logic in the assertions and verifications.

The test driver may have to accommodate thousands of configuration combinations. It may take minutes before they’re all handled correctly. The test driver may iterate hundreds of combinations before failing on one of the configurations, but which one failed?

I often added a print statement at the top of the test that logged the parameter values to the console. If a combination failed, then its parameter values would be the last ones on the log.

Here’s an example of a test and test driver that iterates through all possible combinations of `doThis`. __NOTE:__ There is no need for Test Doubles for this test, since `doThis` has not external dependencies:
```java
    private static void testDriverForDoThisInAllPossibleCombinations(boolean a, boolean b, boolean c) throws Exception {
        System.out.format("doThis(a=%b, b=%b, c=%b)\n", a, b, c);

        // Given-When
        String result = LegacySoftwareUnderTest.doThis(a, b, c);

        // Then
        assertEquals(getExpectedReturnValue(a, b, c), result);
    }

    // NOTE: This logic emerged one test iteration at a time until all combinations were passing.
    private static String getExpectedReturnValue(boolean a, boolean b, boolean c) {
        if (a && !b && !c) return "Fizz";
        if (!a && !b && !c) return "Splat";
        if (!a && !b) return "Beep";
        if (!b) return "FizzBuzz";
        return "BeepBoop";
    }

    public static void testDoThisInAllPossibleCombinations() throws Exception {
        for (Boolean a : Arrays.asList(true, false)) {
            for (Boolean b : Arrays.asList(true, false)) {
                for (Boolean c : Arrays.asList(true, false)) {
                    testDriverForDoThisInAllPossibleCombinations(a, b, c);
                }
            }
        }
    }

```

When there were thousands of iterations, it might also take a long time to reach the failing combination. When that occurred, I would create a separate method that called the test driver using the failed combination so I could more easily isolate it and focus upon getting it to pass.

Here’s an example of running the test driver for one specific iteration, which would allow me to focus upon this single scenario in isolation:
```java
    public static void testSpecificCombination() throws Exception {
        testDriverForDoThisInAllPossibleCombinations(true, false, false);
    }
```

These iterating tests tended to replicate the implementation to some degree, but I always tried to keep the test driver’s implementation different than the tested code. In a way, the test driver was rewriting the code being tested without having to touch the code being tested. Hidden behaviors would sometimes emerge within the test driver.

Iterating tests are brittle. I often felt a little _dirty_ in creating them, but they provided peace of mind that I had all possible paths covered when I didn’t understand the code.

# Updating Legacy Code
Before working on the desired modification, i.e., the bug or the new feature, you may want to consider [refactoring](https://refactoring.guru/refactoring) the current code first.

## Refactoring
Refactoring is changing the structure of the code without changing its behavior. We may want to refactor legacy code before making updates to make it easier to accommodate the updates.

_For each desired change, make the change easy (warning: this may be hard), then make the easy change._ — [Kent Beck](https://x.com/KentBeck/status/250733358307500032?lang=en)

Since `doThis` has coverage, I refactored it with the confidence that if my refactoring was incorrect, then one of the iterating tests would catch it.

No intent has emerged during refactoring, since there wasn’t any original intent. However the refactored code is less complex. If there had been intent in the original code, it may have emerged:
```java
    public static String doThis(boolean a, boolean b, boolean c) {
        if (b) return "BeepBoop";
        if (a) return c ? "FizzBuzz" : "Fizz";
        if (c) return "Beep";
        return "Splat";
    }
```

Adding tests and refactoring legacy code align with these two practices:
* __The Method Use Rule__ - This rule helps you figure out which legacy code requires test coverage. _Before you use a method in a legacy system, check to see if there are tests for it. If there aren’t, write them._ - Michael Feathers
* [__The Camping Rule__](https://deviq.com/principles/boy-scout-rule) - This rule helps you determine what to do in that legacy code. _Always leave the code cleaner than you found it._ - Uncle Bob Martin

Legacy code won't fix itself. A code base with years or decades of legacy code may require additional effort for code rot abatement. You cannot make the system perfect. You cannot make it good. What you can do is make it less bad. Focus upon steering it into the right direction, applying _The Camping Rule_ when we can. Eventually as we steer in the right direction, the existing code base will become less brittle.

Refactoring can be rewarding. You will find bugs lurking in the code that no one spotted before. You will bring order to chaos. You may even take a tangled mess of code and convert it into something beautiful and elegant.

## Characterization Tests Are Brittle
The more you refactor code to convey its intent, the more likely you are for the Characterization Tests to break, because they are brittle. Characterization Tests are brittle, since they are shaped by the implementation and not by behavior. As intent and behavior emerge, write tests that specify the emerging behavior and retire the characterization tests.

Characterization Tests provided initial coverage, but they may only be steppingstones in the process. For example, iterating tests for `doThis` might be replaced with:
* `doThis_ReturnsBeepBoop_WhenBIsTrue()`
* `doThis_ReturnsFizzBuzz_WhenAAndCAreBothTrue()`
* `doThis_ReturnsFizz_WhenAIsTrueAndCIsFalse()`
* `doThis_ReturnsBeep_WhenCIsTrueAndAIsFalse()`
* `doThis_ReturnsSplat_WhenAAndBAndCAreFalse()`

## … then make the easy change
Once the legacy code is covered, and refactored a bit, you can start working on the desired modification. Regardless of whether this is to fix a bug or add a new feature, you can follow a more traditional TDD/BDD process.

For a bug, create a test that replicates the incorrect behavior. Then update the test so that it defines the desired behavior, which will fail. For the new behavior, create a behavior specifying failing test.

Now that we have coverage, refactored code and failing tests for the task at hand, we are much closer to a TDD/BDD scenario. We can modify the legacy code to get the tests to pass and refactor the updated code once all tests pass.

Intent and behavior may start to emerge through refactoring. Refactoring may take you a long way toward making the legacy code more comprehendible, but it will only take you so far. A refactored implementation may start to suggest better design options, which may involve reorganizing classes.

Since unit tests tend to specify behaviors within classes, any class redesign will reorganize where those behaviors reside. The unit tests may need reorganization as well so that they specify the behaviors in the redesigned classes. This may require new tests, removing existing tests, splitting existing tests, etc.

## Hey. This doesn’t look right.
Hidden behaviors and intent may start to emerge through refactoring. This will make the legacy code more maintainable.

Sometimes the behaviors revealed may not look correct. A hidden bug may have been exposed.

Here’s a situation that I encountered while working on some legacy code.

The feature managed a set backend batch tasks. A background manager processed a set of tasks each with a unique integer identifier. The batch manager identified tasks that passed, failed and were interrupted while being processed. It recorded this information and returned it in a `BatchResults` object, which was a straightforward class that looked something like this:
```java
class BatchResults {
    private List<Integer> passed = new LinkedList<>();
    private List<Integer> failed = new LinkedList<>();
    private List<Integer> interrupted = new LinkedList<>();

    public void addPassed(int id) {
        passed.add(id);
    }
    public void addFailed(int id) {
        passed.add(id);
    }
    public void addInterrupted(int id) {
        passed.add(id);
    }
    public int getPassed() {
        return passed;
    }
    public int getFailed() {
        return failed;
    }
    public int getInterrupted() {
        return interrupted;
    }
}
```

This didn’t look right to me. I spun my chair around to address my teammate who sat behind me and asked, “Hey, Ron. Can you look at this? It doesn’t look right to me. The failed and interrupted ids are being added to the passed List.”

“Oh, that’s not right,” Ron replied, “Gee, I wonder how many failed and interrupted ids were incorrectly recorded as passed.”

`BatchResults` didn’t have any unit tests. I added failing tests for the failed and interrupted scenarios and then fixed the code.

This was an obvious error, and Ron had confirmed it. But sometimes the legacy code may reveal behavior and intent that looks questionable, and it’s not as obvious as the error in `BatchResults`. Do not ___fix___ this code, since it may not be broken. Many of us don’t have the domain knowledge to make that call within legacy code. Remember the old adage: ___That’s not a bug. It’s a feature.___ You may ___fix___ something that a customer is depending upon.

However, we don’t just ignore it either. Here’s my recommendation:
* Create a passing test that demonstrates the questionable behavior, but give it a name that questions that behavior. For example, if Ron didn’t confirm the `BatchResults` error, I would have created a test with the name: `getPassed_ReturnsIdsAddedViaAddFailedAndAddInterrupted_SHOULD_IT`. Adding `SHOULD_IT` change the indicative form of the test name into a question.
* Create a ticket just that describes the questionable behavior, why you question it, and reference the `SHOULD_IT` test.

Hopefully the ticket will be assigned to someone who has more domain knowledge than you. If the behavior is correct as-is, then the only update needed is to remove `SHOULD_IT` from the test name, and now we have a test that declares that the behavior is correct, even if it might look a bit shady.

If the behavior is incorrect, then a new failing test that defines the correct behavior should be created. The implementation can then be modified to pass the new test, which will cause the previous passing `SHOULD_IT` test to fail. The `SHOULD_IT` test can then be removed.

Let’s return to `BatchResults` briefly. It didn’t have any unit tests, but should it? There’s a unit testing community that believes that it’s a waste of time to test simple code. Devote your efforts toward covering more complex code. Accessors are a prime example where you often don’t see test coverage. Accessors will be tested indirectly as they are invoked in code that is unit tested.

I’m of the opinion that any code that’s created by hand should be tested. While `BatchResults` is not complex, its error lurked there for months. While I don’t know how the error was introduced, I’d be willing to bet that `addPassed(int id)` was copied-and-pasted as `addFailed(int id)` and `addInterrupted(int id)` and only the method name was updated.

“It doesn’t need to be unit tested,” I can here the developer say, “It’s obviously correct.” It only _looked_ correct, because previous code reviewers saw what they thought should be there rather than what was actually there. My fresh eyes spotted the obvious error.

“If there had been unit tests for the Batch Processor, then it would have found the bug for sure.” Probably true. However, there were no unit tests for the Batch Processor either. And had there been unit tests for the Batch Processor when those tests failed, how long would it have taken anyone to think to look at the `BatchResults` class as the source of the error?

I wrote unit tests for `BatchResults` in about five to ten minutes. Would that have been less than the time it would have taken to debug any Batch Processor tests? Would it have been worth the time as compared to having missed failed and interrupted ids several months?

I feel that an ounce of prevention is worth a pound of cure.

# Summary

# References
https://understandlegacycode.com/ - This is a landing page created by Nicolas Carlo dedicated to legacy code.
https://www.legacycode.rocks/ - This is a landing page for Legacy Code Rocks!, which features a Slack group, podcast, newsletter, and more.
https://understandlegacycode.com/blog/key-points-of-working-effectively-with-legacy-code/
https://x.com/nicoespeon
https://bsky.app/profile/nicoespeon.com
https://www.google.com/search?q=Legacy+code
https://www.google.com/search?q=characterization+tests
https://en.wikipedia.org/wiki/Characterization_test
https://en.wikipedia.org/wiki/Legacy_system
https://michaelfeathers.silvrback.com/characterization-testing
https://michaelfeathers.silvrback.com/bio
https://en.wikipedia.org/wiki/Given-When-Then
https://refactoring.guru/extract-method
https://blog.codinghorror.com/the-big-ball-of-mud-and-other-architectural-disasters/
https://refactoring.guru/refactoring
https://x.com/KentBeck/status/250733358307500032?lang=en

# Complete Code Example
Here's the complete Java code for the iterating tests. I haven't used a test framework. You should be able to copy-and-paste the entire Java program into any Java environment and run it:
```java
import java.util.*;

public class LegacyCode {
    public static void main(String[] args) throws Exception {
        Test.test();
    }
}

class LegacySoftwareUnderTest {
    public static String doThisOriginal(boolean a, boolean b, boolean c) {
        String returnValue = "";
        if (a && !b) {
            returnValue += "Fizz";
            if (c) returnValue += "Buzz";
        } else if (b || c) {
            returnValue = "Beep";
            if (a || b) returnValue += "Boop";
        } else {
            returnValue += "Splat";
        }
        return returnValue;
    }

    public static String doThis(boolean a, boolean b, boolean c) {
        if (b) return "BeepBoop";
        if (a) return c ? "FizzBuzz" : "Fizz";
        if (c) return "Beep";
        return "Splat";
    }
}

class Test {
    public static void test() throws Exception {
        testSpecificCombination();

        testDoThisInAllPossibleCombinations();
    }

    private static void testDriverForDoThisInAllPossibleCombinations(boolean a, boolean b, boolean c) throws Exception {
        System.out.format("doThis(a=%b, b=%b, c=%b)\n", a, b, c);

        // Given-When
        String result = LegacySoftwareUnderTest.doThis(a, b, c);

        // Then
        assertEquals(getExpectedReturnValue(a, b, c), result);
    }

    private static String getExpectedReturnValue(boolean a, boolean b, boolean c) {
        if (a && !b && !c) return "Fizz";
        if (!a && !b && !c) return "Splat";
        if (!a && !b) return "Beep";
        if (!b) return "FizzBuzz";
        return "BeepBoop";
    }

    public static void testDoThisInAllPossibleCombinations() throws Exception {
        for (Boolean a : Arrays.asList(true, false)) {
            for (Boolean b : Arrays.asList(true, false)) {
                for (Boolean c : Arrays.asList(true, false)) {
                    testDriverForDoThisInAllPossibleCombinations(a, b, c);
                }
            }
        }
    }

    public static void testSpecificCombination() throws Exception {
        testDriverForDoThisInAllPossibleCombinations(true, false, false);
    }
    
    private static void assertEquals(String expected, String actual) throws Exception {
        if (!expected.equals(actual)) {
            System.out.format("expected=%s, actual=%s\n", expected, actual);
            throw new Exception();
        }
    }

}
```
