---
title: DRAFT – Prompt Engineering Patterns
description: How to have a dialog with a Large Language Model
unlisted: true
---

<img src="https://live.staticflickr.com/7259/7608126362_9b07f50b97_b.jpg" alt="Oracle of Delphi" title="Image Source: https://www.flickr.com/photos/gandalfsgallery/7608126362" width = "50%" align="center" style="padding-right: 20px;">


# Introduction
I wrote about __Large Language Models__ (LLMs) and __Generative AI__ (GenAI) in my previous [blog](https://jhumelsine.github.io/2024/09/18/llm-genai.html). This blog will provide suggestions on how to invoke good results from LLMs and GenAI via [prompt engineering](https://en.wikipedia.org/wiki/Prompt_engineering).

I learned most of these prompt patterns from two online courses I recently audited: [Generative AI: Prompt Engineering Basics](https://www.coursera.org/learn/generative-ai-prompt-engineering-for-everyone/home/welcome) and [Prompt Engineering for ChatGPT](https://www.coursera.org/learn/prompt-engineering/home/welcome). There are many more resources available in [References](#References) at the bottom of this blog entry.

I will describe Prompt Engineering Patterns with descriptions and prompt examples. I will not provide GenAI results for the following reasons:
* They will consume too much space if listed here.
* Watchvcing GenAI generate responses will have a greater impact than seeing my responses.
* Your responses and my responses will not be the same. GenAI results always vary, even with the same prompt.

I recommend having an LLM/GenAI platform open as you read this. [ChatGPT](https://chatgpt.com/) provided by [OpenAI’s](https://openai.com/) offers paid subscriber and free ChatGPT options. I use the free version of ChatGPT, which requires a login. It can be created on the platform, or you can use other platform accounts, such as Google. While I’ll confirm my prompt examples in ChatGPT, I think most will work in other LLM/GenAI platforms as well.

Copy-and-paste my prompt examples into your chosen LLM/GenAI platform as you proceed through this blog.

# Prompt Engineering Basics
LLMs model the relationships among words, technically tokens, based upon billions of examples from the internet.

When text is provided to an LLM’s prompt, GenAI uses the provided text to generate [the next most likely word](https://jhumelsine.github.io/2024/09/18/llm-genai.html#the-next-most-likely-word) based upon the relationship of words in the model. It adds this generated word to the text and repeats the process to generate the next most likely word. One doesn’t need to know the algorithmic details of how it does this. I don’t quite understand it myself. All one really needs to know is that the LLM is generating a series of the next most likely words using the text prompt as the seed to generate the next most likely words.

Enter a set of random words as a prompt, and GenAI will balk with a response indicating that it can’t do too much with random words. But give it something with contextual form, such as an request or a question, and it will build a response.

# Good Prompt Basics
The initial prompt is a conversation starter with the LLM/GenAI. Better prompts will elicit better responses from the LLM/GenAI. Prompting is communicating with the LLM. Good prompting is about good communication. A good prompt will include the following (mostly from: [Generative AI: Prompt Engineering Basics](https://www.coursera.org/learn/generative-ai-prompt-engineering-for-everyone/home/welcome)):
* __Instructions:__ Provides guidelines regarding the task. That is, what do you want GenAI to do.
* __Context:__ Lists details useful for generating relevant content. That is, narrow the scope of the conversation.
* __Input Data:__ Includes additional information provided by you in the prompt. For example, you may want it to critique something you’ve written, such as an email.
* __Output Indicator:__ Describes the structure of the generated response. For example, do you text, an outline, a table, etc.?

For example, try copy-and-paste this as one prompt:
>List the checks and balances of the three branches of the United States Federal government. 
>
>List them in a 3 by 3 grid with each branch listed as the header for each row and column in the grid. 
>
>The diagonals will be the same branch in the row and column headers, so place "N/A" in those cells.

Here is a more conversational interaction to reach the same outcome:
>What are the checks and balances of the three branches of the United States Federal government?

This returns a text based respons. If a table is desired, then continue with this additional prompt:
>Put that in a table.

I thought the second, more conversational and more natural prompts gave me better results than the first one, but rather than place __N/A__ on the diagonals, which would suggest that a branch has a check and balance upon itself, it lists powers associated with that branch. So I added this third request to clean that up:
> Put "N/A" in the diagonal cells which compare a branch to itself, such as Legislative/Legislative, Executive/Executive and Judicial/Judicial.

This cleaned up the table.

# Prompt Engineering Pattern Introduction
It should be obvious by the header at the top of each blog and the subject matter for most of my blog entries that I’m a fan of [design pattern](https://jhumelsine.github.io/2023/08/29/toolbox.html). Whereas traditional design patterns apply to an object-oriented domain, Prompt Engineering Patterns apply to a LLM/GenAI domain. They are solution approaches to common problems when interacting with LLMs/GenAIs. Unlike traditional design patterns, which require some technical expertise, prompt engineering patterns are not technical. Each pattern is typically a phrase or two that invokes a certain type of response from the LLM/GenAI. Multiple patterns may be used in the same prompt or session.

Prompt Engineering Patterns are coalescing to some degree, but the same technique may have different names depending upon the source. There are also many more patterns waiting to be discovered.

# Best Practices for Prompt Creation
[Generative AI: Prompt Engineering Basics](https://www.coursera.org/learn/generative-ai-prompt-engineering-for-everyone/home/welcome) continued with more suggestions for good prompts:
* Clarity:
    * Use clear concise language
    * Avoid jargon and complex terms
    * Provide explicit instructions
* Context:
    * Establish the context
    * Include relevant information
* Precision:
    * Be specific
    * Use examples
* Role-play/Persona pattern:
    * Assume a persona
    * Provide context for role-play

In essence, if your prompts would make sense to a person, then they will probably make sense to the LLM/GenAI.

# Prompt Engineering Patterns
The main source for many of these Prompt Patterns is [Prompt Engineering for ChatGPT](https://www.coursera.org/learn/prompt-engineering/home/welcome).

## The Persona Pattern
In the real world, if you wanted medical advice, you’d ask a doctor. If you wanted financial advice, you’d ask a financial planner. If you wanted tax advice, you’d ask a tax accountant.

The __Persona Pattern__ allows the LLM/GenAI to respond as if it were that persona, which could be:
* A role, such as doctor, but DO NOT take any of its medical advice seriously - always confirm with a real doctor
* A real person, living or dead, as long as there’s enough public information about that person
* A fictional character
* An Inanimate object

To activate the Persona Pattern use phrases such as:
* You are a _persona_ …
* Act as a _persona_ …
* Provide a response from a _persona_ perspective

Replace _persona_ with something more concrete. Here’s a prompt with more umph to it:
> You will act as a _specific_ expert. Your objective is to engage in a comprehensible <specific> session with me. Begin by asking a series of detailed questions, one at a time, to gather all essential information required to craft the most tailored and memorable <thing> based upon my specific preferences, interests, etc.

## Examples to try
Here are a few examples to try. Copy-and-paste them into your LLM/GenAI platform.

### Career Councilor 
>Act as a career coach. I'm looking for new career opportunities. Help me come up with some options. Interview me to gain an assessment of what jobs I should consider pursuing.
>
>Begin by asking me questions to learn more about me. Ask about my educational background, my previous employment experience, my skillset, the types of problems that I like to solve. Also ask anything else that provides more insights into the type of jobs for which I would be a good match.
>
>Ask me one question at a time. Ask follow up questions to my specific answers but make sure to return to the high level questions as well.
>Once you have acquired enough information about me via my answers, then provide a list of careers for which I would be a good match.

What I like about this prompt is that it exposes you to considerations you may not have thought of yourself. It's a form of idea exploration. If there's a good idea, continue to pursue it, either in the real world or via the LLM. If it's not so much a good idea, then ignore it.

Create a new session and prove the same promtp. Give it slightly different answers and see what else it may produce.

### Constitutional Scholar
> Act as a United States Constitutional scholar. Answer my questions about the US Constitution including the Amendments. Provide the sections of the Constitution and Amendments as references to your answers. Include the text as well as the section where that text resides.

Here are some sample questions:
* How long is the term of the US President?
* Tell me about patent law.
* What's the emoluments clause?
* What are checks and balances and how do they work?
* What can Donald Trump teach us about the Constitution?

### Interfaith Discussion
LLM/GenAI can take on multiple personas. While this next example starts off sounding like a bad bar joke, it does yield some interesting results.
> For this session you will take on 4 personas. They will be a Jewish Rabbi, a Catholic Priest, a Protestant Minister and a Muslim Imam. Each will respond from their point of view. Each will be polite. Each will cite scriptures to reinforce their response. They will respond to each other among themselves in a continued conversation for a few rounds before completing each topic. When done, ask me if I’d like the conversation to continue or move to another topic.

Here are some sample questions:
* What is the nature of God?
* What aspects of your faiths are the same?
* How are your faiths different?

### Meeting of the Minds
Steve Allen produced a TV show in the late 1970s called [__The Meeting of the Minds__]( https://en.wikipedia.org/wiki/Meeting_of_Minds). It was a scripted show where four famous people from history would sit a table for a 30-minute conversation with Allen as their host/moderator. 
>Act as four personas: Galileo, Isaac Newton, Albert Einstein and Stephen Hawking. 
>
>They are sitting at a round table having a discussion. They all speak English. Hawking's speech is not incumbered by his illness. 
>
>The men of past histories know the men in their relative futures, but they don't have detailed knowledge of future discoveries. 
>
>They will have discussions about the nature of the universe as they understand it. They will be curious about what the other men know, especially the men of the future. They will inquire for more insights.
>
>Each conversation will be primed by me as a moderator. The conversation should progress in several rounds. 
>Do not start the discussion until I ask the first question.

Here are some sample questions:
* What is the nature of universe for each of you?
* Please continue among yourselves.
* What obstacles did you encounter when presenting your new ideas?
* You've each changed how we view the universe. Do you have any thoughts on how our view of the universe might change again?

## Adding New Information to the LLM
The LLM hasn’t been trained on all knowledge. Technically, it’s not all knowledge. Some LLMs are domain specific, such as those to assist with programming.
What if you want to leverage the LLM/GenAI in a domain in which it has not been trained? If your domain content is not too large, then include it prompt.
__NOTE:__ In general, do not add any personal or proprietary information you wouldn't share freely with the world.

### Email Style Options
Let’s say that you want the LLM/GenAI to provide different options for the same email you wish to send.
>I need some suggestions on an email that I need to send. I'll provide the email after <EMAIL>. I will tell you to rewrite the original email in different styles, one style at a time. Rewrite it. 
>
>EMAIL Bill, After 30 years of working for the company, I have decided that it's time for me to pursue other interests. 
>
>Therefore, I will be retiring. My last day will be in two weeks on September 30. In the interim, I will work with the team in any desired knowledge transfer.
>
>Best wishes to you and the team.
>
>John Q. Worker

Here are some style choices to try:
* Write as a formal business letter.
* Write it as a joke.
* Write it as if by Dr. Seuss.
* Write it as some who's sick and tired of work, as in "Take this job and shove it!"

Of course, you’d probably make additional updates before sending the email.

### Terms of Service Advisor
Do you read the Terms of Service? Probably not. Take a look at the [YouTube Terms of Service](https://www.youtube.com/static?template=terms).
>Act as an advisor. I will ask you questions about a Terms of Service agreement, which will be copied directly after <TERMS OF SERVICE>. Base your answers upon the content provided. 
>
>TERMS OF SERVICE
>_Copy the YouTube ToS content found on the page linked above and paste it in the prompt here._

Here are some inquiries:
* Summarize the ToS as a bullet list.
* Can I make money from posting my videos and if so how?
* How are legal disputes resolved?
* What are my rights and responsibilities?

### Too Large for the Prompt
There’s a limit to how much you can add to a prompt. I suspect the limit will increase as LLMs advance. But if the Terms of Service agreement you want to understand is too large to copy-and-paste into the prompt.

There are several options, most along the same theme:
* Split the content into smaller prompts. For example, rather than pasting your entire Terms of Service into the prompt, maybe you can seek advice section by section. The risk of this approach is that context among separated sections may be lost.
* Split the agreement into smaller sections and summarize it. Do this manually or ask the LLM/GenAI to summarize it. Then combine the summarized parts. For example, have the LLM/GenAI summarize each section, and save all the sections into one document that will hopefully fit within one prompt. Now you can seek advice on the summarized version of the agreement. However, the risk of this approach is that important information may be excluded from the summary.

## Question Refinement Pattern
Good prompts should have clarity and context, but we often forgo those in casual conversation. Our friends and family tend to know context based upon their previous experiences with us. But LLMs don’t have that knowledge.

What if I ask an LLM:
>Should I go to Harvard?

ChatGPT gave me an answer on the assumption that I was considering attending Harvard for my education. That’s probably a reasonable assumption. But what if the context of my question was actually:
* Should accept a faculty position at Harvard?
* Should I visit Harvard on my upcoming vacation to Boston?

Add a prompt for asking the LLM/GenAI to refine your question:
> From now on, when I ask a question, suggest a better version of my question.

Or even better, rather than just ask for the question have it ask you if 
> From now on, when I ask a question, suggest a better version of my question and ask me if I would like to use it.

Or:
> Whenever I ask a question, suggest a better question and ask me if I would like to use it instead.

ChatGPT still assumed that my original question was in the context of attending Harvard as a student, but it told me that before answering as if that was my actual intent.

With the prompt, it provides more context. And it seems to trigger something else. Even if I confirm that I’m interested in attending, it starts to interview me by asking me questions.

The __From now on__ and __Whenever__ phrases are a type of prompt pattern too. They set up rules for the rest of the conversation.

I don’t need to provide additional examples. Choose one of the examples above which asks the LLM questions, such as the [Constitution Scholar](#Constitutional-Scholar) and the [Religious Scholars]($Interfaith-Discussion) examples. After the original prompt, add the question refinement prompt and ask the same questions or ask your own.

# Summary
Prompt engineering provides of providing context and details for extracting information from the LLM’s relationship of words that’s focused upon that context. It’s a conversation with the LLM in natural language. Prompt engineering patterns natural language phrases that have shown to return good results.

I’ve provided several prompt examples of what I learned in the online courses. I’ll continue in the next blog with more. I don’t plan to list all that I’ve seen. I’ll focus on some of my favorites and the ones that wowed me. I’ll provide references to other prompt engineering patterns in the references.

LLM/GenAI technology is still new. I’m sure people will discover more prompt engineering patterns as LLM/GenAI technology and our experience with it advances. You may even create some of your own.

# References
I’ve only scratched the surface of Prompt Engineering Patterns. Here are references to more prompt pattern content.

See the previous blog’s [references](https://jhumelsine.github.io/2024/09/18/llm-genai.html#references) for introduction LLM/GenAI courses and several LLM platforms.

Search Google for [Prompt Engineering Patterns](https://www.google.com/search?q=prompt+engineering+patterns)

Prompt Engineering Pattern Videos and Courses:
* Google Video search for [Prompt Engineering Patterns](https://www.google.com/search?q=prompt+engineering+patterns&tbm=vid)
* Coursera [Prompt Engineering Pattern]( https://www.coursera.org/search?query=Prompt%20Engineering&sortBy=BEST_MATCH&myLearningTab=IN_PROGRESS) courses. As of this posting, this query returns over 500 results. Enroll for a price for full experience, or audit for free, which doesn’t tend to include tests or grading. Some courses may not provide an audit option.
* Coursera courses I audited, which are the source of most of my blog content:
    * [Generative AI: Prompt Engineering Basics](https://www.coursera.org/learn/generative-ai-prompt-engineering-for-everyone/home/welcome) - Provided by IBM.
    * [Prompt Engineering for ChatGPT](https://www.coursera.org/learn/prompt-engineering/home/welcome) - Provided by Vanderbilt University.

Other Prompt Engineering Pattern content:
* [Generative AI Introduction](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/overview) – Google Document
* [Introduction to prompting](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/prompts/introduction-prompt-design) – Google Document
* [Prompting strategies](https://cloud.google.com/vertex-ai/generative-ai/docs/learn/prompts/prompt-design-strategies) – Google Document
* [Prompt engineering](https://platform.openai.com/docs/guides/prompt-engineering) – OpenAI Document
* [Prompt engineering patterns](https://community.openai.com/t/prompt-engineering-patterns/121040) – OpenAI Prompt Engineering Pattern Forum
* [What Is a Prompt Pattern?](https://www.coursera.org/articles/prompt-pattern) – by Coursera staff
* [A Prompt Pattern Catalog to Enhance Prompt Engineering with ChatGPT](https://www.dre.vanderbilt.edu/~schmidt/PDF/prompt-patterns.pdf) – by Jules White, et al, White is the presenter in the Vanderbilt Coursera course
* [LLM Prompt Engineering Patterns]( https://www.linkedin.com/pulse/llm-prompt-engineering-patterns-stephen-redmond/) – by Stephen Redmond
* [A Step-by-step Guide to Prompt Engineering: Best Practices, Challenges, and Examples](https://www.lakera.ai/blog/prompt-engineering-guide) – by Mokolaj Kowalczyk
* [Prompt engineering: The process, uses, techniques, applications and best practices](https://www.leewayhertz.com/prompt-engineering/) – by Akash Takyar
* [Prompt Engineering Best Practices: Tips, Tricks, and Tools](https://www.digitalocean.com/resources/articles/prompt-engineering-best-practices) – DigitalOcean article
