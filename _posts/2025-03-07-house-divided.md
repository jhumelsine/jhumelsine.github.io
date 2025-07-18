---
title: A House Divided Against Itself Will Not Stand
description: Resolving an inconsistency between my Design Process and TDD blog entries
---

<img src="https://live.staticflickr.com/6030/5966894496_cdfeb202cc_b.jpg" alt="The Thinker" title="Image Source: https://www.flickr.com/photos/mustangjoe/5966894496
" width = "60%" align="center" style="padding-right: 20px;">

# Introduction
I can’t begin to convey how much thought and how much I’ve struggled over this blog entry. I’ve been thinking about it for months. I’ve started and discarded several versions.

I realized I introduced a potential process inconsistency in previous blog entries. 
In [My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html), I described how my design process starts with pen and paper and my brain. My designs often include code snippets, which are implementation details I introduce before I’ve even touched a computer.
In [Writing Tests Before the Implementation](https://jhumelsine.github.io/2024/07/15/tdd.html), I described [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) (TDD), where I encouraged developers to write tests before any coding and let the implementation evolve.

This realization came to light last year in July when I had an extended Slack conversation with an online friend about my [Writing Tests Before the Implementation](https://jhumelsine.github.io/2024/07/15/tdd.html) blog. He often provides comments on my blog drafts, which I always appreciate. His comments often make my writing better, and even if I don’t necessarily agree with his comments, he forces me to think more deeply about what I have written.

He and I had a few differences of opinions about the TDD process. He thinks the process is a bit too restrictive, whereas I think the process constraints is what defines TDD and makes it valuable.

As I was championing the benefits of TDD to shape the implementation, I felt a twinge of insincerity, since this isn't quite what I use in my design process. Do I truly believe in TDD? Have I been shouting the praises of the TDD process without practicing them myself? Have I been a false prophet?

So, which is it? Should we start with a design, or should we start with TDD?

Like most interesting questions in software engineering, I believe the answer is: ___It depends.___

# Depends Upon What?
I don’t think design or TDD is an either-or choice. It mostly depends upon the context and scope of the problem being addressed.

My design process and TDD are [Tools in the Toolbox](https://jhumelsine.github.io/2023/08/29/toolbox.html). The challenge is knowing when to use them and when not to use them.

_You haven’t mastered a tool until you understand when it should not be used._ – [Kelsey Hightower](https://x.com/CodeWisdom/status/1354478025228873731)

## It’s Yes-And
I think that both practices are effective. I think it depends upon the scope of the problem being addressed.

## Design
[My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html) focuses upon domain elements and their relationships. This tends to result in a class design, but I’m not trying to implement a solution or even solve the problem. I’m trying to understand it by decomposing and understanding its domain elements, their relationships and interactions.

As the design matures, I find that the domain elements start to scream which behaviors they are responsible for. It's like writers who say that the characters in their novels take control and tell them where the story is going while they write it.

Sometimes the screaming domain elements are aligned with [design patterns](https://jhumelsine.github.io/3000/01/01/preface.html#design-patterns). The code snippets I mentioned previously are usually associated with the tried-and-true code of the design patterns I'm using.

## TDD
TDD works well when developers know enough to define behavior specifications that can be confirmed in the implementation. I have found that this knowledge is often a result of the design process. The list of behaviors doesn’t need to be complete. New behaviors may emerge while working through the TDD process as the domain is better understood. This is still a discovery process, and it may result in updates to the design as well.

Behavior specifying tests can be written directly prior to implementation as described in [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development). Or they can be written in conjunction with the implementation, as described in [The Three Laws (Rules) of TDD](https://jhumelsine.github.io/2024/07/15/tdd.html#the-three-laws-rules-of-tdd).

The scope of the implementation being specified via tests tends to be a class with its public methods and any of its dependencies.

# Advent of Code
Since retirement, I haven’t had many opportunities to compare design and TDD practices professionally. However, I try to stay somewhat active with coding challenges, such as [Advent of Code]( https://adventofcode.com/).

I’d like to describe two Advent of Code challenges where I used a TDD-First approach in one and a Design-First approach in the other and then compare the results.

## TDD First
Day 12’s challenge from 2024 was [Garden Groups](https://adventofcode.com/2024/day/12). In this challenge, we are given a garden layout with different regions of plant types, identified only by capital letters. Here’s their example of a five region garden:
```
AAAA
BBCD
BBCC
EEEC
```

The garden requires fencing around each region. Here’s their example that illustrates the fencing:
```
+-+-+-+-+
|A A A A|
+-+-+-+-+     +-+
              |D|
+-+-+   +-+   +-+
|B B|   |C|
+   +   + +-+
|B B|   |C C|
+-+-+   +-+ +
          |C|
+-+-+-+   +-+
|E E E|
+-+-+-+
```

The challenge involves calculating how many single fence segments would be required for the perimeter around each region and the area of each region.

I started with simple test, such as this single region garden:
```
A
```

It has a perimeter of four fence segments and an area of one:
```
+-+
|A|
+-+
```

I expanded the second test to:
```
AA
```

This is a garden with six fence segments and an area of two:
```
+-+-+
|A A|
+-+-+
```

I converged to the correct answer quickly in my implementation, from what I recall, as the tests specified more complex garden configurations.

However, I violated a major part of TDD. I didn’t refactor rigorously. I cleaned the code a bit, but for the most part I just worked toward getting the right answer. _Mea culpa. Mea culpa. Mea maxima culpa._ I'll pay the price for this shortly.

Here’s the code that calculates the perimeter and area. I should have known I was going to have issues when a parameter type is `Map<String, List<Set<Position>>>`. This method is only provided as an illustration. I don't expect anyone to understand it in the context of solving the challenge:
```java
    private int getResources(Map<String, List<Set<Position>>> crops) {
        int resources = 0;
        for (Map.Entry<String, List<Set<Position>>> entry : crops.entrySet()) {
            String crop = entry.getKey();
            List<Set<Position>> lists = entry.getValue();
            for (Set<Position> cropPositions : lists) {
                int area = cropPositions.size();
                int perimeter = 0;
                for (Position position : cropPositions) {
                    if (!crop.equals(getCrop(position.getRow()+1, position.getColumn()))) perimeter++; 
                    if (!crop.equals(getCrop(position.getRow()-1, position.getColumn()))) perimeter++; 
                    if (!crop.equals(getCrop(position.getRow(), position.getColumn()+1))) perimeter++; 
                    if (!crop.equals(getCrop(position.getRow(), position.getColumn()-1))) perimeter++; 
                }

                resources += area * perimeter;
            }
        }

        return resources;
    }
```

One of my issues with the Advent of Code format is that there’s a new challenge each day. It’s easy to fall behind. Solve it and move on. There’s not much incentive to keep code clean, except for Part 2 of each day's challenge.

Solving Part 1 for each day's challenge releases Part 2 for that day. Part 2 is a continuation of Part 1, so there is some continuity, but only for that day. Sometimes Part 2 can be solved with a single line update to Part 1's solution. Sometimes it requires a whole new approach.

If we knew Part 2 while working on Part 1, we might be able to design and implement Part 1 so that it can more easily accommodate Part 2. But we can neither see nor predict the future in Advent of Code just as we cannot for most software projects.

Part 2 of the Garden Groups, __Spoiler Alert__, updated the requirements such that a straight row of fencing would be one segment of fencing regardless of its length. The `A` garden would still have a perimeter of four segments, but now the `AA` garden would have a perimeter of four segments rather than the previous six. The fencing across the top and bottom would each be one segment that’s two units long rather than the two segments of one unit in length from Part 1.

The update for the `AA` garden is:
```
+---+
|A A|
+---+
```

I could easily add new tests for Part 2. However, I struggled quite a bit to get the Part 2 solution, since I didn't have a design for Part 1. I only had an implementation powered by TDD minus the refactoring step. I doubled down on the Part 1 solution and forced it to solve Part 2 without attempting to refactor or redesign. 

I am fully aware of how horrible this Part 2 code is that's listed below. I wrote it about three months ago, and I have no idea how it works anymore. There’s too much implementation detail and not enough design. The implementation is too tightly coupled with data structure concepts. I did not take any time to think about the design before I jumped into an implementation using TDD. It’s a micro version of a [Big Ball of Mud](https://en.wikipedia.org/wiki/Anti-pattern#Big_ball_of_mud). Even with coverage, I’d consider this poor legacy code.

If anyone were to try to refactor this code, at least they’d know immediately if they broke any behavior via the test coverage, but I doubt that the tests would assist them in understanding the implementation or what they had done wrong. Consider how challenging and frightening it would be to refactor this code without any test coverage?

Once more, this code is provided only to illustrate a point:
```java
    private int getSideResources(Map<String, List<Set<Position>>> crops) throws Exception {
        int resources = 0;
        for (Map.Entry<String, List<Set<Position>>> entry : crops.entrySet()) {
            String crop = entry.getKey();
            List<Set<Position>> lists = entry.getValue();
            for (Set<Position> cropPositions : lists) {
                int area = cropPositions.size();
                Set<String> sides = new TreeSet<>();
                for (Position position : cropPositions) {
                    int row = position.getRow();
                    int column = position.getColumn();
                    if (!crop.equals(getCrop(position.getRow()+1, position.getColumn()))) sides.add(String.format("%05d#%05d-%05d#%05d", row+1, column, row+1, column+1));
                    if (!crop.equals(getCrop(position.getRow()-1, position.getColumn()))) sides.add(String.format("%05d#%05d-%05d#%05d", row, column, row, column+1));
                    if (!crop.equals(getCrop(position.getRow(), position.getColumn()+1))) sides.add(String.format("%05d#%05d|%05d#%05d", row, column+1, row+1, column+1));
                    if (!crop.equals(getCrop(position.getRow(), position.getColumn()-1))) sides.add(String.format("%05d#%05d|%05d#%05d", row, column, row+1, column));
                }

                sides = reduceSides(crop, sides);
                resources += area * sides.size();
            }
        }

        return resources;
    }

    private Set<String> reduceSides(String crop, Set<String> sides) throws Exception {
        Set<String> reducedSides = null;
        boolean overallReduced = true;
        List<String> removedPosts = new LinkedList<>();
        while (overallReduced) {
            Set<String> firstSides = new TreeSet<>(sides);
            Set<String> secondSides = new TreeSet<>(sides);
            reducedSides = new TreeSet<>();
            overallReduced = false;
            for (String side1 : firstSides) {
                if (side1.contains("-")) {
                    String[] side1Split = side1.split("-");
                    String[] cropSplit = side1Split[1].split("#");
                    String cropUpperRight = getCrop(Integer.valueOf(cropSplit[0])-1, Integer.valueOf(cropSplit[1]));
                    String cropLowerLeft = getCrop(Integer.valueOf(cropSplit[0]), Integer.valueOf(cropSplit[1])-1);
                    String cropUpperLeft = getCrop(Integer.valueOf(cropSplit[0])-1, Integer.valueOf(cropSplit[1])-1);
                    String cropLowerRight = getCrop(Integer.valueOf(cropSplit[0]), Integer.valueOf(cropSplit[1]));
                    if (!removedPosts.contains(side1Split[0])) {
                        if (cropUpperLeft.equals(cropUpperRight) || cropLowerLeft.equals(cropLowerRight)) {
                            for (String side2 : secondSides) {
                                if (side2.startsWith(String.format("%s-", side1Split[1]))) {
                                    String[] side2Split = side2.split("-");
                                    reducedSides.add(String.format("%s-%s", side1Split[0], side2Split[1]));
                                    overallReduced = true;
                                    removedPosts.add(side1Split[1]);
                                } 
                            }
                        }
                        if (!removedPosts.contains(side1Split[0]) && !removedPosts.contains(side1Split[1])) reducedSides.add(side1);
                    }
                } else if (side1.contains("|")) {
                    String[] side1Split = side1.split("\\|");
                    String[] cropSplit = side1Split[1].split("#");
                    String cropUpperRight = getCrop(Integer.valueOf(cropSplit[0])-1, Integer.valueOf(cropSplit[1]));
                    String cropLowerLeft = getCrop(Integer.valueOf(cropSplit[0]), Integer.valueOf(cropSplit[1])-1);
                    String cropUpperLeft = getCrop(Integer.valueOf(cropSplit[0])-1, Integer.valueOf(cropSplit[1])-1);
                    String cropLowerRight = getCrop(Integer.valueOf(cropSplit[0]), Integer.valueOf(cropSplit[1]));
                    if (!removedPosts.contains(side1Split[0])) {
                        if (cropUpperLeft.equals(cropLowerLeft) || cropUpperRight.equals(cropLowerRight)) {
                            for (String side2 : secondSides) {
                                if (side2.startsWith(String.format("%s|", side1Split[1]))) {
                                    String[] side2Split = side2.split("\\|");
                                    reducedSides.add(String.format("%s|%s", side1Split[0], side2Split[1]));
                                    overallReduced = true;
                                    removedPosts.add(side1Split[1]);
                                } 
                            }
                        }
                        if (!removedPosts.contains(side1Split[0]) && !removedPosts.contains(side1Split[1])) reducedSides.add(side1);
                    }
                } else {
                    throw new Exception("Should not get here");
                }
            }
            sides = reducedSides;
        }
        
        return reducedSides;
    }
```

I rushed to stay on schedule. I didn't consider a design. I did not refactor. Advent of Code’s daily schedule is an artificial deadline. Deadlines are self-imposed by developers either trying to keep up with others or just stay on schedule even if it's an artificial deadline.

Advent of Code is a microcosm of software development. There is always deadline pressure. Developers are often pressured to get the code working and deliver without considering a design or refactoring.

TDD can only be effective when the code is refactored as part of the process. Without this discipline, the codebase has a tendency to slide into chaos.

## Design First
I decided to go with a Design-First approach with Day 15, [Warehouse Woes](https://adventofcode.com/2024/day/15). This challenge defines a rectangular warehouse space enclosed by walls with a few wall barriers inside the space. There are boxes distributed around the warehouse floor as well. A robot is given instructions to move up, down, left and right. If the robot encounters a box in front of it on its move, it also moves the box forward. If there are several boxes in a row in its path, then the robot will nudge all of them when it moves.

If a wall is directly in front of the robot, it will block the robot for that move and iterate through moves until it encounters a non-blocked move. If the box in front of a robot’s path is also blocked by the wall or another blocked box, then the robot and boxes are blocked for that move as well.

I could envision the progression of tests starting with simple Warehouse layouts and moving toward more complex ones; however, I restrained myself. I didn’t start with TDD immediately.

I thought about a design, which was simple enough to keep it in my head without having to sketch out a design on paper. My design included several domain elements: __Warehouse__, __WarehouseElement__, __Movable__, __Wall__, __Box__, __Robot__, __Position__ and __MoveDirection__ with relationships, such as:
* __Warehouse__ contains __WarehouseElements__
* A __WarehouseElement__ maintain its __Position__ within the __Warehouse__
* __Movable__ and __Wall__ are __WarehouseElements__
* __Movable__ is a __WarehouseElement__ that can move in a __MoveDirection__ and thus change its __Position__
* __Box__ and __Robot__ are __Movables__

As domain behaviors emerged from the design, it was usually obvious within which domain element their implementations belonged.

The core movement behavior resided in this recursive method:
```java
    private void move (Movable movable, MoveDirection direction) {
        String positionKey = movable.getPosition(direction).toString();
        if (walls.containsKey(positionKey)) return;
        if (boxes.containsKey(positionKey)) {
            Box box = boxes.get(positionKey);
            move(box, direction);
            boxes.remove(positionKey);
            boxes.put(box.getKey(), box);
        }
        if (!boxes.containsKey(positionKey)) movable.move(direction);
    }
```

The method is more domain rich than the Garden Groups code.

<img src="https://media.istockphoto.com/id/1388022879/photo/small-brick-pyramid-on-white-background.jpg?s=612x612&w=0&k=20&c=Oj5PImbfH1mlKmxXJ8fdZb5KV9CD_IATZyipWwHfJZ8=" alt="Pyramid of Bricks" title="Image Source: https://www.istockphoto.com/photos/brick-pyramid" width = "30%" align="right" style="padding-right: 20px;">

The second part of the Warehouse Woes, __Spoiler Alert__, added the stipulation that everything, except the robot, is twice as wide. That’s not too bad until you consider that two-width boxes could be arranged in a pyramid pattern where one box is atop a row of two, which are atop a row of three, etc. When the robot moves the top box down, it will move the entire pyramid box pattern down as well. If just one of those boxes is blocked by a wall, then the entire pyramid of boxes and the robot are blocked.

The `move` method for Part 1 only worked for single width WarehouseElements. It wasn’t obvious to me during the Part 1 design and implementation that the Part 1 solution had an implicit dependency upon single width WarehouseElements. This dependency became obvious once I learned that the domain model needed to support wider WarehouseElements.

I needed to think about my design a bit more. I added a width factor to WarehouseElement. The requirement was only for a width of two, but my design could support any width. The Robot could support varying width, since it was a WarehouseElement, but its width was always set to one, since that was a requirement constraint.

My first tests restricted width to one so that all my previous Part 1 tests would work in a Part 2 implementation as well. Once the width-one tests passed, I added width-two tests.

The refactored `move` method is mostly an extension of the Part 1 method but expanded to support a set of Movables. It also replaced some data structure implementation details with more domain rich methods, such as the `Intersecting` methods:
```java
    private void move(Set<Movable> movables, MoveDirection direction) {
        Set<Movable> newMovables = getNewMovables(movables, direction);

        if (isIntersectingWithWalls(newMovables)) return;

        if (isIntersectingWithBoxes(newMovables)) {
            Set<Movable> intersectingBoxes = getIntersectingBoxes(newMovables, movables);
            move(intersectingBoxes, direction);
            boxes.removeAll(movables);
            boxes.addAll(intersectingBoxes);
        }

        if (!isIntersectingWithBoxes(newMovables)) {
            for (Movable movable : movables) {
                movable.move(direction);
                updateBoundaries(movable);
            }
        }
    }
```

It took a little time to get this code working, but I had a clear path in mind as I made every adjustment with TDD keeping me on track. I didn’t spend much time debugging or figuring out the next step, since each update progressed logically.

# Bob Martin and John Osterhout Debate
I was still procrastinating on this blog entry, even if I felt I had resolved my contradictions. I wasn’t sure if I could justify my resolutions to an audience even if they felt right to me. Was I resolving the conflict or rationalizing it?

While procrastinating, I noticed a [post](https://x.com/unclebobmartin/status/1893659113525023115) on Twitter/X that featured a link to an online [Debate](https://github.com/johnousterhout/aposd-vs-clean-code/blob/main/README.md) between [Robert “Uncle Bob” Martin](https://en.wikipedia.org/wiki/Robert_C._Martin) and [John Ousterhout](https://en.wikipedia.org/wiki/John_Ousterhout) regarding the differences in their software design philosophies. Their debate focused upon three topics:
* Method/Function Length
* Comments
* Test-Driven Development

I stumbled upon one of Bob’s comments in the TDD section of their debate:

>My own experience is that design comes from strategic thought, which is independent of the tactical behavior of either TDD or Bundling. Design is taking one step back from the code and envisioning structures that address a larger set of constraints and needs.
>
>Once you have that vision in your head it seems to me bundling and TDD will yield similar results.

Aha! That was it! Design is thinking strategically about the problem. TDD is thinking tactically about the solution. Strategy is __what__ needs to happen. TDD is about __how__ to accomplish that.

Bob’s comment gave me the final piece of the puzzle to tie it all together and finally complete this blog entry.

__Side Note:__ Returning to the debate, it seemed like John wasn’t listening to Bob, when John said:
>It's hard to design something well if you don't think about the whole design problem at once. TDD explicitly prohibits developers from writing more code than is needed to pass the current test; this discourages the kind of strategic thinking needed for good design.

But that’s not what Bob is saying about TDD. You can step back and think about the overall design strategically.

John has a few more discouraging things to say about TDD, and then he drops this:

>You ask me to trust your extensive experience with TDD, and I admit that I have no personal experience with TDD. On the other hand, I have a lot of experience with tactical programming, and I know that it rarely ends well. TDD is one of the most extreme forms of tactical programming I've encountered.

In the same paragraph, he claims to have no personal experience with TDD, and yet it’s still one of the most extreme forms of tactical programming he’s encountered. I feel like John has his own inconsistency to resolve. I would respect his opinion more if he had said that he had tried TDD himself and found it lacking. His TDD opinion reeks of dogma.

Martin and Ousterhout reunited to continue their debate in a [group video](https://www.youtube.com/watch?v=3Vlk6hCWBw0) format.

# It Depends - Revisited
At the start of this blog, I said that choosing whether to start with design or TDD depends upon the context and scope of the problem being addressed. Here are a few of my guidelines:
* Start with __TDD__ when the problem is well defined with a narrow scope. I suspect that most data structures and algorithms studied in academia would fall into this category.
* Start with __Design__ when the problem's definition is a bit fuzzy with a broader scope. Design will bring the problem more sharply into focus by better understanding the domain. Then use __TDD__ to drive the implementation. __NOTE:__ Depending upon the problem's fuzziness, you may not even be able to begin with __Design__. The problem may require a [Use Case Diagram](https://en.wikipedia.org/wiki/Use_case_diagram) or other architectural analysis, until the components are understood well enough and their __Design__ may begin.
* Start with __Design__ if the problem is being implemented by a team. This will help identify the boundaries and API [contracts](https://jhumelsine.github.io/2025/06/10/contracts.html), which will more clearly identify what's expected of each component in the design and how it communicates its expectations and obligations.

Great software engineering isn’t about rigidly following one methodology but knowing when to switch tools. Whether you start with TDD or design first, the goal is the same: writing maintainable, reliable code.

# Summary

<img src="https://media.istockphoto.com/id/186421041/photo/the-gateway-arch-of-st-louis-missouri-taken-from-the-sky.jpg?s=612x612&w=0&k=20&c=Uc0L0xt7WQ80dHnsU8DkH8KTSU2MzhaVKuoloG26HcI=" alt="St. Louis Arch" title="Image Source: https://www.istockphoto.com/photos/st-louis-arch-fall" width = "30%" align="right" style="padding-right: 20px;">

I was quite conflicted at the start of this blog. Jesus said in Matthew 12:25, “A house divided against itself will not stand.” I was concerned that my design and TDD approaches were like a divided house.

Writing this blog entry was a bit of therapy. I worked through my inner conflict. Design is strategic thinking, and TDD is tactical thinking. We need both.

These two approaches are not opposing forces that will collapse upon themselves. They are two reinforcing forces like the sides of an arch pressing against and supporting one another making the whole stronger.
