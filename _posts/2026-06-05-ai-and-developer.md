---
title: DRAFT OPEN FOR REVIEW AND COMMENTS – The End of Coding as Toil
description: Thoughts on AI, software engineering, and the changing role of developers
unlisted: true
---

# Introduction
The greatest disruptive technology these days is Generative AI.
Those who learn how to leverage it will have an advantage over those who do not.

The software industry is changing rapidly, but I do not believe software engineering is disappearing.
I think the nature of software engineering is changing.

For decades, software developers manually translated ideas into code.
Generative AI increasingly performs much of that translation for us.

That does not eliminate the need for software engineers.
If anything, it increases the importance of understanding systems, constraints, architecture, maintainability, and customer problems.

Programming was never the end goal.
[Solving problems](https://jhumelsine.github.io/2023/08/26/problem-solvers.html) for our customers has always been the goal.

ChatGPT arrived only a few months before I retired after a 38-year software engineering career.
I have not built commercial AI systems professionally, but I have spent decades watching abstraction layers reshape software development.
I think AI is another one of those moments.

# Technology Eliminates Toil

Humans have been using technology for millennia to eliminate toil.

**Writing code by hand is becoming toil.**

I spent my entire 38-year career writing code by hand.
I was skilled.
Nineteenth century blacksmiths were also skilled, but the Industrial Revolution rendered village blacksmiths obsolete.

There are still blacksmiths.
Most now work as artisans, hobbyists, or craftspeople.
But the village blacksmith as a common profession has disappeared.

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

Like others before them, software developers now find themselves standing in the crosshairs.

AI can generate code faster than people.

But software engineering was never fundamentally about typing quickly.

Rumors of the software engineer's death are greatly exaggerated.

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

Humans have always introduced ambiguity, interpretation, tradeoffs, assumptions, inconsistency, and bugs into software development.

**We tolerate non-deterministic humans; however, we become uncomfortable with non-deterministic AI.**

When autonomous systems fail, we often view it as a technological failure, such as with self-driving vehicles.
When humans fail, we often call it an unfortunate or tragic accident.

We demand perfection from technology in ways we rarely demand from ourselves.

AI did not introduce non-determinism into software development.
It was always there.

# Coding Was Never the Hard Part

For a significant portion of my career, I used the [Waterfall Model](https://en.wikipedia.org/wiki/Waterfall_model) because it was simply the way software was developed at the time.

We spent enormous amounts of time typing.
But we were not necessarily typing code.
We typed requirements documents, design documents, specifications, and plans.
Much of that documentation became obsolete once development began.

The process gave us time to think, which was valuable.
But we often spent more time thinking about our interpretation of the problem rather than the customer's actual problem.

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

**AI accelerates typing.
It does not automatically accelerate understanding.**

# Wisdom vs Knowledge

It took me years to realize that knowing how to program and knowing how to design software were not the same thing.

For much of my early career, I knew the rules of programming languages, but I did not yet understand the wisdom of how to organize software.

It is [similar to chess](https://jhumelsine.github.io/2023/08/24/its-your-move.html).
The rules of chess fit on a single sheet of paper yet its strategies and tactics fill shelf upon shelf of books.

Eventually I learned [Design Patterns and software architecture principles](https://jhumelsine.github.io/table-of-contents#design-patterns-and-architecture).
Once those concepts clicked in my head, I realized I had been approaching software incorrectly for years.

The difference became obvious during a feature I designed shortly after learning Design Patterns.

The product architect completely respecified the feature between releases.
It was almost a 180-degree shift in direction.

I reviewed my UML class design and realized something important.
Some parts of the design could simply be discarded.
Other parts represented reusable infrastructure and remained completely intact.

I designed new classes implementing the revised behavior and plugged them into the existing design without difficulty.

If I had not invested effort into a modular design with separation of concerns, the feature likely would have required a complete redesign and reimplementation from scratch.

That was the moment I truly understood the value of architecture.
Software engineering is not just about building systems that work.
It is about building systems that can survive change.

A good design makes code trivial.
I first mentioned this in an early [The Big Wheel Keeps on Turning](https://jhumelsine.github.io/2023/08/28/wheels.html), an early [Design Patterns](https://jhumelsine.github.io/table-of-contents#design-patterns-and-architecture) blog entry with:
>Though design patterns are very flexible, they are also based upon well-defined structures that encourage a modular design and separation of concerns. I know this sounds cliché, but once I’m satisfied with a design, my code often writes itself and it tends to just work. Difficult or stubborn bugs decreased significantly for me once I started to incorporate design patterns.

Though it sounds a bit pretentious but writing code from a good design felt a bit like the [Scribbling and Bibbling](https://www.youtube.com/watch?v=qzY2IeoveHw&t=100s) scene from _Amadeus_.
Now we can let the AI do the _scribbling and bibbling_.

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

AI already knows about software engineering practices and principles:
- [Design Patterns](https://jhumelsine.github.io/table-of-contents#design-patterns-and-architecture),
- [Automated Testing](https://jhumelsine.github.io/table-of-contents#automated-testing),
- [TDD](https://jhumelsine.github.io/2024/07/15/tdd.html),
- [Mutation Testing](https://jhumelsine.github.io/2025/03/28/mutation-testing.html),
- CI/CD,
- Domain-Driven Design,
- scalability,
- modularity,
- and security

because those concepts exist within its training data, but it does not know when to apply them.

AI does not automatically know which constraints matter for a particular system unless humans specify them.
The problem may not be that AI lacks software engineering knowledge.
The problem may be that engineers need enough experience and judgment to guide AI effectively to use them.

AI can implement, but senior engineers provide prioritization, judgment, tradeoffs, and organizational memory.
Junior developers may not initially have that judgment to guide the AI.

AI may allow smaller teams to build systems that previously required much larger organizations.

But leveraging AI does not eliminate the need for engineering judgment.
If anything, it amplifies the consequences of poor decisions.

# Architecture Is About Change

One of my concerns with AI-generated software is not whether it can solve the immediate problem.
It usually can.

My concern is whether the design remains flexible as we learn more about the real problem through user feedback.

We do not want the user to say:
> "Can you move this button slightly to the right?"

only to have the AI regenerate the entire codebase from scratch.

Architecture is fundamentally about controlling change.
Good modular design allows systems to evolve safely and incrementally.
A well-designed system allows behavior to change while stable infrastructure remains intact, much like my previous experience mentioned above.

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

Software engineers solve customer problems with software.
Whether humans write the code directly or AI generates it matters less than whether the system actually solves the problem well.

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

One concern I have is what happens to junior developers, which I briefly mentioned above.

Computer Science graduates are not experts in every topic they studied in academia.
Most software developers will never personally design CPUs, compilers, operating systems, or networking protocols.
But understanding those concepts still matters.
The computer is not a mysterious black box even if we do not understand every implementation detail.

I suspect future software engineers may need fewer implementation skills than previous generations.
But they will still need enough understanding of good software engineering principles and practices to guide AI systems intelligently.

Good software engineering principles and practices are not generally taught in academia, which has been one of my driving forces in creating these blog posts.
Historically, many developers learned software engineering through:
- boilerplate work,
- bug fixing,
- maintenance,
- repetitive implementation,
- and gradual exposure to larger systems.

AI increasingly automates many of those activities.

That creates an uncomfortable question:
_If junior developers are not given the opportunity to learn software engineering through experience or mentorship early in their careers, how do future senior engineers emerge?_

Current senior engineers eventually retire, move into management, or leave the industry.
If younger developers never gain enough experience to develop architectural judgment and engineering wisdom, we may eventually create a deficit of human knowledge within the industry.

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

**AI can generate code faster than humans can realistically review line by line.**

That changes the nature of validation.

Future engineering processes may rely more heavily upon:
- automated testing,
- specification validation,
- architectural constraints,
- observability,
- static analysis,
- and behavioral verification.

Humans and AI need to work together.

Some companies are already making aggressive staffing decisions based on assumptions about AI-driven productivity, as can be seen in headlines like these: [**Block lays off nearly half its staff because of AI. Its CEO said most companies will do the same**](https://www.cnn.com/2026/02/26/business/block-layoffs-ai-jack-dorsey).

The companies that are making drastic layoffs under the assumption that AI alone can replace software engineers are making a mistake.

AI can generate software, but humans still define what "good" means.

# We're Not Dead Yet
<img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExazNmaGpiZDB2OWZwMngwcXRwZjQxOTBvYTVnYXRtaTBqcW9mcWx4YSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/ZErfXKEMifIifbFM7c/giphy.gif" alt="We're not dead yet" title="Image Source: https://giphy.com/gifs/goodnightsunrise-not-dead-yet-werenotdeadyet-were-ZErfXKEMifIifbFM7c" width = "30%" align="right" style="padding: 20px;">

Sometimes I wonder whether the software engineering principles I spent years learning and documenting are becoming obsolete.

Ironically, I suspect they may become even more important.

AI can generate implementations rapidly, but architecture, modularity, constraints, and engineering judgment still need to come from somewhere.

The software industry is changing, but software engineering is not disappearing.

Generative AI increasingly translates ideas into code for us.

That does not eliminate the need for software engineers.
If anything, it increases the importance of:
- understanding systems,
- managing complexity,
- organizing change,
- defining constraints,
- and solving customer problems.

The tools are changing.

The abstractions are rising.

But software engineering was never fundamentally about typing code.
It was always about understanding problems and building systems that continue serving people as the world evolves around them.

Much is changing, but the mission is not.

# References
* [Coding Is Dead, Long Live Programming](https://ian-cooper.writeas.com/coding-is-dead-long-live-programming) - Ian Cooper argues that while the act of writing the code will no longer be done by humans, the act of directing the AI to write the code that's needed will still be the responsibility of humans.
* [Patterns for Reducing Friction in AI-Assisted Development](https://martinfowler.com/articles/reduce-friction-ai/) - Rahul Garg proposes five patterns that bring this collaborative scaffolding to AI-assisted development, shifting the experience from correcting a tool to collaborating with a capable teammate.
* [Modern Software Engineering AI Video Playlist](https://www.youtube.com/playlist?list=PLwLLcwQlnXBwUTz7GM9_a5ebyMA4PaP5f) - A playlist of AI related videos from the [Modern Software Engineering Video Channel](https://www.youtube.com/@ModernSoftwareEngineeringYT).
* [Redefining the Software Engineering Profession for AI](https://cacm.acm.org/opinion/redefining-the-software-engineering-profession-for-ai/) - Without the hiring of early-in-career developers, the profession’s talent pipeline will collapse, and organizations will face a future without the next generation of experienced engineers - _Communications of the ACM_
* [The velocity trap: Why genAI is exposing broken foundations](https://www.thoughtworks.com/en-us/insights/podcasts/pragmatism-in-practice/the-velocity-trap--why-genai-is-exposing-broken-foundations) - Thoughtworks podcast. The core thesis of this discussion is that AI is not automatically transforming software delivery. Instead, it acts as an amplifier of an organization’s existing strengths and weaknesses. Teams with strong engineering foundations, streamlined workflows, good testing practices, and adaptable cultures are accelerating dramatically with AI, while organizations with fragile pipelines, technical debt, siloed data, and outdated processes are often slowing down despite increased code generation.
* [AI and the workforce: Beyond efficiency to real value creation](https://www.thoughtworks.com/en-us/insights/podcasts/pragmatism-in-practice/ai-and-the-workforce--beyond-efficiency-to-real-value-creation) - Thoughtworks podcast. The core thesis of this discussion is that AI transformation is not primarily about replacing workers or achieving dramatic short-term productivity gains, but about reshaping how organizations learn, collaborate, and create value. AI acts as an amplifier of existing organizational strengths and weaknesses, making strong engineering practices, mentorship, continuous learning, and cross-functional collaboration more important rather than less. The speakers argue that success will come from combining bottom-up experimentation with top-down organizational strategy, redesigning workflows and team structures around AI-assisted work, and focusing on long-term value creation instead of narrow efficiency metrics or cost cutting. Rather than eliminating the need for human expertise, AI increases the importance of critical thinking, architectural judgment, and cultivating junior talent so organizations can sustainably evolve in an uncertain and rapidly changing technological landscape.
* [Software Fundamentals Matter More Than Ever](https://www.youtube.com/watch?v=v4F1gFy-hqg) - Video presentation by [Matt Pocock](https://www.aihero.dev/) ([GitHub](https://github.com/mattpocock)) - "AI coding tools are overhyped and powerful at the same time. Used well, they're extraordinary. Used badly, they'll bury you in spaghetti code faster than any human team could. The difference isn't the tool. It's the process. After 18 months of teaching developers to build with AI agents, Matt Pocock has watched the same patterns emerge: the devs who succeed aren't the ones who delegate everything or nothing. They're the ones who fall back on engineering fundamentals. In this talk, he shares the iterative process his students use to ship high-quality applications with AI agent swarms, and why the principles that make it work (ubiquitous language, vertical slices, TDD, deep modules) are decades-old ideas that didn't break. They got more important."
