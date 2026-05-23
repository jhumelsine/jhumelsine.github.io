---
title: WORK IN PROGRESS – The End of Coding as Toil
description: Thoughts on AI, software engineering, and the changing role of developers
unlisted: true
---

# Introduction

The greatest disruptive technology these days is Generative AI.
Those who learn how to leverage it will have an advantage over those who do not.

I have no business writing this blog.
I only have limited experience with Generative AI.
ChatGPT was released upon the world only a few months before I retired.
In that short period of time, it has flooded the Software Engineering community.

Many companies have made drastic cuts across the board due to AI, as can be seen in headlines like these: [**Block lays off nearly half its staff because of AI. Its CEO said most companies will do the same**](https://www.cnn.com/2026/02/26/business/block-layoffs-ai-jack-dorsey).

I think this is shortsighted.

The software industry is changing rapidly, but I do not believe Software Engineering is disappearing.
I think the nature of Software Engineering is changing.

For decades, software developers manually translated ideas into code.
Generative AI increasingly performs much of that translation for us.

That does not eliminate the need for software engineers.
If anything, it increases the importance of understanding systems, constraints, architecture, maintainability, and customer problems.

Programming was never the end goal.
The goal has always been [solving problems](https://jhumelsine.github.io/2023/08/26/problem-solvers.html) for people.

# Technology Eliminates Toil

Humans have been using technology for millennia to eliminate toil.

**Writing code by hand is becoming toil.**

I spent my entire 38-year career writing code by hand.
I was skilled.
Nineteenth century blacksmiths were also skilled, but the Industrial Revolution rendered village blacksmiths obsolete.

There are still blacksmiths.
Most now work as artisans, hobbyists, or craftspeople.
But the village blacksmith as a common profession disappeared.

Many other professions were transformed or eliminated during the Industrial Revolution.
Textile workers were among the first, leading to the [Luddite](https://en.wikipedia.org/wiki/Luddite) revolt.

<img src="https://upload.wikimedia.org/wikipedia/commons/0/09/Jacquard.loom.cards.jpg" title="Image Source: https://commons.wikimedia.org/wiki/File:Jacquard.loom.cards.jpg" alt="Jacquard loom" width = "30%" align="left" style="padding-right: 20px;">

One of the earliest programmable machines appeared in the textile industry as well.
The [Jacquard machine](https://en.wikipedia.org/wiki/Jacquard_machine) used punch cards to control woven patterns in cloth.

Computers have eliminated many forms of labor.
Switchboard operators disappeared.
Bank tellers diminished.
Movie ticket takers faded away.

<img src="https://live.staticflickr.com/2758/4154904299_c33828173d_h.jpg" title="Image Source: https://www.flickr.com/photos/ironrodart/4154904299" alt="Switch Board Operators" width = "35%" align="right" style="padding-left: 20px;">

Like others before them software developers themselves are standing in the crosshairs.

AI can generate code faster than people.
Sometimes much faster.

But software engineering was never fundamentally about typing quickly.

# AI Is Another Layer of Abstraction

In [Computer Science Problems](https://jhumelsine.github.io/2026/02/11/computer-science-software-engineering.html#computer-science-problems), I wrote:

> Each additional layer on the tech stack is for the benefit of humans, not the computer. The computer is just as content to execute machine code written by a human as it is to execute machine code generated from a compiler from source code written by a human.

Generative AI is adding yet another abstraction layer to the technology stack.

Some argue that this time is different because previous abstractions were deterministic while AI is not deterministic.

I used to agree with that argument, then I encountered a comment online that changed my thinking:

> We have always had non-determinism. Human developers have always been non-deterministic.

That took a while to sink in.

Once implemented, software behaves deterministically.
But the process of designing and building software has never been deterministic.

Give the same requirements to five different teams and you will likely receive five completely different architectures and implementations.

Humans have always introduced ambiguity, interpretation, tradeoffs, assumptions, and inconsistency into software development.

We tolerate non-deterministic humans; however, we become uncomfortable with non-deterministic AI.

When autonomous systems fail, we often view it as a technological failure, such as with self-driving vehicles.
When humans fail, we often call it an unfortunate or tragic accident.

We demand perfection from technology in ways we rarely demand from ourselves.

AI did not introduce non-determinism into software development.
It was always there.

# Coding Was Never the Hard Part

For a significant portion of my career, I used the [Waterfall Model](https://en.wikipedia.org/wiki/Waterfall_model) because it was simply the way software was developed at the time.

We spent enormous amounts of time typing.
But we were not necessarily typing code.
We typed requirements documents.
Design documents.
Specifications.
Plans.
And much of that documentation became obsolete once implementation actually began.

The process gave us time to think, which was valuable.
But we often spent more time thinking about our understanding of the problem rather than the customer's actual problem.

We lacked tight feedback loops.

Sometimes we built robust systems that solved the wrong problem.

Eventually I started learning more about software design, especially after discovering [Design Patterns and architecture principles](https://jhumelsine.github.io/table-of-contents#design-patterns-and-architecture).
That changed how I viewed software development entirely.

By the end of my career, I probably spent 75% of my time thinking and only 25% typing.

The real work was never typing code. The real work was:
- understanding domains,
- understanding constraints,
- organizing systems,
- managing change,
- and solving customer problems.

AI accelerates typing.
It does not automatically accelerate understanding.

# Wisdom vs Knowledge

It took me years to realize that knowing how to program and knowing how to design software were not the same thing.

For much of my early career, I knew the rules of programming languages, but I did not yet understand the wisdom of how to organize software.

It is [similar to chess](https://jhumelsine.github.io/2023/08/24/its-your-move.html).
The rules of chess fit on a single sheet of paper yet its strategies and tactics fill shelf upon shelf of books.

Eventually I learned [Design Patterns and software architecture principles](https://jhumelsine.github.io/table-of-contents#design-patterns-and-architecture).
Once those concepts clicked in my head, I realized I had been approaching software incorrectly for years.

The difference became obvious during one particular feature I designed shortly afterward.

The product architect completely respecified the feature between releases.
It was almost a 180-degree shift in direction.

I reviewed my UML design and realized something important.
Some parts of the design could simply be discarded.
Other parts represented reusable infrastructure and remained completely intact.

I designed new classes implementing the revised behavior and plugged them into the existing structure without difficulty.

If I had not invested effort into modularity and separation of concerns, the feature likely would have required a complete redesign and reimplementation from scratch.

That was the moment I truly understood the value of architecture.
Software engineering is not just about building systems that work.
It is about building systems that can survive change.

A good design makes code trivial.
I first mentioned this in an early [The Big Wheel Keeps on Turning](https://jhumelsine.github.io/2023/08/28/wheels.html), an early [Design Patterns](https://jhumelsine.github.io/table-of-contents#design-patterns-and-architecture) blog entry with:
>Though design patterns are very flexible, they are also based upon well-defined structures that encourage a modular design and separation of concerns. I know this sounds cliché, but once I’m satisfied with a design, my code often writes itself and it tends to just work. Difficult or stubborn bugs decreased significantly for me once I started to incorporate design patterns.

Though it sounds a bit pretentious, writing code from a good design felt a bit like the [Scribbling and Bibbling](https://www.youtube.com/watch?v=qzY2IeoveHw&t=100s) scene from _Amadeus_.
Let the AI do the _scribbling and bibbling_.

AI already knows most programming languages better than I do.
It remembers syntax perfectly.
It can generate boilerplate in seconds.
It rarely forgets API details.
It can refactor tirelessly.

AI has knowledge, but knowledge is not the same thing as wisdom.

AI knows the rules of programming.
It does not necessarily understand the strategies and tactics of good software design.

# AI as an Infinite Junior Developer
This distinction between knowledge and wisdom becomes especially important when working with AI systems directly.
I suspect that software engineers will increasingly resemble team leads managing systems of AI agents.

The software engineer will not simply hand work to AI and walk away.

The engineer will:
- guide the agents,
- redirect them,
- answer questions,
- define constraints,
- review outputs,
- and ensure that generated systems remain aligned with the actual customer problem.

In many ways, AI resembles an infinitely energetic junior developer.
It is fast.
It is knowledgeable.
It is tireless.
It can produce enormous amounts of code.
But it does not necessarily possess judgment, experience, or practical wisdom.

AI already knows about:
- Design Patterns,
- Automated Testing,
- TDD,
- CI/CD,
- Domain-Driven Design,
- scalability,
- modularity,
- and security

because those concepts exist within its training data.

But AI does not automatically know which of those constraints matter for a particular system unless humans specify them.
The problem may not be that AI lacks software engineering knowledge.
The problem may be that junior developers do not yet know enough to guide AI effectively.

# Architecture Is About Change

One of my concerns with AI-generated software is not whether it can solve the immediate problem.
It usually can.

My concern is whether the design remains flexible as we learn more about the real problem through user feedback.

We do not want the user to say:
> "Can you move this button slightly to the right?"

only to have the AI regenerate the entire codebase from scratch.

Architecture is fundamentally about controlling change.
Good modular design allows systems to evolve safely and incrementally.
A well-designed system allows behavior to change while stable infrastructure remains intact.

That matters even more in an AI-driven world.

Regenerating entire systems introduces:
- risk,
- instability,
- architectural drift,
- context loss,
- and unnecessary token usage.

Good architecture reduces regeneration.

That is one reason I believe architecture and design patterns may become more important rather than less important in the age of AI.

# Vibe Coding and the Monkey's Paw

The term [Vibe Coding](https://en.wikipedia.org/wiki/Vibe_coding) entered the lexicon recently and spread rapidly through the industry.

Describe what you want.
Let the AI generate the code.

There is certainly value in this approach, especially for experimentation and rapid prototyping.
I actually think AI may dramatically improve feedback loops.

During the Waterfall era, we often spent months attempting to fully understand requirements before users ever saw working software.

AI changes that dynamic.
We can now rapidly generate prototypes and place them directly in front of users for feedback.
That feedback is incredibly valuable.

However, I do not think that vibe coded prototypes should automatically become production systems.
The prototype validates ideas.
The production system must survive change.

I am also not convinced that "Vibe Coding" was the best term.

<img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExcWhxZjlldzdlZXpiNDQ4Z25pdmlueWt3Nm9wMW9ydmNjZWt4MHljOSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/fSGqUm3IcVBESFM0hK/giphy.gif" alt="Monkey Paw" title="Image Source: https://giphy.com/gifs/leroypatterson-wish-monkey-paw-fSGqUm3IcVBESFM0hK" width = "30%" align="right" style="padding: 20px;">

Perhaps "Monkey Paw Coding" would have been more accurate.
The AI often gives you exactly what you asked for, but not necessarily what you intended.
Natural language is full of ambiguity.

Human requirements have always been ambiguous.
Ambiguous requirements have always been dangerous in software development.
AI simply allows us to reach those misunderstandings much faster.

# Software Engineers as Orchestrators

I believe future software engineers will spend less time programming and more time specifying intent.

Programming itself was never the point.
[Solving customer problems](https://jhumelsine.github.io/2023/08/26/problem-solvers.html) was always the point.

Software engineers solve customer problems with software.
It does not fundamentally matter whether humans write the code by hand or AI generates it.

What matters is whether the system:
- solves the customer's problem,
- behaves correctly,
- remains maintainable,
- and evolves safely over time.

As confidence in AI systems improves, software engineers may spend less time reading individual lines of code and more time validating behavior, constraints, architecture, and outcomes.

The role of the engineer shifts from manual implementation toward orchestration.

Future software engineers may increasingly define:
- intent,
- architecture,
- constraints,
- quality expectations,
- and evaluation criteria

while AI systems perform much of the implementation work.

# The Apprenticeship Crisis

One concern I have is what happens to junior developers.

Historically, many developers learned through:
- boilerplate work,
- bug fixing,
- maintenance,
- repetitive implementation,
- and gradual exposure to larger systems.

AI increasingly automates many of those activities.

That creates an uncomfortable question:
_How do future senior engineers emerge?_

Senior engineers eventually retire, move into management, or leave the industry.
If younger developers never gain enough experience to develop architectural judgment and engineering wisdom, we may eventually create a deficit of human knowledge within the industry.

Computer Science graduates do not need to become experts in every topic they study.
Most software developers will never personally design CPUs, compilers, operating systems, or networking protocols.
But understanding those concepts still matters.
The computer is not a mysterious black box even if we do not understand every implementation detail.

I suspect future software engineers may need fewer low-level implementation skills than previous generations.
But they will still need enough understanding to guide AI systems intelligently.

# We Need an AI-Native Engineering Process

I do not think AI replaces Software Engineering processes.
If anything, I think it increases the need for them.
We likely need to evolve software development processes specifically for AI-assisted development.

Many of those ideas may resemble practices we already use:
- TDD,
- CI/CD,
- modularity,
- architecture,
- automated testing,
- code review,
- and separation of concerns.

But AI introduces new challenges.
AI can generate code faster than humans can realistically review line by line.
That changes the nature of validation.

Future engineering processes may rely more heavily upon:
- automated testing,
- specification validation,
- architectural constraints,
- observability,
- static analysis,
- and behavioral verification.

Humans and AI need to work together.

I think companies making drastic layoffs under the assumption that AI alone can replace software engineers are making a mistake.

AI can generate software, but humans still define what "good" means.

# We're Not Dead Yet
<img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExazNmaGpiZDB2OWZwMngwcXRwZjQxOTBvYTVnYXRtaTBqcW9mcWx4YSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/ZErfXKEMifIifbFM7c/giphy.gif" alt="We're not dead yet" title="Image Source: https://giphy.com/gifs/goodnightsunrise-not-dead-yet-werenotdeadyet-were-ZErfXKEMifIifbFM7c" width = "30%" align="right" style="padding: 20px;">

Sometimes I wonder whether the software engineering principles I spent years learning and documenting are becoming obsolete.
Ironically, I suspect they may become even more important.
AI can generate implementations rapidly, but good architecture, modularity, constraints, and engineering judgment still need to come from somewhere.

The software industry is not ending, but the nature of software engineering is changing.

For decades we manually translated ideas into code.
Generative AI increasingly performs much of that translation for us.

That does not eliminate the need for software engineers.
If anything, it increases the importance of:
- understanding systems,
- managing complexity,
- organizing change,
- defining constraints,
- and solving customer problems.

Programming was never fundamentally about typing code.

It was always about understanding problems and building systems that continue serving people as the world changes around them.

The tools are changing.

The abstractions are rising.

But software engineering was never fundamentally about typing code.
It was always about understanding problems, organizing change, and building systems that continue serving people as the world evolves around them.

Much is changing, but the mission is not.
