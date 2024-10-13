---
title: DRAFT – Prompt Engineering Patterns Continued
description: TBD
unlisted: true
---

# Introduction
I introduced __Prompt Engineering Patterns__ in my previous [blog](https://jhumelsine.github.io/2024/09/25/prompt-engineering-patterns1.html). This blog will continue with a few more.

# Pre-Prompt or Built-In Prompts
A Large Language Model/Generative AI (LLM/GenAI) platform may include some pre-prompts or built-in prompts to set up a few ground rules. These prompts are intrinsically added at the start of each session.

They may instruct the LLM/GenAI to generate results of a certain form or avoid questionable content such as:
* Profanity
* Racist, sexist, gender insulting language
* Questionable responses, such as instructions on how to build a bomb
* Etc.

These rules aren’t programmed in the traditional sense. They are in natural language much like the prompts I showed in the previous blog and will continue to show in this blog. They tend to be hidden from the user, but sometimes enterprising users find them.

Some of [Apple’s prompts were revealed]( https://apple.slashdot.org/story/24/08/06/2113250/apples-hidden-ai-prompts-discovered-in-macos-beta) with instructions such as:
> You are an assistant which helps the user respond to their mails. Given a mail, a draft response is initially provided based on a short reply snippet. In order to make the draft response nicer and complete, a set of question and its answer are provided. Please write a concise and natural reply by modifying the draft response to incorporate the given questions and their answers. Please limit the reply within 50 words. Do not hallucinate. Do not make up factual information.

This YouTube video podcast recording, [ChatGPT Pre-Prompt Text Leaked](https://www.youtube.com/watch?v=QWhcFRcIbPw), describes ChatGPT’s pre-prompts that tell it that it has access to the browse tool, and that it can use it to get up-to-date information from the internet. The pre-prompts include instructions to guide it in obtaining this information.

Depending upon your LLM/GenAI platform, you may have the ability to add your own pre-prompt built-ins. But if you don’t have this ability, then store any personal pre-prompts and copy-and-paste them at the start of each session.

# Prompt Engineering Patterns
As with my previous [Prompt Engineering Patterns Blog Entry](https://jhumelsine.github.io/2024/09/25/prompt-engineering-patterns1.html), I won’t list the GenAI output. I encourage you to copy-and-paste my prompts into your LLM/GenAI of choice, such as ChatGPT, to see what is generated for you. Feel free to experiment as well.

## Cognitive Verifier Pattern
Our interaction with an LLM/GenAI may be too broad at first. The __Cognitive Verifier Pattern__ instructs the LLM to break down an initial question into smaller more specific questions, which when answered it can aggregate to a more accurate answer.

Here’s the prompt:
> When you are asked a question, follow these rules:
>
>Generate a number of additional questions that would help more accurately answer the question.
>
>Combine the answers to the individual questions to produce the final answer to the overall question

### How to achieve a healthier lifestyle
I asked ChatGPT:
> When you are asked a question, follow these rules:
>
>Generate a number of additional questions that would help more accurately answer the question.
>
>Combine the answers to the individual questions to produce the final answer to the overall question
>
>How do I change my lifestyle to be healthier?

It asked about a half dozen follow up questions about my current lifestyle, what I wanted to focus on, medical conditions, time availability, gym access, etc. It also provided me with a preliminary write-up on the importance of a healthy lifestyle.

I answered with one response with one word or short phrases, such as:
>1. Answer1, 2. A bit more for Answer 2, … 6. Final Answer for 6.

It provided me with a writeup based upon my specific answers.

### Retirement Planning
> When you are asked a question, follow these rules:
>
>Generate a number of additional questions that would help more accurately answer the question.
>
>Combine the answers to the individual questions to produce the final answer to the overall question
>
>How do I make sure I have enough money for retirement?

ChatGPT responded similarly with 6 questions. I provided made up answers. It produced a generic report, but it was tailored to my responses. For example, it asked if my plans needed to consider dependents to which I said I had a 10-year-old child and I wanted to make sure that college was covered. It suggested a 529 plan, which specific type of college funding plan available in the United States.

### Alternatives
The prompt doesn’t need to be based upon questions. Here are two other examples
> When you are asked to create a recipe, follow these rules. Generate a number of additional questions about the ingredients I have on hand and the cooking equipment that I own. Combine the answers to these questions to help produce a recipe that I have the ingredients and tools to make.

I asked it:
> Create a recipe that's combines elements of a traditional Thanksgiving meal with Japanese cuisine.

Based upon my answers, it created a recipe for: __ Sesame Ginger Turkey with Soy-Glazed Sweet Potatoes and Stuffing__. Your responses will differ.

Always be careful with generated recipes. LLMs/GenAIs have been known to create deadly concoctions. See: [AI can generate recipes that can be deadly. Food bloggers are not happy](https://www.npr.org/2024/09/23/g-s1-23843/artificial-intelligence-recipes-food-cooking-apple). 

Nothing in my generated recipe looked like it would kill anyone, but that doesn’t mean that it would be tasty. But we don’t need GenAI to create nasty dishes. Susan Stamberg’s Cranberry Relish recipe is broadcast on National Public Radio (NPR) every Thanksgiving season. I made it one year – horrible. See: [Cranberry Relish: The NPR Recipe That Divides Thanksgiving Tables](https://www.npr.org/sections/thesalt/2018/11/16/666296027/cranberry-relish-the-npr-recipe-that-divides-thanksgiving-tables).

And here's an example to help plan a trip:
>When you are asked to plan a trip, follow these rules. Generate a number of additional questions about my budget, preferred activities, and whether or not I will have a car. Combine the answers to these questions to better plan my itinerary. 

## Audience Persona Pattern
The [__Persona Pattern__](https://jhumelsine.github.io/2024/09/25/prompt-engineering-patterns1.html#persona-pattern) asks the LLM/GenAI to respond in the context of persona.

The __Audience Persona Pattern__ asks the LLM/GenAI to respond to a specific audience.

The Personal Pattern and Audience Persona Pattern can be used together.

To invoke the Audience Persona Pattern, add phrases such as:
>Explain it to me like I’m \<persona\>.
>
>Assume that I’m a \<persona\>.
>
>Present it to a group of \<personas\>.

And then replace <persona> with the desirec context.

### Physics Professor with High School Students
>You’re a college physics professor. You’ve been invited by the local high school’s physics teacher to give a presentation to the advanced physics class on quantum mechanics. These are high school students who know some calculus. They also know classical physics, but this will be their first introduction to quantum mechanics.
>
>Create a presentation about quantum mechanics suitable for this advanced physics class. Include the historical background of quantum mechanics. Include some of the crazy ideas. Keep it fun.

### Physics Professor with College Students
Compare the above with this:
>You’re a college physics professor. You're presenting a lecture on quantum mechanics to a classroom of physics majors. They also know classical physics and all the mathematic principles needed to understand quantum mechanics, but this will be their first introduction to quantum mechanics. 
>
>Create a presentation about quantum mechanics suitable for this physics class.

### Bug Presentation with Grade School Kids
>Help! I’ve been tapped at the last minute to give a presentation about insects to a group of third and fourth graders. I don’t have a lot of prep time.
>
>Come up with a set of slides that describe insects to these young kids in a way that’s fun and educational. The presentation will be about 30 minutes long.

## Flipped Interaction Pattern
Most prompt patterns have focused upon the human driving the conversation with the LLM/GenAI either by the human making requests or asking questions.

The __Flipped Interaction Pattern__ puts the LLM/GenAI in the driver’s seat; although, we do need to tell it that we’re putting it in the driver's seat.

Here are some ways to activate this pattern:
>I would like you to ask me questions about \<a topic\> to achieve \<a goal>\.
>
>You should ask questions until \<a condition\> is met to achieve the goal.
>
>_Optionally_ Ask me the questions one at a time, two at a time.
>
>Ask me the first question.

### Career Councilor
I snuck this pattern into the previous blog with the [Career Councilor](https://jhumelsine.github.io/2024/09/25/prompt-engineering-patterns1.html#career-councilor) example. Look there.

### Computer Science TA Tutor
>Act as a Computer Science tutor.
>
>I'm taking a class on Data Structures.
>
>Ask me questions, one at a time, about Data Structures. If my answer is wrong, tell me why it is wrong and ask me another question on the same topic. 
>
>If the answer is correct, then continue with more difficult questions.

With a few simple adjustments, this prompt could be updated for most topics.

### Church Finances
>My church is experiencing a bit of a short fall in donations. Our expenses exceed our offerings. I would like you to ask me questions to help me address our financial shortfall. You should ask questions until you have sufficient information about our financial situation and some ideas of how to address it.
>
>Ask me the first question.

Adjust for any other volunteer organization with financial concerns.

## Chain of Thought Pattern
Solving a large problem often requires breaking it down into smaller problems. This is often the case with math problems when most of us were in school where we had to show so that we’d make fewer mistakes and demonstrate to our teachers that we understood the process.

We can do the same thing with LLMs and GenAIs. By asking them to solve the problem step-by-step, indicating their chain of thought and showing their work. They will break the problems down into smaller steps as well.

Recent updates in some LLMs, such as ChatGPT, may not require this pattern as frequently as before. See [Reasoning Models](https://platform.openai.com/docs/guides/reasoning), which recommends:
>__Avoid chain-of-thought prompts:__ Since these models perform reasoning internally, prompting them to "think step by step" or "explain your reasoning" is unnecessary.

Until reasoning is common in LLMs, we can still use it. This prompt can activate the pattern:
>For this session, provide your step-by-step reasoning with your chain of thought, and show your work.

I’ve tried all the examples below several times. They’ve always returned the correct answers to me, even if the form of their answers has varied.

### Medical Test Results
>For this session, provide your step-by-step reasoning with your chain of thought, and show your work.
>
>Assume that a medical test returns an accurate indication of the condition 99% of the time. 0.1% of the general population has this condition. If a test comes back positive, what's the probability that the patient has the condition?

The answer is a bit surprising at only about 9%. It uses [Bayes’ Theorem](https://en.wikipedia.org/wiki/Bayes%27_theorem) which can be a bit intimidating.

To get an easier to understand explanation, follow up with:
>Explain it to me without the detailed math.

### Prisoner’s Dilemma
>For this session, provide your step-by-step reasoning with your chain of thought, and show your work.
>
>Explain the Prisoner’s Dilemma to me.

Follow up with:
>Is there an incentive to cooperate over multiple games?

And finally:
>Give me some real-world examples of the Iterated Prisoner's Dilemma

### Algebra Word Problem
>For this session, provide your step-by-step reasoning with your chain of thought, and show your work.
>
>If 2 men can mow 2 lawns in an hour, how long will it take 4 men to mow 1 lawn?

### Unit Conversion Problem
>For this session, provide your step-by-step reasoning with your chain of thought, and show your work.
>
>How fast does human hair grow in miles per hour?

### Algebra Word Problem Revisited
>For this session, provide your step-by-step reasoning with your chain of thought, and show your work.
>
>Two trains on the same track are on a collision course toward each other. They are 100 miles apart. The first train travels at 25 miles per hour. The second train travels at 35 miles per hour.
>
>There is a bird flying between them. It starts at the first train and flies toward the second train at 50 miles per hour. When it reaches the second train it immediately turns around and flies back to the first train at 50 miles per hour.
>
>It flies back and forth between the two trains at 50 miles per hour until it is squashed in the collision. How far did the bird travel?

## Template Pattern
This is not to be confused with the [Template Method Design Pattern](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html), which is something completely different.

The __Template Pattern__ tells the GenAI if you want the result in a specific format. It’s good to know the technique, but I don’t use it often often. I’ve found that the default formatting tends to provide what I want, and if I want something other than text, such as a table, I ask it to reformat in tabular form, and that tends to work well too.

But if consistent responses are needed, such as via a form letter, then this may be the pattern to use.

The prompts are long the lines of:
>I am going to provide a template for your output
>
>X is my placeholder for content
>
>Try to fit the output into one or more of the placeholders that I list
>
>Please preserve the formatting and overall template that I provide
>
>This is the template: PATTERN with PLACEHOLDERS

You will need to replace "X" with an appropriate placeholder, such as "CAPITALIZED WORDS" or "<PLACEHOLDER>". You will then need to specify a pattern to fill in, such as "Dear <FULL NAME>" or "NAME, TITLE, COMPANY".

### Thanksgiving Dinner
This example won’t start with the Template Pattern, but it will get there soon.
> I'm hosting Thanksgiving dinner for 8 people in total. Create a traditional menu for me.

Adjust the menu as desired by adding or subtracting items.

This is the prompt with formatting:
> Please create a grocery list for me from this menu. I am going to provide a template for your output . \<placeholder\> are my placeholders for content. Try to fit the output into one or more of the placeholders that I list. Please preserve the formatting and overall template that I provide. This is the template: Aisle \<name of aisle\>: \<item needed from aisle\>, \<qty\> (\<dish(es) used in\>)

And finally:
> I want to serve the meal at 3PM. Create a step-by-step schedule of what I should be doing so that the meal can be served at the desired time.

### Hexagonal Architecture Q&A
This is interesting, since it suggests that ChatGPT understand Markdown Language.
> I am going to give you a template for your output. CAPITALIZED WORDS are my placeholders. Fill in my placeholders with your output. Please preserve the overall formatting of my template. My template is, with Topic, Question and Answer each listed separately. I.e., do not put the Answer on the same line as the Question:
> 
>##Topic: \<TOPIC NAME\>
> 
>\*\*\*Question\*\*\* \<ONE SENTENCE QUESTION\>
> 
>\*\*\*Answer\*\*\* \<ONE PARAGRAPH ANSWER\>
> 
>Using information listed in the following URLs:
>* https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html 
>* https://jhumelsine.github.io/2023/10/28/hexagonal-architecture-structure.html 
>* https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html 
>* https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html 
>* https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html 
>* https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html 
>* https://jhumelsine.github.io/2023/11/30/hexagonal-architecture-adapter-flexibility.html, 
>
>Create at least 20 questions and answers organized by topic.

## Format of the Recipe Pattern
My previous Thanksgiving preparation schedule prompt is a form of this pattern, but we can add more structure to the prompt.
>I would like to achieve X 
>I know that I need to perform steps A,B,C 
>Provide a complete sequence of steps for me 
>Fill in any missing steps 
>(Optional) Identify any unnecessary steps

You will need to replace "X" with an appropriate task. You will then need to specify the steps A, B, C that you know need to be part of the recipe / complete plan.

### Purchase a Car
> I would like to purchase a car. I know that I need to perform steps like deciding upon the type of car, find a dealership, secure financing, ... . Provide a complete sequence of steps for me. Fill in any missing steps.

## Alternative Approaches Pattern
Rather than trying to craft the perfect prompt, ask the LLM/GenAI for some prompt examples. That is, let the LLM/GenAI brainstorm some ideas for you.

Here are some phrases to trigger this pattern:
>If there are alternative ways to accomplish a task X that I give you, list the best alternate approaches 
>(Optional) Compare/contrast the pros and cons of each approach 
>(Optional) Include the original way that I asked 
>(Optional) Prompt me for which approach I would like to use

You will need to replace "X" with an appropriate task.

Some examples:
>For every prompt I give you, If there are alternative ways to word a prompt that I give you, list the best alternate wordings. Compare/contrast the pros and cons of each wording.
>
>For anything that I ask you to write, determine the underlying problem that I am trying to solve and how I am trying to solve it. List at least one alternative approach to solve the problem and compare / contrast the approach with the original approach implied by my request to you.

### Thanksgiving Dinner Revisited
>When I ask you a question, I want you to generate several prompts that could be used to obtain responses from ChatGPT. List the relative strengths and weaknesses of each prompt. Summarize the prompt strategies when done.

Then prompt:
>Help! I'm hosting Thanksgiving dinner this year with 8 people, including my new in-laws. My mother-in-law has always hosted the "perfect" Thanksgiving dinner. I've never even cooked a turkey before. I'm really intimidated, and I'm freaking out a bit. I still have time, but I don't know where to start. I don't even know what I should be asking to get help.
>
>How can I present a successful Thanksgiving dinner and impress my new in-laws?

This should generate several potential prompts with their relative strengths and weaknesses.

Continue with:
>Can you come up with prompt that maximizes the strengths of these suggestions and minimizes their weaknesses?

If you like its new prompt, then add:
> Execute that optimized prompt.

## Ask for Input Pattern
We often start prompting with a precursor prompt setting up the “rules” of the session. Sometimes, the LLM/GenAI gets a bit overly enthusiastic, and it starts generating responses to what it thinks we want from the precursor prompt set up.

We can ignore the preliminary response, or we can add a preemptive pattern in the precursor prompt set up by ending it with:
>Ask me for the first task.

And _task_ can be specific to the context of your prompt too.

## Fact Check List Pattern
LLM/GenAI is prone to hallucinate or fabricate facts in its responses. We always need to check its responses. The __Fact Check List Pattern__ leverages the LLM/GenAI to give us some of the facts that we should check in its responses.

This pattern won’t double check the facts for you. But it will provide a summary of the facts that you should check yourself.

Include the following to activate this pattern:
> Whenever you output text, generate a set of facts that are contained in the output. The set of facts should be inserted at the end of the output. The set of facts should be the fundamental facts that could undermine the veracity of the output if any of them are incorrect.

### Constitutional Scholar with Fact Check List
Returning to a previous example, let’s add fact check list to the Constitutional Scholar.
> Act as a United States Constitutional scholar. Answer my questions about the US Constitution including the Amendments. Provide the sections of the Constitution and Amendments as references to your answers. Include the text as well as the section where that text resides. 
>
>Whenever you output text, generate a set of facts that are contained in the output. The set of facts should be inserted at the end of the output. The set of facts should be the fundamental facts that could undermine the veracity of the output if any of them are incorrect.
The LLM/GenAI should confirm this and here are several questions to ask, one at a time:
> How do justices reach the Supreme Court?
>
> What does the US Constitution say about tariffs?
>
> Who has the right to vote?

## Tail Generation Pattern
This blog is getting long. Do you remember the first prompt patterns presented without scrolling back up? Sometimes our conversations with an LLM/GenAI get long, and the LLM/GenAI may start to forget some of the early prompts. The __Tail Generate Pattern__ asks the LLM to remember the initial context of the session by having it repeat it. It’s much like people repeating temporary phone numbers to keep them in their short-term memory until they can write them down or place the call.

The pattern can activated by adding the following at the end of the initial prompts:
>At the end, …

And then reemphasize the original context. This isn’t quite as cut-and-paste as some other prompt patterns, so here’s an example.

### Meeting of the Minds Revisited
> Act as four personas: Galileo Galilei, Isaac Newton, Albert Einstein and Stephen Hawking.

>They are sitting at a round table having a discussion. They all speak English. Hawking’s speech is not incumbered by his illness. 
>
>The men of past histories know the men in their relative futures, but they don’t have detailed knowledge of future discoveries. 
>
>They will have discussions about the nature of the universe as they understand it. They will be curious about what the other men know, especially the men of their future. They will inquire for more insights. 
>
>Each conversation will be primed by me as a moderator. The conversation should progress in several rounds. 
>
>At the end of a conversation, ask me if I want the current discussion to continue or present a new topic among these minds of science.
>
>Do not start the discussion until I have introduced the first topic.
Then between responses, continue the conversation with the following, and choose to continue any topic that you find interesting:
> Who were your inspirations?
>
> Are you a man of faith, and do you find that faith supports your scientific work or conflicts with it?
>
> Is mathematics an invention or a discovery?
>
> I feel that the axioms of any mathematical context, such as geometry, is an invention. Then the conclusions from those axioms, such as theorems, is a discovery. I like to use games, such as chess, as an example. The rules of chess is an invention. They fit on a single sheet of paper. Yet the strategy and tactics of how to play the game of chess well is a discovery based upon generations of play and study. They fill volumes of books. With that analogy in mind, please continue your discussion of mathematics as invention or discovery.

# Summary
Prompt engineering patterns are not exclusive. Use them in combination. The previous _Meeting of the Minds_ used at least three patterns, possibly a few more.
Explore more patterns on your own. Follow the reference link below to more prompt engineering patterns than I could jam into this blog post.

# References
[references](https://jhumelsine.github.io/2024/09/25/prompt-engineering-patterns1.html#references)
