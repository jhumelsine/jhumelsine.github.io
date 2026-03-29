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
