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

Writing code by hand is becoming toil.

I spent my entire 38-year career writing code by hand.
I was skilled.
Nineteenth century blacksmiths were also skilled, but the Industrial Revolution rendered village blacksmiths obsolete.

There are still blacksmiths.
Most now work as artisans, hobbyists, or craftspeople.
But the village blacksmith as a common profession disappeared.

Many other professions were transformed or eliminated during the Industrial Revolution.
Textile workers were among the first, leading to the [Luddite](https://en.wikipedia.org/wiki/Luddite) movement.

<img src="https://upload.wikimedia.org/wikipedia/commons/0/09/Jacquard.loom.cards.jpg" title="Image Source: https://commons.wikimedia.org/wiki/File:Jacquard.loom.cards.jpg" alt="Jacquard loom" width = "30%" align="left" style="padding-right: 20px;">

One of the earliest programmable machines appeared in the textile industry.
The [Jacquard machine](https://en.wikipedia.org/wiki/Jacquard_machine) used punch cards to control woven patterns in cloth.

That technology eventually evolved into the punch cards I used during my first two years in college.

Computers eliminated many forms of labor.
Switchboard operators disappeared.
Bank tellers diminished.
Movie ticket takers faded away.

<img src="https://live.staticflickr.com/2758/4154904299_c33828173d_h.jpg" title="Image Source: https://www.flickr.com/photos/ironrodart/4154904299" alt="Switch Board Operators" width = "35%" align="right" style="padding-left: 20px;">

The software industry has contributed heavily in automating many other professions.

Now software developers themselves are standing in the crosshairs.

AI can generate code faster than people.
Sometimes much faster.

But software engineering was never fundamentally about typing quickly.

# AI Is Another Layer of Abstraction

In [Computer Science Problems](https://jhumelsine.github.io/2026/02/11/computer-science-software-engineering.html#computer-science-problems), I wrote:

> Each additional layer on the tech stack is for the benefit of humans, not the computer. The computer is just as content to execute machine code written by a human as it is to execute machine code generated from a compiler from source code written by a human.

Generative AI is adding yet another abstraction layer to the technology stack.

Some people argue that this time is different because previous abstractions were deterministic while AI is not deterministic.

I used to agree with that argument.

Then I encountered a comment online that changed my thinking:

> We have always had non-determinism. Human developers have always been non-deterministic.

That took a while to sink in.

Once implemented, software behaves deterministically.
But the process of designing and building software has never been deterministic.

Give the same requirements to five different teams and you will likely receive five completely different architectures and implementations.

Humans have always introduced ambiguity, interpretation, tradeoffs, assumptions, and inconsistency into software development.

We are comfortable when humans are non-deterministic.
We become uncomfortable when AI is non-deterministic.

It reminds me somewhat of self-driving cars.

When autonomous systems fail, we often view it as a technological failure.
When humans fail, we often call it an accident.

We demand perfection from technology in ways we rarely demand from ourselves.

AI did not introduce non-determinism into software development.
It was always there.

# Coding Was Never the Hard Part

For a significant portion of my career, we used the Waterfall Model because it was simply the way software was developed at the time.

We spent enormous amounts of time typing.
But we were not necessarily typing code.

We typed requirements documents.
Design documents.
Specifications.
Plans.

Much of that documentation became obsolete once implementation actually began.

The process gave us time to think, which was valuable.
But we often spent more time thinking about our understanding of the problem than the customer's actual problem.

We lacked tight feedback loops.

Sometimes we built robust systems that solved the wrong problem.

Eventually I started learning more about software design, especially after discovering Design Patterns and architecture principles.

That changed how I viewed software development entirely.

By the end of my career, I probably spent 75% of my time thinking and only 25% typing.

The real work was never typing syntax.

The real work was:
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

It is similar to chess.

The rules of chess fit on a single sheet of paper.
The strategy and tactics fill shelf upon shelf of books.

Eventually I learned Design Patterns and software architecture principles.
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

AI already knows most programming languages better than I do.
It remembers syntax perfectly.
It can generate boilerplate in seconds.
It rarely forgets API details.
It can refactor tirelessly.

AI has knowledge.

But knowledge is not the same thing as wisdom.

AI knows the rules of programming.
It does not necessarily understand the strategy and tactics of good software design.

# AI as an Infinite Junior Developer

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

However, I do not think prototypes should automatically become production systems.

The prototype validates ideas.
The production system must survive change.

I am also not convinced that "Vibe Coding" was the best term.

Perhaps "Monkey Paw Coding" would have been more accurate.

The AI often gives you exactly what you asked for, but not necessarily what you intended.

Natural language is full of ambiguity.

Human requirements have always been ambiguous.
AI simply amplifies that ambiguity at machine speed.

# Software Engineers as Orchestrators

I absolutely believe future software engineers will spend less time programming and more time specifying intent.

Programming itself was never the point.

Solving customer problems was always the point.

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

How do future senior engineers emerge?

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

AI can generate software.

But humans still define what "good" means.

# We're Not Dead Yet

The software industry is not ending.

But the nature of software engineering is changing.

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

The mission is not.

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

# Determinism
In [Computer Science Problems](https://jhumelsine.github.io/2026/02/11/computer-science-software-engineering.html#computer-science-problems), I wrote:
>Each additional layer on the tech stack is for the benefit of humans, not the computer. The computer is just as content to execute machine code written by a human as it is to execute machine code generated from a compiler from source code written by a human.

Generative AI is adding another layer to the tech stack and moving us one more step away from bare metal.

Some are shouting that it's not the same as before: **This time it's different. The previous tech stack was deterministic. AI is not deterministic.**

I used to have the same opinion, but I saw a social media post a few weeks ago that changed my mind.
Unfortunately, I don't have a reference to it, but I can paraphrase it:
>We have always had non-determinism. Human developers have always been non-deterministic.

It took a little while for that to sink in.
While we think of the translation between layers of the tech stack is deterministic, and once implemented, it is deterministic, the process of defining the tech stack and implementing the tech stack is not deterministic.

If the same specification were given to several different teams, we'd have an equal number of completely different designs and implementations.

We're okay with people being non-deterministic in designing and writing code, but we're uncomfortable when AI does it.

It's akin to self-driving cars.
When a self-driving car causes a fatality, it's a failure of technology.
When a human driver causes a fatility, it's a tragic accident.
We demand perfection from technology when we do not demand it from ourselves.
It feels a bit like the [Uncanny Valley](https://en.wikipedia.org/wiki/Uncanny_valley).

AI didn't introduce non-determinism in software development.
It's always been there.

# We're Not Dead Yet
<img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExazNmaGpiZDB2OWZwMngwcXRwZjQxOTBvYTVnYXRtaTBqcW9mcWx4YSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/ZErfXKEMifIifbFM7c/giphy.gif" alt="We're not dead yet" title="Image Source: https://giphy.com/gifs/goodnightsunrise-not-dead-yet-werenotdeadyet-were-ZErfXKEMifIifbFM7c" width = "30%" align="right" style="padding: 20px;">

In [Will GenAI Take Our Jobs?](https://jhumelsine.github.io/2024/09/18/llm-genai.html#will-genai-take-our-jobs) I mused whether AI would take our jobs:
>While GenAI won’t take all our jobs, it will change the nature of many jobs. Those who know how to leverage GenAI will take the jobs of those who do not. Those who learn how to harness GenAI, especially as it improves, will remove much toil from their lives.

I used accountans and spreadsheets as an example:
>Spreadsheets removed accounting toil. How many professional accountants do you think are still in business who never learned to use Spreadsheets?

Writing good code was never about how fast you could type.
Writing good code was about understanding the [Domain](https://jhumelsine.github.io/2026/04/23/career.html#domain) and creating a good design that solves problems within that domain.

A good design makes code trivial.
I first mentioned this in an early [The Big Wheel Keeps on Turning](https://jhumelsine.github.io/2023/08/28/wheels.html), an early [Design Patterns](https://jhumelsine.github.io/table-of-contents#design-patterns-and-architecture) blog entry with:
>Though design patterns are very flexible, they are also based upon well-defined structures that encourage a modular design and separation of concerns. I know this sounds cliché, but once I’m satisfied with a design, my code often writes itself and it tends to just work. Difficult or stubborn bugs decreased significantly for me once I started to incorporate design patterns.

Though it sounds a bit pretentious, writing code from a good design felt a bit like the [Scribbling and Bibbling](https://www.youtube.com/watch?v=qzY2IeoveHw&t=100s) scene from _Amadeus_.
Let the AI do the _scribbling and bibbling_.

# Has It All Been In Vain?
I've spent the past three years documenting software ideas that I learned during my career in this blog.
Has it all been in vain?
Is AI making the practices I've been writing about obsolete?

I don't think so.
If anything, I think the practices in my blog may be even more important than ever.

AI won't create a good design by default.
It will need to be nudged by software engineers.

This along with other judgement calls is where software engineers will still add value.

The reports of the software engineering's death is greatly exaggerated.

# Context Window
LLMs are amazing.
I know there's no real thought process within them, but they certainly present the illusion of thought.

With all of their training, they have one limitation.
They know everything and nothing at the same time.
Their responses only make sense within the context of the current task, which is their [context window](https://en.wikipedia.org/wiki/Context_window).
It's their working memory.

Context windows may be large, but they are finite.
When context is at capacity, you don't know what important knowledge will be jettisoned to make room for new context being added.

It may remove some of its constraints about how to design and implement code.
It may remove domain knowledge.
It may remove design and implementation knowledge.
It won't remember what it forgot.

This is highligted in [**Coding Is Dead, Long Live Programming**](https://ian-cooper.writeas.com/coding-is-dead-long-live-programming).

It's similar to attention spans in people, but there are differences.
Short term memory is humans is limited to about [Seven Chunks](https://en.wikipedia.org/wiki/The_Magical_Number_Seven,_Plus_or_Minus_Two).
Context will eventually fade.
But we forget something, we tend to remember that we knew it at one point, and we know that we may need to refresh our memories.

When AI forgets something from its context window, it's pretty much gone.

# AI is a Tool
Generative AI is a tool.
It's a force multiplier.
Generative AI will make good software engineers better and bad software engineers worse.
A power saw in the hands of skilled crafters allows them to build cabinets quicker.
A power saw in the hands of knuckheads allows them to saw their thumbs off quicker.

This isn't too different from what I described in [Prompt Engineering Patterns](https://jhumelsine.github.io/2024/09/25/prompt-engineering-patterns1.html) and [Prompt Engineering Patterns Continued](https://jhumelsine.github.io/2024/10/18/prompt-engineering-patterns2.html), except that I didn't provide an examples that generated code.

I dabbled in code generating a few months ago during the most recent [Advent of Code](https://adventofcode.com/) challenges.
After I had spent several hours solving a challenge by hand, I copied the challenge description directly from the Advent page into ChatGPT and asked it to solve the problem in Java, which I then executed.
In many cases it returned the expected answer on the first try.
I won't say it was demoralizing that AI created a program that solve the problem in a matter of seconds rather than the hours it took me, but it did highlight how far Generative AI has come in just a few years since I retired.

I'm not sure if I'm disappointed that I'm missing this paradigm shift in the profession or grateful that I'm mostly able to avoid it.

Let's examine several ways to leverage Generative AI.

Two kinds of concerns for developers
* Those who care about the clarity and structure of the code.
* Those who care about delivering something to the customer/user as quickly as possible.

The first concern may not be as critical as before ... or maybe it's even more critical.

# Constraints
LLMs know programming languages and syntax, but they don't necessarily know how to design it well.

Without any constraints, it's as likely to design and implement a [Big Ball of Mud](https://blog.codinghorror.com/the-big-ball-of-mud-and-other-architectural-disasters/) as it is to design and implement a modular design.

It mostly spits out code based upon its repository accessible training data.
It's mirroring back to us the kind of code people have posted.
People post bad code.

We can ...


# Vibe Coding
The term [Vibe Coding](https://en.wikipedia.org/wiki/Vibe_coding) entered the lexicon a little more than a year ago, but it took off like wildfire.

Describe what you want from the LLM, and let it create the code for you.

Do we even need software developers anymore?
Let the Product Managers vibe the code by talking to the AI and telling it what they want.
This is basically what I did with my Advent example above.

"There is a new programming language—it's called Human." — Jensen Huang, Nvidia CEO

"From one gut feeling I derive much consolation: I suspect that machines to be programmed in our native tongues... are as damned difficult to make as they would be to use." — Edsger W. Dijkstra

<img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExcWhxZjlldzdlZXpiNDQ4Z25pdmlueWt3Nm9wMW9ydmNjZWt4MHljOSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/fSGqUm3IcVBESFM0hK/giphy.gif" alt="Monkey Paw" title="Image Source: https://giphy.com/gifs/leroypatterson-wish-monkey-paw-fSGqUm3IcVBESFM0hK" width = "30%" align="right" style="padding: 20px;">

I'm not sure that ___Vibe Coding___ was the correct term.
I think that maybe ___Monkey Paw Coding___ may have been more accurate.
The AI will give you want you asked for, but it may not be exactly what you wanted.

A [Saturday Night Live sketch](https://www.instagram.com/reel/C8xj8C1y7n1/) from the 1984 illustrates this.
In the sketch takes place at the retirement party for Ray the manager of a nuclear reactor.
His crew is slightly concerned that they'll have to monitor the nuclear reactor without him.

Ray reassures them that they'll be fine.
As he leaves he tells them there's only one thing they have to worry about: ___You can't put too much water into a nuclear reactor.___

One of the technicians walks to the control panel and is asked what she's doing.
She's putting more water in the nuclear reactor ... to top it off.
She interprets Ray's final instruction along the lines of: _You can't be too rich or too thin._
So go ahead and add more water. 

Her coworker thinks she's out of her mind.
He interprets Ray's final instruction as: __You can't put too much water into a nuclear reactor, because it will cause it to melt down._

The ambiguity is the humor mechanism for the rest of the sketch.

But what if a Product Manager were vibe coding a nuclear reactor with: ___You can't put too much water into a nuclear reactor___?

Prototype

Personal Project

Probably not Production

## Boilerplate Coding

### Partner Coding
Cleaner code => fewer tokens. Reference: https://beyond.minimumcd.org/docs/agentic-cd/operations/tokenomics/

How to maintain context better. https://martinfowler.com/articles/reduce-friction-ai/context-anchoring.html

# Summary
TBD

# References
TBD

# NOTES
* Focus upon AI.
    * I missed the AI paradigm shift. ChatGPT was released a few months before I retired. My comments are speculative and without any experience to back them up. Take everything I write here with a grain of salt.
    * AI – https://www.youtube.com/watch?v=lJiDBRh96oU
    * Vibe Coding?
    * Directing it like an Architect or Tech Lead.
    * If directing, you’ll need to learn Software Engineering skills, such as Design Patterns, Practices/Procedures like TDD, BDD, etc. Organization such as DDD. You may not have to know how to do these directly, but you may need to know how to do them well enough to direct the AI and evaluation what it has produced. I suspect you'll need these, in which case most of my blog entries will be useful and possibly even more important. However, if AI learns and understands these on its own, much like Data Structure implementation specifics or details lower in the stack, they may not be as important day-to-day. Maybe instead of supporting future software engineers, my blogs become an archive of what we used to do before Ai took over.
    * https://ian-cooper.writeas.com/coding-is-dead-long-live-programming

MORE:
AI will remove toil, and I think that writing the code is or will considered toil.

I think that leveraging AI to remove that toil successfully will be about the context window and token management.

There's a limit to how much the AI can remember. It's context window is a bit like an attention span or short term memory. People have limited attention spans and short term memory too, but we have the ability to build long term memory for a specific context. I don't know that AI has that type of long term memory.

I have a feeling that AI has complete general knowledge of its training date, and then short term memory in the context window for what's prompted or stored in context.md files.

When the context window capacity is maxed out, some knowledge is ejected, and we don't control what it is. It could be very important knowledge that becomes jetsam. It could be something that's domain specific. It could be an architecture, design or implementation rule. It could be something with behavior. I think that's when AI goes off the rails.

Context windows will continue to get larger, probably for a price, but there will always be a limit and there might even be diminishing returns. For example, doubling the context window's capacity might require four times the cost.

This is where I think boundaries could play a role. Don't force the AI to know the entire system. Define bounded contexts with a limited number of external resource dependencies. Give AI its guardrails and limitations and let it solve that isolated problem.

I don't think this is uniquely an AI approach. I think it works well for teams of people too. It was the theme of: Hexagonal Architecture – Why it works. Though this was part of my Hexagonal Architecture series, it doesn't apply to HexArch exclusively. It about dependency/knowledge management.

I introduced the concept of an Event Horizon. The idea is that these boundaries allow information to flow in only one direction, and it may be in or out depending upon the nature of the boundary. For stable/fixed boundaries, they have no knowledge or dependency of the outside world. Neither teams nor people need to be concerned outside the boundary. For unstable/flexible boundaries, the outside world has no knowledge or dependency upon them. This means that the design/implementation inside the unstable/flexible boundary can be anything from one class to thousands of classes. It won't affect the elements outside the boundary.

I described this a bit more two blog posts later with Nested Hexagons.

Jump forward almost a year and I Summarized the Test Double blog with this:
Future blogs will introduce additional concepts that will coalesce as well. I feel there may be a grand unified theory of software engineering that’s still just a bit beyond my grasp. If there is such a grand unified theory, I’d be willing to bet that Dependency and Knowledge Management is part of it.

Then a few months after that I Summarized in the Coupling and Cohesion that they are part of that grand unified theory as well.

The grand unified theory is starting to come together in my mind, but it's a ways down the road. I need to finish the Design Patterns and introduce and write about some design principles, such as SOLID. Think I think I can get into how and why this all works.

In order to have bounded contexts, you're going to need to architect and design the system with bounded contexts in mind and the Context Map that documents their interaction.

I suspect that AI could help with this too. The trick is to limit it to the bounded context and its contract. Don't allow it to cross the Bounded Context Event Horizon.

Then we have something sort of interesting. We have the higher level system at the Bounded Context and Context Map level, and then we have the individual lower level Bounded Contexts. We can use AI to craft each, but I think these should all be separate AI sessions.

https://ian-cooper.writeas.com/coding-is-dead-long-live-programming

https://martinfowler.com/articles/reduce-friction-ai/context-anchoring.html

https://beyond.minimumcd.org/docs/agentic-cd/operations/tokenomics/

https://coding-is-like-cooking.info/2026/03/test-driven-development-with-agentic-ai/

https://www.youtube.com/watch?v=Wcv0600V5q4

https://www.thoughtworks.com/en-us/insights/podcasts/technology-podcasts - early April about AI and Software Engineering.

https://cacm.acm.org/opinion/redefining-the-software-engineering-profession-for-ai/

https://www.thoughtworks.com/en-us/insights/podcasts/technology-podcasts/how-feels-software-engineer-ai-changing-relationship-code

Add the concept of a chunk. It's more efficient with memory.

Reference good software practices. You don't need to invent, design or implement practices. You only need to reference them.

Mythical Man Month - Surgical Team. https://herbertograca.com/2018/09/10/3-the-surgical-team/

What is CRAP in software complexity?

https://www.thoughtworks.com/en-us/insights/podcasts/pragmatism-in-practice around April 10-23. AI and the Workforce. Not posted when I'm writing this.

CRAP - https://testing.googleblog.com/2011/02/this-code-is-crap.html

Use agents to find the signal from the noise in log files. Connects to observability. See: https://x.com/unclebobmartin/status/2047679611752862121

Dave Farily new AI playlist - https://www.youtube.com/watch?v=8WJ1XVBh8NA&list=PLwLLcwQlnXBxEN_e5gfzfKmUS9XW2U8Jz

What about an agent that represents the user? It could use the product and provide feedback. It wouldn't be a pass/fail test. It would be a sounding board. I don't know if one could prompt a user well enough to make this useful, but it feels like an interesting idea.

Emily Bache video about AI and good software practices - https://www.youtube.com/watch?v=_erJ5EdQFYU

Dave Farley podcast with AWS guy - https://rss.com/podcasts/theengineeringroom/2787831/

https://www.aihero.dev/ and https://github.com/mattpocock

https://www.youtube.com/watch?v=XDNXLdwq114

https://martinfowler.com/articles/exploring-gen-ai/harness-engineering-memo.html
https://martinfowler.com/articles/harness-engineering.html

++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++

https://www.youtube.com/watch?v=v4F1gFy-hqg

Vibe coding is fine for prototypes and small personal projects where you're the only user. But it's probably not suitable ... yet ... for production code.
GenAI is great for filling in boilerplate code. This could be via autocomplete features or just telling it to create the equals() method for a Java class. It saves some typing and removes some toil, but it's not going to yield a huge performance improvement.
I think the Goldilocks sweet spot is somewhere between.

Deep vs Shallow is not a situation of either/or. It's yes/and.

Ousterhout has the right idea, but it's the wrong presentation or at least focusing upon the wrong attribute.

It's about contracts and interfaces ... period. It's not about how much code is needed to implement the behaviors defined in those contracts and interfaces.

Therefore, it's completely possible, and I think quite reasonable, for a deep contract's implementation to be composed of other deep-ish classes, each with their own contracts, which help realize the behavior for the deep contract in which they reside.

This isn't small set of several large monolithic blocks versus a large set of small blocks. It's more akin to a composition tree structure.

Therefore, we could indeed have a deep class that's small, because it delegates its sub-behaviors to other classes. It's like the contractor on a construction site. He doesn't do all of the work, but he manages the work and delegates it to other specialized contractors, such as carpenters, plumbers and electricians.

I think the same applies to AI. Humans will be the contractors and the specialized contractors will be AI agents.

If he liked Ubiquitous Language from DDD, I can't wait to see how he feels about Bounded Contexts. Haha.

We don't have to teach AI many of these software practices. We can probably reference them, and it will know from its training how to use them. Some guidance will probably still be needed for context to the specific situation, but we won't have to start from scratch.

Don't worry, I don't do much AI either, but I think you'll find this talk really inspiring 

My Goldilocks sweet spot sounds along these lines.

Humans have been using technology to remove toil for millennia. Coding has now become toil.

Software engineering was never about typing. It really wasn't about the code either. However, it was necessary because we didn't have the technology to avoid it. Now we have the technology.

The heart of software engineering is the domain and the design. AI can't handle that ... yet.

For a pretty good chunk of my career, I didn't start the coding until I was satisfied with my design. Then the code pretty much wrote itself after that. That's what I think AI generated code will be like, except that I would not be the one writing the code.

People are equating working with AI as being like pair-programming. There will be some elements of this, but I think the model is going to be closer to a team lead directing the people on his team. Except that now, the team will be AI agents rather than other developers. Much like human team, the lead still comes up with the overall design, divides it up among the team and checks the work.

The main difference is that the team lead can build a team of as many AI agents as needed and as narrowly focuses as desired. For example, one AI agent could have a single responsibility for mutation testing. Teams can be a different composition of AI agents. And scale is not an issue. An agent team could be defined for just one class, which is something you wouldn't do with human developers.

We have two issues with AI:

Token cost
Limited context window


We don't want to blow through our token budget. We also don't want agents to forget important things, just because their context window is full.

We have the same issues with human developers. We manage this with systems will well defined boundaries ... or at least we should do that. My gut feeling is that the practices we've used for humans to develop software will be useful for agents too.

I also think there are some practices that will help with hallucinations, such as mutation testing.

I suspect that good boundaries, such as Bounded Contexts, or Deep Classes in using Ousterhout's terms, will be key to this. Just as these boundaries protect the human mind from having to know about too many details, they also shield the AI from those details it doesn't care about either.

For example, the AI should only know about the contracts of other classes. It shouldn't know about the entire design. This will help keep the number of tokens and the size of the context window manageable ... I think.

However, this does introduce some degree of duplication risk. Different Bounded Context agents could develop classes with similar behaviors. Maybe a good design includes a rover agent whose job is to look for duplication candidates and return findings back to the human designer. I would not trust them to make the changes, but I would consider their findings.

If one creates an army of agents, how do they communicate? I don't think it would be wise for agents to know about another. However, I could see them sharing artifacts. For example, the code generated by developer agents as output could be the input to the mutation testing agent. It feels cleaner, and all artifacts would be viewable for human confirmation too.

I've also heard of prompts along the lines of: Write code like Kent Beck.
