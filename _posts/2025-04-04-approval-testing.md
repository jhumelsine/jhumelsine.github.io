---
title: DRAFT – Approval Testing - A Test Strategy for those who reluctant to try TDD
description: I’m the Design Pattern Evangelist, and I APPROVE this blog post
unlisted: true
---

<img src="https://media2.giphy.com/media/v1.Y2lkPTc5MGI3NjExamR2MHU4MThldzR0Ym5pMzhpb3B2bXo3N252azE0aTlpZGRqd3VvdSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/7lyvQ60pEKBmE/giphy.gif" alt="The Flintstones" title="Image Source: https://giphy.com/gifs/party-7lyvQ60pEKBmE" width = "60%" align="center" style="padding-right: 20px;">

# Introduction
Some developers, often seniors, just cannot make that adjustment to [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) (TDD). They have to write the code first. Maybe there's another way to accomodate testing for those _dinosaurs_ with __Approval Testing__.

[Unit Testing](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) declares behavior specifications. 

[Characterization Testing](https://jhumelsine.github.io/2025/03/24/legacy-code.html#characterization-tests) reveals existing behavior. 

[Approval Testing](https://approvaltests.com/) approves emerging behavior as it’s being implemented.

All three types of tests look similar, since they feature the [Given/When/Then](https://en.wikipedia.org/wiki/Given-When-Then) format. The distinction among the three practices resides in how the __Then__ portion of each test is created. This is why I specifically used the term _Testing_, a process, rather than _Test_, an artifact.

<img src="/assets/ApprovalTestingVennDiagram.png" alt="Approval Testing Venn Diagram" title="Rectangles were wasier to work with than ovals" width = "35%" align="right" style="padding-right: 35px;">

The processes among the three mainly differ as follows:
* For _Unit Testing_, the __Then__ section specifies ___desired behavior___.
* For _Characterization Testing_, the __Then__ section documents ___existing behavior___.
* For _Approval Testing_, the __Then__ section documents ___emerging behavior___.

I’ve written blog entries about [Unit Testing](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) and [Characterization Testing](https://jhumelsine.github.io/2025/03/24/legacy-code.html#characterization-tests) previously.
Approval Testing can be thought of as the intersection of Unit and Characterization Testing in that __approved__ behavior is __desired__ in its __existing__ form as it __emerges__.
I’ll focus upon Approval Testing for the rest of this blog entry.

# I know it when I see it
[_I can’t define pornography, but I know it when I see it._]( https://en.wikipedia.org/wiki/I_know_it_when_I_see_it) — Paraphrasing [__United States Supreme Court Justice Potter Stewart__](https://en.wikipedia.org/wiki/Potter_Stewart)

Much like Justice Steward, many developers are not comfortable defining behavior specifications via Unit Tests via the TDD process, but they know it when they see it.

Approval Testing allows developers to write their code, validate it via their own observation and then document that validated behavior observation in a __Given/When/Then__ test.

If a subsequent implementation update causes an Approval Test to fail, then the developer must observe the new _failing_ behavior and decide whether it violates the previous approved behavior or whether it’s demonstrating new desired behavior that’s emerging. If the new emerging behavior is correct, then a new behavior assertion replaces the previous approved one.

## Yabba Dabba Doo
I described [Characterization Testing](https://jhumelsine.github.io/2025/03/24/legacy-code.html#characterization-testing) as a technique to add tests to existing legacy code. In my brief description, I described setting up an assert declaring that an obviously invalid String, such as _Fred Flintstone_, would be the return value for a method. Then when the legacy code returned its actual String value, we could replace _Fred Flintstone_ with the actual value.

Let’s tweak that scenario a bit. Let’s assume that you’re working for [__Hanna-Barbera__](https://en.wikipedia.org/wiki/Hanna-Barbera), the cartoon production company, and you’re working on the [__YabbaDabbaDoo__](https://en.wikipedia.org/wiki/Yabba_Dabba_Doo) (YDD) feature.

You’re still prototyping YDD, so you may not have quite enough domain knowledge to feel confident enough to specify behavior using TDD. However, you can prototype some code, and when you observe emerging behavior you like, then you can document it in a test.

You putter around a bit and observe that the code is returned _Fred Flintstone_, which looks it’s doing what you’d expect it to do.

__STOP!__ Don’t proceed with more code until you’ve documented this observation in a test. Create a __Given/When/Then__ test that asserts that _Fred Flintstone_ is returned. You may need to create the test from scratch, or maybe you’ve already been working on the __Given/When__ sections, and you only need to complete the test with the __Then__ section that asserts _Fred Flintstone_.

You can then proceed with the code making sure to always stop and document what we’re observing via tests. You should also refactor as you proceed to keep the code clean.

As you prototype with Approval Testing, you may become more familiar with the domain. We you may become familiar enough to move from Approval Testing to TDD.

## Distinction between Approval and Characterization Tests
When I first saw presentations about Approval Tests, I thought the presenters were using an alternative name for Characterization Tests. Based upon blog entries and videos by others, this seems to be a common interpretation. I think many view Approval and Characterization Tests as two names for the same process.

After having thought about Approval and Characterization Tests over the years, I think the two processes are different with a subtle distinction.

Existing behaviors are recorded in Characterization Tests as-is. They reveal and document existing behavior, usually in legacy code, whether it’s right or wrong. Any questionable behavior should be codified in a Characterization Test as-is and flagged by adding `SHOULD_IT` in its name as described in [Hey. This doesn’t look right.](https://jhumelsine.github.io/2025/03/24/legacy-code.html#hey-this-doesnt-look-right)

Observable behaviors are recorded in Approval Tests only when the developer has approved the observed behaviors as desired as they emerge as the code matures.

## Find Gaps with Mutation Testing
Approval Testing is reactionary testing. It doesn't drive the implementation. The implementation drives it. Therefore, if practicing Approval Testing, it may also be a good idea to include [Mutation Testing](https://jhumelsine.github.io/2025/03/28/mutation-testing.html) to help find any behavior gaps that the Approval Tests may have missed.

# Some Behaviors Require Observation
Some behaviors are difficult to specify. They must first be observed, and then they can be codified in an automated test. Graphical User Interfaces (GUIs) would be in this category.

GUIs are notoriously difficult to test. Using Approval Testing, the developer would execute the code and visually confirm the GUI adjusting the code until the GUI looks correct. It can be approved in an Approval Test, but how do we test something visual?

The Approval Test doesn’t approve the GUI directly. It approves the content that is rendered to produce the GUI. For example, if the GUI’s form factor is a web browser, the content that’s rendered  by the browser could be the ASCII that’s in an `HTML` file. The ASCII content `HTML` file could be validated in one large String comparison assert.

While not a requirement for Approval Testing, Approval Tests often has one assertion, which may be based upon asserting the `toString()` result of a complex object against a known value. For example, the GUI assert could be a comparison of the entire `HTML` file ASCII content as one long String.

I use this techninque fairly often I'm still following what I'd consider TDD, but it's probably closer to Approval Testing. I'll start with the test first and write all three __Given/When/Then__ sections. However, parts of the __Then__ section are still left undefined. Here's an example from one of my [Advent of Code](TBD) tests from [2024 Day 14 - Warehouse](TBD):
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

`Warehouse.toString()` returns a string value of its configuration. When I ran the test, it failed, stating that `TBD` was not the expected value. The actual value was `Robot:(3,0), {(2,0)=Box:(2,0), (1,0)=Box:(1,0)}, {(0,0)=Wall:(0,0)}`. I visually confirmed that that was the expected value, and I updated the test as follows:

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

## Yabba Dabba Doo Once More

<img src="https://live.staticflickr.com/3145/2970400508_dbf3ef8861_b.jpg" alt="Fred Flintstone" title="Image Source: https://www.flickr.com/photos/andertoons-cartoons/2970400508" width = "25%" align="right" style="padding-right: 20px;">

Let’s return to our __Hanna-Barbera__ project. Given that the customer’s domain is cartoons, they are going to want to feature them in their GUI. So rather than just return _Fred Flintstone’s_ name, the customer will want to see his image.

You can easily launch the GUI and see an image of Fred when we expect to see him, where you expect to see him, how large you expect him to be, but this isn’t easily automated as a test. What we can do is confirm the ASCII that will render this image via the GUI.

For example, Fred’s rendering for this page is defined as padded on the right and being 25% of the width of the window. Here’s the __HTML__ code that renders it. You could easily create a test that confirms that any code that generates specifications for how Fred should be rendered by comparing it to this specification:
```md
<img src="https://live.staticflickr.com/3145/2970400508_dbf3ef8861_b.jpg" alt="Fred Flintstone"
    title="Image Source: https://www.flickr.com/photos/andertoons-cartoons/2970400508" width = "25%" align="right" style="padding-right: 20px;">
```

## Humble Object Teaser
Testing the source of the GUI rather than the GUI itself is one example of the Humble Object Pattern. There are more. I will blog about this soon in an upcoming blog (TBD).

# Summary
__TBD__

# References
__TBD__
