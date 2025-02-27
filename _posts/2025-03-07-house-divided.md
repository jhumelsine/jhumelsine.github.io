---
title: DRAFT – A House Divided Against Itself Will Not Stand
description: How I resolve a possible inconsistency in the processes I profess
unlisted: true
---

<img src="https://live.staticflickr.com/6030/5966894496_cdfeb202cc_b.jpg" alt="The Thinker" title="Image Source: https://www.flickr.com/photos/mustangjoe/5966894496
" width = "60%" align="center" style="padding-right: 20px;">

# Introduction
I can’t begin to convey how much thought, and how much I’ve struggled over this blog entry. I’ve been thinking about it for months. I’ve started and discarded several versions.

I realized I introduced a potential process inconsistency in my blogs. This realization occurred last year in July when I had an extended Slack conversation with an online friend about my [Writing Tests Before the Implementation](https://jhumelsine.github.io/2024/07/15/tdd.html) blog, where I described [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) (TDD). He often provides comments on my blog drafts, which I always appreciate. His comments often make my writing better, and even if I don’t necessarily agree with his comments, he forces me to think more deeply about what I have written.

He and I had a few differences of opinions about the TDD process. He thinks the process is a bit too restrictive, whereas I think the process constraints is what defines TDD and makes if valuable.

As I was championing the benefits of TDD to shape the implementation with my friend on Slack, I felt a twinge of insincerity. Did I truly believe this? Have I been shouting the praises of the TDD process without practicing them myself? Have I been a false prophet?

Right before I started my [Automated Testing](https://jhumelsine.github.io/3000/01/01/preface.html#automated-testing) series, I had posted [My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html) where I describe how my design process starts with pen and paper and my brain. My designs often include code snippets, which are implementation details before I’ve even touched a computer. That’s code that has not been created using the TDD process.

So, which is it? Should we start a design, or should we start with TDD?

Like most interesting questions in software engineering, I believe the answer is: ___It depends.___

# Depends Upon What?
My design process and TDD are [Tools in the Toolbox](https://jhumelsine.github.io/2023/08/29/toolbox.html). The challenge is knowing when to use them and when not to use them.

_You haven’t mastered a tool until you understand when it should not be used._ – [Kelsey Hightower](https://x.com/CodeWisdom/status/1354478025228873731)

I don’t think design or TDD is an either-or choice. I think it mostly depends upon the context and scope of the problem being addressed.

## It’s not Either-Or; it’s Yes-And
I think that both practices are effective. I think it depends upon the scope of the problem being addressed.

## Design
[My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html) first focuses upon domain elements and their relationships. This tends to map to a class design, but I’m not trying to implement a solution or even solve the problem. I’m trying to understand it by decomposing it into its domain elements. I’m trying to understand the different domain elements, their relationships and interactions.

## TDD
TDD works well when developers know enough to define behavior specifications that can be confirmed in the implementation. The list of behaviors doesn’t need to be complete. New behaviors may emerge while working through the TDD process as the domain is better understood. This is still a discovery process, and it may result in updates to the design as well.

Behavior specifying tests can be written directly prior to implementation as described in [Test-Driven Development]( https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development). Or they can be written in conjunction with the implementation, as described in [The Three Laws (Rules) of TDD](https://jhumelsine.github.io/2024/07/15/tdd.html#the-three-laws-rules-of-tdd).

The scope of the implementation being specified via tests tends to be a class with the API being its public methods.

# Advent of Code
Since retirement, I haven’t had many opportunities to compare design and TDD practices professionally. However, I try to stay somewhat active with coding challenges, such as [Advent of Code]( https://adventofcode.com/).

I’d like to describe two Advent of Code challenges where I used a TDD-First approach in one and a Design-First approach in the other.

## TDD-First
Day 12’s challenge from 2024 was [Garden Groups](https://adventofcode.com/2024/day/12). In this challenge, we are given a garden layout with different regions of plant types, identified only by capital letters. Here’s their example of five regions:
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

It has a perimeter of four fence segments and an area of one, as seen graphically:
```
+-+
|A|
+-+
```

I expanded the second test to:
```
AA
```

This is a garden with six fence segments and an area of two, as seen graphically:
```
+-+=+
|A A|
+-+-+
```

I converged to the correct answer fairly quickly, from what I recall, as the tests got more sophisticated.

However, I violated a major part of TDD. I didn’t refactor too much. I cleaned the code a bit, but for the most part I just worked toward getting the right answer. _Mea culpa. Mea culpa. Mea maxima culpa._

Here’s the code that calculates the perimeter and area. I should have known I was going to have issues when the parameter type is `Map<String, List<Set<Position>>> crops`:
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

One of my issues with the Advent of Code format is that there’s a new problem each day. It’s easy to fall behind. There’s not much incentive to keep code clean. Solve it and move on.

Each day’s challenge is in two parts. Solving the first part opens access to the follow-up part of the challenge. The second part is a continuation of the first problem, so there is some continuity, but for the most part continuity is only for that day. Sometimes the second part can be solved with a single line update to the first part’s solution. Sometimes it requires a whole new approach.

If we knew the second part while working on the first part, we might be able to design and implement the first part so that it can more easily accommodate the second part. But we neither see nor predict the future in the Advent of Code just like we cannot for most software projects.

The second part of the Garden Groups, __Part 2 Spoiler Alert__, updated the behavior such that a straight row of fencing would be one segment of fencing regardless of its length. The `A` garden would still have a perimeter of four segments, and the `AA` garden would have a perimeter of four segments as well. The fencing across the top and bottom would each be one segment that’s two units long rather than the two segments of one unit in length from Part 1.

The update for the `AA` garden can be seen graphically via:
```
+--=+
|A A|
+---+
```

I could easily add new tests for Part 2. However, I struggled quite a bit to get the Part 2 solution. I doubled down on the Part 1 solution and forced it to solve Part 2 via:
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

I am fully aware of how horrible this code is. I wrote this about two months ago, and I have no idea how it works anymore. There’s too much implementation detail and not enough design. The implementation is too tightly coupled with data structure concepts. It’s a micro version of a [Big Ball of Mud](https://en.wikipedia.org/wiki/Anti-pattern#Big_ball_of_mud). Even with coverage, I’d consider this poor legacy code.

If anyone were to try to refactor this code, at least they’d know immediately if they broke any behavior, but I doubt that the tests would assist them in understanding the implementation or what they had done wrong. Consider how challenging and frightening it would be to refactor this code without any test coverage?

Advent of Code’s daily challenge is an artificial deadline. Deadlines are self-imposed by developers they’re trying to keep up with others or just themselves.

Advent of Code is a microcosm of software development. There is always deadline pressure. Developers are often pressured to get the code working and deliver without refactoring.

## Design First
I decided to go with a Design-First approach with Day 15, [Warehouse Woes](https://adventofcode.com/2024/day/15). This challenge defines a rectangular warehouse space enclosed by walls with a few wall barriers inside the space. There are boxes distributed around the warehouse floor as well. A robot is given instructions to move up, down, left and right. If the robot encounters a box in front of it on its move, it also moves the box. If there are several boxes in a row in its path, then the robot will nudge all of them when it moves.

If a wall is directly in front of the robot, it will block the robot for that move and proceed to the next unblocked move. If the box in front of a robot’s path is also blocked by the wall or another blocked box, then the robot and boxes are blocked for that move as well.

I could envision the progression of tests starting with simple Warehouse layouts and moving toward more complex ones; however, I restrained myself. I didn’t start with TDD immediately.

I thought about a design, which was simple enough to keep it in my head. My design included several classes: __Warehouse__, __WarehouseElement__, __Movable__, __Wall__, __Box__, __Robot__, __Position__ and __MoveDirection:__
* __Warehouse__ contains __WarehouseElements__
* A __WarehouseElement__ maintain a __Position__
* __Movable__ and __Wall__ are __WarehouseElements__
* __Movable__ is a __WarehouseElement__ that can move in a __MoveDirection__ and thus change its __Position__
* __Box__ and __Robot__ are __Movable__ __WarehouseElements__

As domain behaviors emerged from the design, it was usually obvious within which class their implementations belonged.

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

The second part of the Warehouse Woes, __Part 2 Spoiler Alert__, added the stipulation that everything, except the robot, is twice as wide. That’s not too bad until you consider that two-width boxes could be arranged in a pyramid pattern where one box is atop a row of two, which are atop a row of three, etc. When the robot moves the top box down, it will move the entire pyramid box pattern down as well. If just one of those boxes is blocked by a wall, then the entire pyramid of boxes and the robot are blocked.

The `move` method for Part 1 only worked for single width WarehouseElements. It wasn’t obvious to me during the Part 1 design and implementation that the Part 1 solution had an implicit dependency upon single width WarehouseElements. This dependency became obvious once I learned that the domain model needed to support wider WarehouseElements.

I needed to think about my design a bit more. I added a width factor to WarehouseElement. The requirement was only for a width of two, but my design could support any width. The robot could support varying width, but its width was always set to one.

My first tests restricted width to one so that all my previous Part 1 tests would work for Part 2 as well. Once the width-one tests passed, I added width-two tests.

The refactored `move` method, which supported a set of movables, became:
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

It handles a more sophisticated domain model, and I think it’s a less data structure dependent implementation. It took time to get this code working, but I had a clear path in mind as I made every adjustment with TDD keeping my on track. I didn’t spend much time debugging or figuring out the next step.

# Bob Martin and John Osterhout Debate
I had still been procrastinating this blog entry, even if I felt I had resolved my contradictions. However, I wasn’t sure if I could justify my resolutions to an audience even if they felt right to me. Was I resolving the conflict or rationalizing it?

Recently I noticed a post on Twitter/X that featured a link to a [Debate](https://github.com/johnousterhout/aposd-vs-clean-code/blob/main/README.md) between [Robert “Uncle Bob” Martin](https://en.wikipedia.org/wiki/Robert_C._Martin) and [John Ousterhout](https://en.wikipedia.org/wiki/John_Ousterhout) regarding the differences in their software design philosophies. Their debate focused upon three topics:
* Method/function length
* Comments
* Test-Driven Development

I read their debate where I stumbled upon one of Bob’s comments in the TDD section:

>My own experience is that design comes from strategic thought, which is independent of the tactical behavior of either TDD or Bundling. Design is taking one step back from the code and envisioning structures that address a larger set of constraints and needs.
>
>Once you have that vision in your head it seems to me bundling and TDD will yield similar results.

Aha! That was it! Design is thinking strategically about the problem. TDD is thinking tactically about the solution. Strategy is __what__ needs to happen. TDD is about __how__ to accomplish that.

Bob’s comment gave me the final piece of the puzzle to tie it all together and finally complete this blog entry.

__Side Note:__ Following-up with the debate, where it was like John wasn’t listening to Bob, when John said:
>It's hard to design something well if you don't think about the whole design problem at once. TDD explicitly prohibits developers from writing more code than is needed to pass the current test; this discourages the kind of strategic thinking needed for good design.

But that’s not what Bob is saying about TDD. You can step back and think about the overall design.

John has a few more discouraging things to say about TDD, and then he drops this:

>You ask me to trust your extensive experience with TDD, and I admit that I have no personal experience with TDD. On the other hand, I have a lot of experience with tactical programming, and I know that it rarely ends well. TDD is one of the most extreme forms of tactical programming I've encountered.

In the same paragraph, he claims to have no personal experience with TDD, and yet it’s still one of the most extreme forms of tactical programming he’s encountered. I would respect his opinion more if he had said that he had tried TDD himself and found it lacking. His TDD opinion reeks of dogma.

# Summary

<img src="https://thumbs.dreamstime.com/b/arch-titus-rome-28381536.jpg" alt="A Roman Arch" title="Image Source: https://www.dreamstime.com/photos-images/arch-titus-menorah.html" width = "15%" align="right" style="padding-right: 20px;">

I was quite conflicted at the start of this blog. Jesus said in Matthew 12:25, “A house divided against itself will not stand.” I was concerned that my design and TDD approaches were like a divided house.

Writing this blog entry was a bit of therapy. I worked through the conflict. Design is strategic thinking, and TDD is tactical thinking. We need both.

These two approaches are not opposing forces that will collapse upon themselves. They are the two reinforcing like the sides of an arch pressing against and supporting one another making the whole stronger.

# References
