---
title: DRAFT – Approval Testing - A Test Strategy for those who are reluctant to try Test-Driven Development
description: I’m the Design Pattern Evangelist, and I APPROVE this message
unlisted: true
---

<img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExamR2MHU4MThldzR0Ym5pMzhpb3B2bXo3N252azE0aTlpZGRqd3VvdSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/7lyvQ60pEKBmE/giphy.gif" alt="The Flintstones" title="Image Source: https://giphy.com/gifs/party-7lyvQ60pEKBmE" width = "70%" align="center" style="padding-right: 20px;">

# Introduction
Some developers, often seniors, just cannot make the adjustment to [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) (TDD). They have to write the code and see it work first. Maybe there's another way to provide a testing strategy for those _dinosaurs_ who need to see the working code first by practicing __Approval Testing__.

__[Unit Testing](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) declares behavior specifications.__ 

__[Characterization Testing](https://jhumelsine.github.io/2025/03/24/legacy-code.html#characterization-tests) reveals existing behavior.__ 

__[Approval Testing](https://approvaltests.com/) approves emerging behavior as it’s being implemented.__

All three types of tests look similar, since they feature the [Given/When/Then](https://en.wikipedia.org/wiki/Given-When-Then) format. The distinction among the three practices resides in how the __Then__ portion of each test is created. This is why I specifically used the term _Testing_, a process, rather than _Test_, an artifact of the testing process.

<img src="/assets/ApprovalTestingVennDiagram.png" alt="Approval Testing Venn Diagram" title="Rectangles were wasier to work with than ovals" width = "35%" align="right" style="padding-right: 35px;">

These three processes mainly differ as follows:
* For _Unit Testing_, the __Then__ section specifies ___desired behavior___.
* For _Characterization Testing_, the __Then__ section documents ___existing behavior___.
* For _Approval Testing_, the __Then__ section documents ___emerging behavior___.

I’ve written blog entries about [Unit Testing](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) and [Characterization Testing](https://jhumelsine.github.io/2025/03/24/legacy-code.html#characterization-tests) previously.
Approval Testing can be thought of as the intersection of Unit and Characterization Testing in that __desired__ behavior is __approved__ in its __existing__ form as it __emerges__.
I’ll focus upon Approval Testing for the rest of this blog entry.

# ..., but I know it when I see it
[_I can’t define pornography, but I know it when I see it._]( https://en.wikipedia.org/wiki/I_know_it_when_I_see_it) — Paraphrasing [__United States Supreme Court Justice Potter Stewart__](https://en.wikipedia.org/wiki/Potter_Stewart)

Much like Justice Stewart, many developers are not comfortable defining behavior specifications via the TDD process, but they know the desired behavior when they see it.

Approval Testing allows developers to write their code, validate it via their own observation and then document that validated behavior observation in a __Given/When/Then__ test.

If a subsequent implementation update causes an Approval Test to fail, then the developer must observe the new _failing_ behavior and decide whether it violates the previous approved behavior or whether it’s demonstrating new desired behavior that’s emerging. If the new emerging behavior is correct, then a new behavior assertion replaces the previous approved one.

## Yabba Dabba Doo
I featured [Characterization Testing](https://jhumelsine.github.io/2025/03/24/legacy-code.html#characterization-tests) as a technique to add tests to existing legacy code. In my brief description, I described setting up an assert declaring that an obviously invalid String, such as _Fred Flintstone_, would be the return value from a method. Then when the legacy code returned its actual String value, we could replace _Fred Flintstone_ with the actual value.

Let’s tweak that scenario a bit. Let’s assume that you’re working for [__Hanna-Barbera__](https://en.wikipedia.org/wiki/Hanna-Barbera), the cartoon production company, and you’ve been assigned to the [__YabbaDabbaDoo__](https://en.wikipedia.org/wiki/Yabba_Dabba_Doo) feature.

Given that you’re still prototyping, you may not have quite enough domain knowledge to feel confident enough to specify behavior using TDD. However, when you observe emerging behavior while prototyping, then you can document that behavior in a test.

You putter around a bit and observe that one of the methods returns _Fred Flintstone_, which looks like it’s doing what you’d expect it to do for that scenario.

__STOP!__ 

Don’t proceed with more code until you’ve documented this observation in a test. Create a __Given/When/Then__ test that asserts that _Fred Flintstone_ is returned. You may need to create the test from scratch, or maybe you’ve already been working on the __Given/When__ sections of a test, and you only need to complete the it with the __Then__ section that asserts _Fred Flintstone_.

You can then proceed with additional development making sure to always stop and document what we’re observing via tests. You should also refactor as you proceed to keep the code clean.

As you prototype with Approval Testing, you may become more familiar with the domain. You may become familiar enough to move from Approval Testing to TDD.

## Distinction between Approval and Characterization Testing
When I first saw presentations about Approval Testing, I thought the presenters were describing the Characterization Testing process by a different name. Using two terms for the same testing process seems to be a common practice based upon blog entries and videos by others. I think many view Approval and Characterization Tests as two names for the same process. I initially thought this too myself.

After having thought about Approval and Characterization Tests over the years, I feel that the two processes, while very similar, are different in a subtle way.

Existing behaviors are recorded in Characterization Tests as-is. They reveal and document existing behavior, usually in legacy code, whether it’s right or wrong. Any questionable behavior should be codified in a Characterization Test as-is and flagged by adding `SHOULD_IT` in its name as described in [Hey. This doesn’t look right.](https://jhumelsine.github.io/2025/03/24/legacy-code.html#hey-this-doesnt-look-right)

Observable behaviors are recorded in Approval Tests only when the developer has approved the observed behaviors as desired as they emerge from the maturing code.

Both practices are similar procedures, in that they both document behavior emerging from the implementation. The main difference is in how long that behavior has been within the code. For Characterization Testing the behavior has been there for days, months or years. For Approval Testing the behavior has been there for only seconds or minutes.

## Find Gaps with Mutation Testing
Approval Testing is reactionary testing. It doesn't drive the implementation. The implementation drives it. Therefore, if practicing Approval Testing, it may also be a good idea to include [Mutation Testing](https://jhumelsine.github.io/2025/03/28/mutation-testing.html) to help identify any behavior that the Approval Tests may have missed.

# Some Behaviors Require Observation
Some behaviors are difficult to specify. They must first be observed, and then they can be codified in an automated test. Graphical User Interfaces (GUIs) would be in this category.

GUIs are notoriously difficult to test. Using Approval Testing, the developer would execute the code and visually confirm the GUI and adjust the code until the GUI looks correct. It can be approved in an Approval Test, but how do we test something visual?

The Approval Test doesn’t approve the GUI directly. It approves the content being rendered to produce the GUI. For example, if the GUI’s form factor is a web browser, the content that’s rendered  by the browser could be the ASCII that’s in an `HTML` file. The ASCII content `HTML` file could be validated in one large String comparison assert.

## Yabba Dabba Doo Once More

<img src="https://live.staticflickr.com/3145/2970400508_dbf3ef8861_b.jpg" alt="Fred Flintstone" title="Image Source: https://www.flickr.com/photos/andertoons-cartoons/2970400508" width = "25%" align="right" style="padding-right: 20px;">

Let’s return to our __Hanna-Barbera__ project. Given that the customer’s domain is cartoons, they are going to want to feature cartoon images in their GUI. So rather than just returning _Fred Flintstone’s_ name, the customer will want to see his image.

You can easily launch the GUI and verify that you're seeing an image of Fred:
* When you expect to see him
* Where you expect to see him
* And in the size in which you expect to see him

This isn’t easily automated as a test. But we can automate the ASCII that will render Fred's image via the GUI.

For example, Fred’s rendering to the right is defined as padded on the right and being 25% of the width of the window. Here’s the __HTML__ code that renders it. You could  create a test that confirms the generated __HTML__ for how Fred should be rendered matches the following:
```html
<img src="https://live.staticflickr.com/3145/2970400508_dbf3ef8861_b.jpg" alt="Fred Flintstone"
    title="Image Source: https://www.flickr.com/photos/andertoons-cartoons/2970400508"
        width = "25%" align="right" style="padding-right: 20px;">
```

## Humble Object Teaser
Testing the source of the GUI rather than the GUI itself is one example of the Humble Object Pattern. There are more. I will describe this soon in an upcoming blog (TBD).

# Approval Testing via String Comparisons
While not a requirement for Approval Testing, Approval Tests often have one assertion, which may be based upon asserting the `toString()` result of a complex object against an expected value. For example, the GUI assert could be a comparison of the entire `HTML` file ASCII content as one long String as described previously.

I use this technique fairly often I'm still practicing what I'd consider TDD, but it's probably closer to Approval Testing. I start with the test first and populate all three __Given/When/Then__ sections. However, parts of the __Then__ section are still left undefined. Here's an example from one of my [Advent of Code](https://adventofcode.com/) tests from [2024 Day 14 - Warehouse Woes](https://adventofcode.com/2024/day/15), which I previously described in [A House Divided - Advent of Code](https://jhumelsine.github.io/2025/03/07/house-divided.html#advent-of-code).

This is one of my tests for Part 1:
```java
public moveRobotToLeftAlsoMovesBoxes() {
    // Given
    Warehouse warehouse = new Warehouse();
    warehouse.add(new Wall(new Position(0, 0)));
    warehouse.add(new Box(new Position(2, 0)));
    warehouse.add(new Box(new Position(3, 0)));
    warehouse.add(new Robot(new Position(4,0)));

    // When
    warehouse.move(MoveDirection.LEFT);

    // Then
    assertEquals("TBD", warehouse.toString());
}
```

`Warehouse.toString()` returns a String value of its configuration. When I ran the test, it failed, stating that `TBD` was not the actual value. The actual value was `Robot:(3,0), {(2,0)=Box:(2,0), (1,0)=Box:(1,0)}, {(0,0)=Wall:(0,0)}`. I visually confirmed that that was the expected value, and I updated the test as follows:

```java
public moveRobotToLeftAlsoMovesBoxes() {
    // Given
    Warehouse warehouse = new Warehouse();
    warehouse.add(new Wall(new Position(0, 0)));
    warehouse.add(new Box(new Position(2, 0)));
    warehouse.add(new Box(new Position(3, 0)));
    warehouse.add(new Robot(new Position(4,0)));

    // When
    warehouse.move(MoveDirection.LEFT);

    // Then
    assertEquals("Robot:(3,0), {(2,0)=Box:(2,0), (1,0)=Box:(1,0)}, {(0,0)=Wall:(0,0)}", warehouse.toString());
}
```

When it came to Part 2, I used a more visual representation for the `Warehouse`. The return value is the String representation of a List of rows, each of which is a String that identifies the elements on that row with __#__ for a `Wall`, __Bb__ for a `Box` and __@__ for the `Robot`, which mostly match the graphics used in the description of the problem from Advent of Code. 

From what I remember, I used TDD when creating this test by providing what I thought the String representation of the `warehouse` rendering would be, but I used Approval Testing to compare my expected String against the actual String. That is, when the test failed, I didn't assume the code was incorrect. I double checked my original expected String with the actual returned String, since I could have easily made a mental typo when typing in the expected value manually.
```java
public moveRobotUpMovesOtherBoxesUp() {
    int widthFactor = 2;
    Warehouse warehouse = new Warehouse(widthFactor);
    warehouse.add(new Wall(new Position(0, 0), widthFactor));
    warehouse.add(new Box(new Position(1, 2), widthFactor));
    warehouse.add(new Box(new Position(0, 3), widthFactor));
    warehouse.add(new Box(new Position(2, 3), widthFactor));
    warehouse.add(new Box(new Position(1, 4), widthFactor));
    warehouse.add(new Robot(new Position(1,5)));
    assertEquals("[##.., ...., .Bb., BbBb, .Bb., .@..]", warehouse.getRendering().toString());

    warehouse.move(MoveDirection.UP);
    assertEquals("[##.., .Bb., BbBb, .Bb., .@.., ....]", warehouse.getRendering().toString());
}
```

# Summary
Approval Testing offers an alternative to traditional Test-Driven Development by allowing developers to validate and document behavior as it emerges. Positioned between Unit and Characterization Testing, this method is particularly useful for developers who prefer to write code first and approve expected outcomes after observation. By approving behavior iteratively, developers can maintain confidence in their code while gradually transitioning to more structured testing approaches.

# References
Here are some resources:
* [Approval Tests](https://approvaltests.com/) - Website dedicated to Approval Tests with their own set of [Resources](https://approvaltests.com/resources/)
* [What's the difference between Regression Tests, Characterization Tests, and Approval Tests?](https://understandlegacycode.com/blog/characterization-tests-or-approval-tests/) - Nicolas Carlos states that Characterization and Approval Tests are the same, but I feel there's a distinction
* [Unlocking the Power of Approval Testing](https://dev.to/sergiomarcial/unlocking-the-power-of-approval-testing-a-comprehensive-guide-for-software-engineers-3o6f) - Blog by Sergio Marcial
* [Approval Testing: Benefits, How to Perform & Tools](https://testsigma.com/blog/approval-testing/) - Blog from Testsigma Engineering Team
* [Automate Approval Testing What It Is and How to Use It](https://www.qodo.ai/blog/automate-approval-testing/) - Blog by Talia Parnel
* [What is Approval Testing?](https://www.linkedin.com/pulse/what-approval-testing-john-ferguson-smart/) - Blog by John Ferguson Smart
* [How Approval Tests Measure Up Against Kent Beck’s Desiderata](https://www.youtube.com/watch?v=S71ku1VSik8) - Video by Emily Bache
* [The BEST way to find BUGS in an API: Contract vs Approval Testing](https://www.youtube.com/watch?v=Q1m01D8eMaU) - Video by Emily Bache
* ["Approval Testing" by Emily Bache (@emilybache)](https://www.youtube.com/watch?v=0ZVKcFsEp-4) - Presentation at The Legacy of SoCraTes
* [Gain CONFIDENCE With Approval ](https://www.youtube.com/watch?v=5H2s1knHUlA) - Video conversation between Dave Farley and Emily Bache
* [Add APPROVAL TESTING To Your Bag Of Tricks](https://www.youtube.com/watch?v=jAMVtMesHqk) - Video by Dave Farley
* and for more, Google: [Approval Testing](https://www.google.com/search?q=approval+testing)
