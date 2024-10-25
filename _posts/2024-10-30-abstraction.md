---
title: DRAFT – What Is Abstraction?
description: Abstraction is separating the WHAT from the HOW in the design and implementation.
unlisted: true
---

# Introduction
I’m a volunteer mentor for computer science students at the university where I acquired my undergraduate degree. I visited campus last month for homecoming weekend. While there, I had some time to meet face-to-face with Scott, one of my mentees. Since I live about 250 miles from the university, most interaction with my mentees is electronic, so this was a rare opportunity to meet with him in person.

Scott and I grabbed a quick bite at an off-campus pizza shop directly across from one of the engineering buildings. I asked him about his summer experiences and how his final year before graduation was shaping up.

Scott knows that I’m a fan of Design Patterns, but he wasn’t sure how the patterns fit into what he’s been studying. With only my words and waving hands for presentation I compared Data Structures to Design Patterns. Data Structures solve well defined fixed problems bounded by a limited scope, whereas Design Patterns define solution approaches to recurring flexible fuzzy problems not necessarily bounded by scope. I confessed that developers may need to experience real-world problems in industry before they appreciate Design Patterns. I used a few analogies such as:
* Solving a problem with Data Structures is like being a cook following a recipe to the letter, whereas approaching a problem with Design Patterns is like being a [chef](https://jhumelsine.github.io/2023/08/21/knock-knock-whos-there.html) creating a new dish based upon years of experience. __NOTE:__ I first typed _tears of experience_ by accident. Though I changed it, _tears_ may be more accurate than _years_.
* Learning programming language fundamentals is like learning the rules to a game, whereas learning implementation design is like learning the [strategy and tactics](https://jhumelsine.github.io/2023/08/24/its-your-move.html) of how to play the game well.
* The [wheel](https://jhumelsine.github.io/2023/08/28/wheels.html) is a mechanical engineering design pattern. All wheels have the same basic design as a disk-shaped object that rotates upon an axis at its center. Each wheel must be designed specifically in the context in which it’s being used.

Then Scott asked, “What is _Abstraction_?” What are they teaching kids in college these days?

I tried to give him a quick answer, but we were running out of time. I promised him I’d follow up with a more complete answer, and that’s the main purpose of this blog entry.

# What vs How
Though I chide academia for not explaining abstraction, the word ___abstract___ has appeared in 50% of my previous blog posts, and I haven’t defined it either.

I dedicated the [Getting the Right Abstraction is Hard]( https://jhumelsine.github.io/2023/09/22/right-abstraction-is-hard.html) blog entry to abstraction over a year ago, and I assumed my readers knew what I meant.

I suspect that definitions for _abstraction_ vary. I included a few [Abstraction](https://jhumelsine.github.io/2023/12/15/bumper-sticker-computer-science-software-engineering.html#abstraction) quotes in [Bumper Sticker Computer Science and Software Engineering]( https://jhumelsine.github.io/2023/12/15/bumper-sticker-computer-science-software-engineering.html).

Google’s AI generated this definition for me:
>Abstraction is a fundamental concept in software engineering and computer science that involves removing unnecessary details to focus on what's important. It's a way to make systems more generic and easier to understand.

Other online references also focus upon _removing details_. I don’t view ___abstraction___ as _removing details_ as much as I view it as ___moving details___. Those details still exist. Abstraction is about the management of those details.

An _Abstraction_, such as an `interface` in Java, declares __WHAT__ behaviors are provided. That is, an interface is a set of methods that declare __WHAT__ those methods are and do without defining __HOW__ those methods will do them. It’s a contract.

The details, which define __HOW__ the interfaces will be implemented, have been _moved_ to the concrete classes, which implement the interfaces.

This is evident in the [Strategy Design Pattern](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html), for which an interface may have multiple implementations:
* `ComputerAidedDesign.render()` can `draw()` any `Shape` without having to know specific `Shape`s. It doesn’t know that a `Shape` could be a `Circle`, `Triangle` or a `Rectangle`.
* `Shape` is the interface abstraction that declares __what__ it can do: `draw()`.
* `Circle`, `Triangle`, and `Rectangle` are each a concrete class defining __how__ to implement `draw()` for each specific `Shape`.

<img src="/assets/AbstractionStrategy1.png" alt="Basic Strategy"  width = "80%" align="center" style="padding-right: 35px;">

The Strategy Design Pattern generally does not indicate how references to `Shape` find their way into the `private List<Shape> shapes` attribute within `ComputerAidedDesign`. I introduced a way to address this in the [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) blog.

The following diagram enhances the previous diagram with the addition of Dependency Injection and a few more diagram elements:
* `ComputerAidedDesignConfigurer` is a Dependency Injector. It creates instances of `Circle`, `Triangle` and `Rectangle` and adds them to `ComputerAidedDesign` via an API not shown in the design.
* The red dashed lines are design boundaries. They do not represent implementation artifacts. I’ve added them to illustrate how the design space is organized in regions. I introduced this concept in [Hexagonal Architecture – Why it works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html). Though introduced in the context of Hexagonal Architecture, these concepts do not depend upon Hexagonal Architecture. These concepts are about dependency and knowledge management. Each pair of connected elements have an implicit knowledge and dependency relationship, which is represented via the arrowheads. When A points to B, then A has knowledge of and depends upon B. B has no knowledge of or dependency upon A.
* The red horizontal dashed line separates the abstract and the concrete elements within the scope of this design space. `ComputerAidedDesign` knows and depends upon `Shape`, but nothing else. The abstraction in this context is that `render()` knows how to draw a collection of `Shapes`.
* The red vertical dashed line separates the configuration from the concrete implementation. `Circle`, `Triangle` and `Rectangle` implement `Shape` and define how each specific `Shape` is drawn. They only know and depend upon `Shape`.
* `ComputerAidedDesignConfigurer` creates the instances and assembles them. It has knowledge of the rest of the design, but only to the extent that it knows the classes exist, how to instantiate them and add those instances to `ComputerAidedDesign`. It does not access their functional methods. All arrowheads point away from the `Configurer`. It is essentially invisible to the rest of the design.

<img src="/assets/AbstractionStrategy2.png" alt="Strategy with Configurer and design boundaries"  width = "80%" align="center" style="padding-right: 35px;">
 
Abstraction makes this design more maintainable. When new `Shape`s are desired, such as `Trapezoid` and `Rhombus`, no updates are required above the red horizontal dashed line.

New concrete `Trapezoid` and `Rhombus` classes implement `Shape`. `ComputerAidedDesignConfigurer` will be updated to know about the existence of the new concrete classes as well.

Ease of maintenance continues in the Concrete region as well. The existing concrete classes are not affected by the addition of the new concrete classes.

<img src="/assets/AbstractionStrategy3.png" alt="Strategy with more concrete classes added"  width = "80%" align="center" style="padding-right: 35px;">

One may notice that `Rectangle`, `Trapezoid` and `Rhombus` are four-sided `Shape`s. What if more four-sided concrete classes were added, such as `Parallelogram`, `Square` and `Quadrangle`? We might observe common code among the four sided `Shape`s and decide to refactor them.

If we choose to refactor or redesign the concrete classes below the red horizontal dashed line, we can do so without concern of breaking any behavior that resides in the abstraction above the red horizontal dashed line.
 
# Abstraction Is Not Obvious
Strategy is only one example of abstraction. Functions, procedures and methods with meaningful names that document __what__ the code does are probably the most common forms of abstraction as championed by the [Extract Method](https://refactoring.guru/extract-method) refactoring technique.

The right abstraction may not be obvious. See: [Getting the Right Abstraction is Hard]( https://jhumelsine.github.io/2023/09/22/right-abstraction-is-hard.html)

The ability to abstract wasn’t available in early programming languages. The first programming language I learned was [BASIC](https://en.wikipedia.org/wiki/BASIC#:~:text=BASIC%20(Beginners'%20All%2Dpurpose,at%20Dartmouth%20College%20in%201963.)) in the 1970s. Each statement had a line number, and that’s how program flow was directed via `GOTO` and `GOSUB` statements. There were no subroutine names. Subroutines didn’t have arguments or return types. All variables were global. And from what I recall, variables could be no more than two letters long.

As programming languages advanced, they supported abstraction. Abstraction is not for the benefit of the computer. Abstraction is for the benefit of the software developer to better convey and understand the intent of the implementation.

# Abstraction In the Real World
Real world abstractions abound around us. Once you see them, they’re easier to see and understand in code too.

## Cars
<img src="https://live.staticflickr.com/2145/1557975479_5c89f5bd58_c.jpg" alt="Driver's side car interior" title="Image Source: https://www.flickr.com/photos/philiphubs/1557975479" width = "35%" align="right" style="padding-right: 20px;">

Cars are complex machines, yet most adults can drive unfamiliar cars easily. Rental cars are often different from what we drive at home, but most people can hop in the car and drive off the rental lot.

Most driving controls and the dashboard are an abstraction of what’s going on under the hood. The driver uses the steering wheel, gas pedal and brake pedal to tell the car __what__ to do. The under-the-hood mechanics is __how__ the car manifests the driver’s desires.

The dashboard displays pertinent information to the driver, such as speed, miles, remaining fuel, warning lights, etc. This is mostly an abstraction as well.

<img src="https://images.pexels.com/photos/104836/speed-car-vehicle-drive-104836.jpeg" alt="Tachometer" title="Image Source: https://www.pexels.com/photo/speedometer-gauge-reading-at-zero-104836/" width = "35%" align="left" style="padding-right: 20px;">
 
There’s one dashboard instrument that I’ve never understood why it's presented to the driver – the tachometer. I’m not a car enthusiast, so maybe there’s a reason to display the RPMs of the engine that I don't know. My last three cars had tachometers, and they were automatic transmissions. My first car was standard transmission, and I rarely looked at the tachometer to decide when to shift gears.

The tachometer feels like a [Leaky Abstraction](https://en.wikipedia.org/wiki/Leaky_abstraction) to me. It’s telling me design details about the engine that I don’t need to know. We want to avoid leaky abstractions in our designs. We don’t want to expose implementation details to our users via the abstraction. In the case of the tachometer, its exposure is benign. Any concept that’s exposed to the user will be used by a user. If a project leaks an implementation detail via an abstraction, then it’s part of the interface.

Woe be the software project that leaks its database schema via an abstraction. Its database schema will be part of its API. 

## Wikipedia
Each Wikipedia page focuses upon one topic. The page’s title is its abstraction which briefly describes __what__ the page is about. The rest of the page contains topic details.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/83/Down_the_Rabbit_Hole_%28311526846%29.jpg/1200px-Down_the_Rabbit_Hole_%28311526846%29.jpg?20220726070017" alt="Alice in Wonderland with the Rabbit" title="Image Source: https://commons.wikimedia.org/wiki/File:Down_the_Rabbit_Hole_(311526846).jpg" width = "35%" align="right" style="padding-right: 20px;">
 
Most Wikipedia pages contain embedded hypertext links to other Wikipedia pages for additional information. Sometimes readers go so far down the [Wiki Rabbit Hole](https://en.wikipedia.org/wiki/Wiki_rabbit_hole) that they forget where they started.

While there’s basically one layer of abstraction for each Wikipedia page, there are many layers of abstraction within the Wikipedia environment. As readers follows the embedded links they dive deeper into details for additional context, but it’s the readers' choice to do so.

What if a Wikipedia editor didn’t want to create a new page and add an embedded link? What if they put the new content in the current page, even if that content strays from the page’s original topic? And then another editor adds more content to that content further straying from the page’s original topic?

This pattern continues. Soon, the page becomes bloated with off-topic details. The reader most read the entire page, but it may not be obvious which sections are part of the original topic and which sections contain supporting off-topic content.

Fortunately, I think that Wikipedia editors tend to keep most pages focused upon the topic and link to other pages as necessary. I can’t say that all software developers follow the same practice.

In the same way that each Wikipedia page is about a core topic, I feel that each method, function and procedure should have one core responsibility, which is known as the [Single Responsibility Principle](https://en.wikipedia.org/wiki/Single-responsibility_principle) (SRP).

_So much complexity in software comes from trying to make one thing do two things._ — [Ryan Singer](https://x.com/codacy/status/728285924191244289)

A method’s name is its abstraction declaring __what__ it does. The implementation contains the details of __how__ it does it. Rather than creating a new method or class and referencing it via the method name, all too often, developers will place the new code in the original method even if it strays from the method’s single responsibility. It’s a shortcut made for convenience.

I’ve encountered methods that are hundreds of lines long, which obviously violate SRP. Each of these methods grew slowly – one shortcut at a time.

Most visual editors can display at most about 50 lines of code on the screen. Any part of a method which I cannot see, I must retain in my head to understand the behavior implemented with a method. If a method is 500 lines, long, I can only see at most 10% of the method at any given time. I’m too old to hold the remaining 90% in my mind.

Methods tend to get long when an implementation jams multiple layers of abstraction into one method. Reading the method takes the reader on a nauseating roller coaster ride from high level business concepts to low level infrastructural details and back again. If segregated into separate methods or classes along abstraction boundaries, then readers would only need to examine those lower-level abstractions if necessary.

<img src="https://live.staticflickr.com/2152/2161518548_eb13cddfe5.jpg" alt="Decomposed Car" title="Image Source: https://www.flickr.com/photos/hahatango/2161518548" width = "35%" align="right" style="padding-right: 20px;">
 
Real world concrete abstractions, such as cars, have multiple layers of abstractions as well. A car is comprised of systems, such as the engine, transmission, drive train, breaking system, electrical system, etc. These systems have their own components. Eventually the entire car decomposes to a set of parts. Real world abstractions deconstruct to their most basic parts.

Wikipedia is different from cars. Follow the links of a Wikipedia page, and you end up at another Wikipedia page. No matter how many Wikipedia reference links you follow, you still end up at another Wikipedia page that mostly looks like the others. It’s a single topic page with words and links to other pages.

Software is the same. As the reader descends into method implementations, each method will tend to look like other methods. Code that implements high level and low level concepts will tend to look the same. It will consist of `if` statements, `for` loops, calls to other methods, etc. This similarity is what makes it difficult to separate business concepts from infrastructure details when they are intermixed in the same method.

Layers of abstractions in Wikipedia and software are fractal. Regardless of where you reside in the layers of abstraction, it all tends to look the same.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/97/Fractal_Julia.jpg/1199px-Fractal_Julia.jpg" alt="Fractal" title="Image Source: https://commons.wikimedia.org/wiki/File:Fractal_Julia.jpg" width = "35%" align="right" style="padding-right: 20px;">
 
The fractal nature of software is a double-edged sword. On the positive side, our abstractions are not constrained by the physical world. On the negative side, our abstractions are not constrained by the physical world. We can design our abstractions anyway we desire. We do not have any real world guardrails to keep our abstractions in check.

Our abstractions, right or wrong, are our responsibility. Get the abstraction right and code will practically write itself. Get the abstraction wrong and it will cause you many headaches.

# Cohesive Abstractions
Since this blog is getting long, I’ll postpone __Cohesive Abstractions__ to the next blog entry.

# Summary
Abstraction is an important concept in software engineering separating __what__ from __how__. Abstraction is one of the main concepts that allows us to create a modular design for easy of future maintenance. 

However, the right abstraction is not always easy or obvious. Don’t force abstractions too early. It may take a few examples before the abstractions emerge from the domain, design or implementation.

# References
* [Wikipedia Abstraction In Computer Science](https://en.wikipedia.org/wiki/Abstraction#In_computer_science)
* [Wikipedia Abstraction (Computer Science)](https://en.wikipedia.org/wiki/Abstraction_(computer_science))
* [Wikipedia Abstraction Principle (Computer Programming)](https://en.wikipedia.org/wiki/Abstraction_principle_(computer_programming))
* [What Are Abstractions in Software Engineering with Examples](https://thevaluable.dev/abstraction-type-software-example/) - The Valuable Dev
* [What is Abstraction in Programming – And Why is it Useful?](https://www.freecodecamp.org/news/what-is-abstraction-in-programming/) - Tiago Capelo Monteiro
* [abstraction definition](https://www.techtarget.com/whatis/definition/abstraction) - Ivy Wigmore
* [Abstraction: Not What You Think It Is](https://www.pathsensitive.com/2022/03/abstraction-not-what-you-think-it-is.html) - Jimmy Koppel
* [Software Engineering: It’s All About Abstraction](https://source.coveo.com/2022/03/30/software-engineering-it-s-all-about-abstraction/) - Kevin Lalumiere
* [Abstraction](https://www.cs.cornell.edu/courses/cs211/2006sp/Lectures/L08-Abstraction/08_abstraction.html) - Cornell University: CS211
* and for more, Google: [Abstraction in software](https://www.google.com/search?q=Abstraction+in+software)
