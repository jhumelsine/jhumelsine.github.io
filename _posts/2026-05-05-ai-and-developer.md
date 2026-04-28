---
title: WORK IN PROGRESS – AI and the Single Developer
description: TBD
unlisted: true
---

# Introduction
I have no business writing this blog.

Right now the greatest disruptive technology is Generative AI.
Those who learn how to leverage it will have an advantage over those who do not.

I have limited experience with Generative AI.
ChatGPT was released upon the world a few months before I retired.
In that short period of time, it's flooded the Software Engineering community.
Many companies have made drastic cuts across the board to be filled by AI, as can be seen in headlines like these: [**Block lays off nearly half its staff because of AI. Its CEO said most companies will do the same**](https://www.cnn.com/2026/02/26/business/block-layoffs-ai-jack-dorsey).

I think this may be shortsighted.
The reports of the death of software engineering is greatly exaggerated.

Generative AI is a tool.
It's a force multiplier.
Generative AI will make good software engineers better and bad software engineers worse.
A power saw in the hands of skilled crafters allows them to build cabinets quicker.
A power saw in the hands of knuckheads allows them to saw their thumbs off quicker.

Generative AI has limited context.
It tends to have a short attention span.
When context is at capacity, you don't know what important knowledge will be jettisoned to make room for new context being added.

This is highligted in [**Coding Is Dead, Long Live Programming**](https://ian-cooper.writeas.com/coding-is-dead-long-live-programming).
https://beyond.minimumcd.org/docs/agentic-cd/operations/tokenomics/
https://martinfowler.com/articles/reduce-friction-ai/context-anchoring.html

Let's examine several ways to leverage Generative AI.

Two kinds of concerns for developers
* Those who care about the clarity and structure of the code.
* Those who care about delivering something to the customer/user as quickly as possible.

The first concern may not be as critical as before ... or maybe it's even more critical.

### Vibe Coding
You can never put too much water into the nuclear reactor. https://www.instagram.com/reel/C8xj8C1y7n1/

### Boilerplate Coding

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
