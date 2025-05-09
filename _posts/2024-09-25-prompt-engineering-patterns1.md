---
title: Prompt Engineering Patterns
description: How to have a productive conversation with a Large Language Model
---

<img src="https://live.staticflickr.com/7259/7608126362_9b07f50b97_b.jpg" alt="Oracle of Delphi" title="Image Source: https://www.flickr.com/photos/gandalfsgallery/7608126362" width = "50%" align="center" style="padding-right: 20px;">


# Introduction
I wrote about __Large Language Models__ (LLMs) and __Generative AI__ (GenAI) in my previous [blog](https://jhumelsine.github.io/2024/09/18/llm-genai.html). This blog will provide suggestions on how to invoke good results from LLMs and GenAI via [prompt engineering](https://en.wikipedia.org/wiki/Prompt_engineering).

I learned most of these prompt patterns from two online courses I recently audited: [Generative AI: Prompt Engineering Basics](https://www.coursera.org/learn/generative-ai-prompt-engineering-for-everyone/home/welcome) and [Prompt Engineering for ChatGPT](https://www.coursera.org/learn/prompt-engineering/home/welcome). There are more resources available in the [References](#references) at the bottom of this blog entry.

I will describe Prompt Engineering Patterns with a few examples. I will not provide my GenAI generated results for the following reasons:
* They would consume too much space if copied here.
* Watching GenAI generate responses in real time will have a greater impact than reading my responses.
* Your responses and my responses will not be the same. GenAI results always vary, even with the same prompt.

I recommend having an LLM/GenAI platform open as you read this. [ChatGPT](https://chatgpt.com/), provided by [OpenAI](https://openai.com/), offers paid subscriber and free ChatGPT options. I use the free version of ChatGPT, which requires a login. The login can be created on their platform, or you can authenticate via other platform accounts, such as Google. While I have confirmed my prompt examples in ChatGPT, I think most will work in other [LLM/GenAI platforms](https://jhumelsine.github.io/2024/09/18/llm-genai.html#references) as well.

Copy-and-paste my prompt examples into your chosen LLM/GenAI platform as you proceed through this blog. And experiment as you follow along.

# Prompt Engineering Basics
LLMs model the relationships among words, technically [tokens](https://en.wikipedia.org/wiki/Large_language_model#Tokenization), based upon billions of examples from the internet.

When text is provided in an LLM’s prompt, GenAI uses the provided text to generate [the next most likely word](https://jhumelsine.github.io/2024/09/18/llm-genai.html#the-next-most-likely-word) based upon the relationship of words in the model. It adds this generated word to the prompt text and repeats the process to generate the next most likely word. One doesn’t need to know the algorithmic details of how it does this. I don’t quite understand it myself. All one really needs to know is that the LLM is generating a series of the next most likely words using the initial text prompt as the seed.

Enter a set of random words as a prompt, and GenAI will balk with a response indicating that it can’t do too much with random words. But give it something with context, such as a request or a question, and it will build a response.

# Good Prompt Basics
The initial prompt is a conversation starter with the LLM/GenAI. Better prompts will elicit better responses from the LLM/GenAI. Prompting is communicating with the LLM. Good prompting is about good communication. A good prompt will include the following (mostly from: [Generative AI: Prompt Engineering Basics](https://www.coursera.org/learn/generative-ai-prompt-engineering-for-everyone/home/welcome)):
* __Instructions:__ Provides guidelines regarding the task. That is, what do you want GenAI to do.
* __Context:__ Lists details useful for generating relevant content. That is, narrow the scope of the conversation to focus upon a specific topic.
* __Input Data:__ Includes additional information provided by you in the prompt. For example, you may want it to critique something you’re written, such as an email.
* __Output Indicator:__ Describes the structure of the response. For example, do you want text, an outline, a table, etc.?

Copy-and-paste this as one prompt, which should generate the table:
>List the checks and balances of the three branches of the United States Federal government. 
>
>List them in a 3 by 3 grid with each branch listed as the header for each row and column in the grid. 
>
>The diagonals will be the same branch in the row and column headers, so place "N/A" in those cells.

Here is a more conversational interaction to reach the same outcome. Start a new session and start with this prompt:
>What are the checks and balances of the three branches of the United States Federal government?

This returned a text-based response for me. If a tabular response were desired, then continue with this additional prompt:
>Put that in a table.

I thought the second, more natural conversational prompts gave me better results than the first one. But rather than place __N/A__ on the diagonals, it lists powers associated with that branch on the diagonal, which felt a bit confusing to me. So, I added this third request to clean that up:
> Put "N/A" in the diagonal cells which compare a branch to itself, such as Legislative/Legislative, Executive/Executive and Judicial/Judicial.

This cleaned up the table.

# Prompt Engineering Pattern Introduction
I’m a huge fan of [design pattern](https://jhumelsine.github.io/2023/08/29/toolbox.html). Traditional design patterns apply to an object-oriented domain. Prompt Engineering Patterns apply to an LLM/GenAI domain. They are solution approaches to common problems when interacting with LLMs/GenAIs. Unlike traditional design patterns, which require some technical expertise, prompt engineering patterns are not technical. Each pattern is typically a phrase or two that invokes a certain type of response from the LLM/GenAI. Multiple patterns may be used in the same prompt or session.

Prompt Engineering Patterns are coalescing to some degree, but the same technique may have different names depending upon the source. There are also more patterns waiting to be discovered.

# Best Practices for Prompt Creation
The [Generative AI: Prompt Engineering Basics](https://www.coursera.org/learn/generative-ai-prompt-engineering-for-everyone/home/welcome) course continued with more suggestions for good prompts:
* __Clarity:__
    * Use clear concise language
    * Avoid jargon and complex terms
    * Provide explicit instructions
* __Context:__
    * Establish the context
    * Include relevant information
* __Precision:__
    * Be specific
    * Use examples
* __Role-play/Persona pattern:__
    * Assume a persona
    * Provide context for role-play

In essence, if your prompts would make sense to a person, then they will probably make sense to the LLM/GenAI.

# Prompt Engineering Patterns
The main source for many of these Prompt Patterns is from the [Prompt Engineering for ChatGPT](https://www.coursera.org/learn/prompt-engineering/home/welcome) course.

## Persona Pattern
In the real world, if you wanted medical advice, you’d ask a doctor. If you wanted financial advice, you’d ask a financial planner. If you wanted tax advice, you’d ask a tax accountant.

There's a contextual domain for each of these professions. The __Persona Pattern__ will allow us to tap into those domains with a few words or phrases

The Persona Pattern allows the LLM/GenAI to respond as if it were that persona, which could be:
* A role, such as doctor, but __DO NOT__ take any of its medical advice seriously — always confirm with an actual physician
* A real person, living or dead, if there’s enough public information about that person
* A fictional character
* An inanimate object

To activate the Persona Pattern use phrases such as:
* You are a _persona_ …
* Act as a _persona_ …
* Provide a response from a _persona_ perspective ...

Replace _persona_ in the above with something more concrete.

Here’s a persona prompt with more instructions within it. Replace _specific_ with a type of expert:
> You will act as a _specific_ expert. Your objective is to engage in a comprehensible _specific_ session with me. Begin by asking a series of detailed questions, one at a time, to gather all essential information required to craft the most tailored and memorable <thing> based upon my specific preferences, interests, etc.

## Examples to try
Here are a few examples to try. Copy-and-paste them into your LLM/GenAI platform.

### Career Councilor 
>Act as a career coach. I'm looking for new career opportunities. Help me come up with some options. Interview me to gain an assessment of what jobs I should consider pursuing.
>
>Begin by asking me general employment questions to learn more about me. Ask about my educational background, my previous employment experience, my skillset, the types of work problems that I like to solve. Also ask anything else that provides more insights into the type of jobs for which I would be a good match.
>
>Ask me one question at a time. Ask follow-up questions based upon my specific answers but make sure to return to the high-level questions as well.
>Once you have acquired enough information about me via my answers, then provide a list of careers for which I would be a good match.

What I like about this prompt is that it exposes you to considerations you may not have thought of yourself. It's a form of idea exploration. If there's a good idea, continue to pursue it, either in the real world or via the LLM. If it's not so much a good idea, then ignore it.

Create a new session and prove the same prompt. Give it slightly different answers and see what else it may produce.

### Constitutional Scholar
> Act as a United States Constitutional scholar. Answer my questions about the US Constitution including the Amendments. Provide the sections of the Constitution and Amendments as references to your answers. Include the text as well as the section where that text resides.

Here are some sample questions:
* How long is the term of the US President?
* What can you tell me about patent law?
* What's the emoluments clause?
* What are checks and balances and how do they work?
* What can Donald Trump teach us about the Constitution?

### United Methodist Scholar
I'm a member of a United Methodist church. The policies and procedures of the United Methodist Church (UMC) denomination is documented in an almost 900-page tome called [The Book of Discipline](https://www.cokesbury.com/book-of-discipline-book-of-resolutions-free-versions) (_BoD_). Reading it is about as easy and captivating as reading the US Department of Agriculture's regulations for [Lamb and Mutton](https://www.ams.usda.gov/sites/default/files/media/Lamb_IMPS200Series_2022.pdf).

It's difficult for laity to navigate the _BoD_ to find answers to their inquiries. I suspect it's difficult for many clergy too.

I asked ChatGPT if it had been trained on the _BoD_. It said that it had. I asked it a few questions and requested the _BoD_ sections it referenced in its responses. I checked them out with the actual sections in the _BoD_, and they seemed correct.

ChatGPT is now my guide into the _Book of Discipline_.

Here is my initial prompt:
>You are a scholar in the "Book of Discipline" from the United Methodist Church.
>
>When you respond to my requests, only reference the Book of Discipline in your responses.
>
>Provide references to the Book of Discipline in your responses, including both the section referenced and text in that section.

Here are a few example inquiries, such as:
* Describe the organizational structure of the UMC in the same way one might describe the structure of the Federal US government to a civics classroom.
* What groups and committees reside within a local church? What are their roles and responsibilities? Who are members? How do they become members?

ChatGPT wasn't aways 100% reliable in providing references, but if I followed up with: __Include the Book of Discipline references and text__, then it added them. 

__NOTE:__ ChatGPT's _BoD_ text sometimes varied from the actual _BoD_, and it sometimes confabulated policy, but the reference numbers were always accurate to double check.

### Interfaith Discussion
LLM/GenAI can take on multiple personas. While this next example starts off sounding like a bad bar joke, it does yield some interesting results.
> For this session you will take on 4 personas. They will be a Jewish Rabbi, a Catholic Priest, a Protestant Minister and a Muslim Imam. Each will respond from their own point of view. Each will be polite. Each will cite scriptures to reinforce their response. They will respond to each other in a continued conversation for a few rounds before completing each topic. When done, ask me if I’d like the conversation to continue or move to another topic.

Here are some sample questions:
* What is the nature of God?
* What aspects of your faiths are the same?
* How are your faiths different?

### Meeting of the Minds
Steve Allen produced a TV show in the late 1970s called [__The Meeting of the Minds__]( https://en.wikipedia.org/wiki/Meeting_of_Minds). It was a scripted show where four famous people from history, portrayed by actors, would sit a table for a 30-minute conversation with Allen as their host/moderator. 
>Act as four personas: Galileo Galilei, Isaac Newton, Albert Einstein and Stephen Hawking. 
>
>They are sitting at a round table having a discussion. They all speak English. Hawking's speech is not incumbered by his illness. 
>
>The men of past histories know the men in their relative futures, but they don't have detailed knowledge of future discoveries. 
>
>They will have discussions about the nature of the universe as they understand it. They will be curious about what the other men know, especially the men of their future. They will inquire for more insights.
>
>Each conversation will be primed by me as a moderator. The conversation should progress in several rounds. After several rounds ask me if I want the discussion to continue or present a new topic.
>
>Do not start the discussion until I have introduced the first topic.

Here are some sample conversation topics:
* What is the nature of the universe for each of you?
* What obstacles did you encounter when presenting your new ideas?
* You've each changed how we view the universe. Do you have any thoughts on how our view of the universe might change again?

## Adding New Information to the LLM
The LLM isn't trained with all possible knowledge. Some LLMs are domain specific, such as those that assist with programming.

What if you want to leverage the LLM/GenAI in a domain in which it has not been trained? If your domain content is not too large, then include it in the prompt.

__NOTE:__ In general, do not add any personal or proprietary information in the prompt that you wouldn't share freely with the world.

### Email Style Options
Let’s say that you want the LLM/GenAI to provide different options for an email you wish to send.
>I need some suggestions on an email that I need to send. I'll provide the email after \<EMAIL\>. I will tell you to rewrite the original email in different styles, one style at a time. Rewrite it. 
>
>EMAIL
>
>Bill,
>
>After 30 years of working for the company, I have decided that it's time for me to pursue other interests. 
>
>Therefore, I will be retiring. My last day will be in two weeks on September 30. In the interim, I will work with the team in any desired knowledge transfer.
>
>Best wishes to you and the team.
>
>John Q. Worker

Here are some style-choice follow-up prompts:
* Write it as a formal business letter.
* Write it as a joke.
* Write it as if by Dr. Seuss.
* Write it as some who's sick and tired of work, as in "Take this job and shove it!"

You’d probably want to make additional updates by hand before sending the email.

### Terms of Service Advisor
Do you read the Terms of Service before clicking on __accept__? Probably not. Take a look at the [YouTube Terms of Service](https://www.youtube.com/static?template=terms). Do you understand it?
>I will ask you questions about a Terms of Service agreement, which will be copied directly after \<TERMS OF SERVICE\>. Base your answers upon the content provided. 
>
>TERMS OF SERVICE
>
>_Copy the YouTube ToS content found on the page linked above and paste it in the prompt here._

Here are some prompt inquiries:
* Summarize the ToS as a bullet list.
* Can I make money from posting my videos and if so how?
* How are legal disputes resolved?
* What are my rights and responsibilities?

Consider a slightly different scenario. You work in customer support at your company. There are many policies upon how you can assist customers, especially for refunds. There are different policies based upon the amount of the refund, the number of refunds that a customer has requested, etc. While the policies are documented, they read much like Terms of Service agreements.

What if your company's customer support policies were part of its private LLM? You'd be able to type in each customer situation, and it could tell you what options you have for each customer situation that's consistent with customer support policies.

### Too Large for the Prompt
There’s a limit to how much text you can add to a prompt. I suspect the limit will increase as LLMs advance. But what if the Terms of Service agreement, or any other content, you want analysis from the LLM is too large to copy-and-paste into the prompt?

There are several options, mostly along the same theme:
* Split the content into smaller prompts. For example, rather than pasting your entire Terms of Service agreement into the prompt, copy and paste one section that is small enough. Seek advice section by section. The risk of this approach is that context split among separated sections may be lost.
* Split the content into smaller sections and summarize each. Do this manually or ask the LLM/GenAI to summarize it. Then combine the summarized parts into one prompt. For example, have the LLM/GenAI summarize each section of the Terms and Service agreement, and save all the sections into one document that will hopefully fit within one prompt. Now you can seek advice on the summarized version of the agreement. The risk of this approach is that important information may be excluded from the summary.

#### ADDENDUM added on April 18, 2025
Since the original publication last year in September, I've found other techniques to help manage a large prompt in ChatGPT. I don't know how long ChatGPT has been supporting these. It's possible this technique has been there for months. I also don't know of the limitations, but so far I haven't encountered any.

You don't have to put all content into the prompt. You can reference it, and ChatGPT will use it. Here are somethings I've done.

##### Provide the URL to additional information
In the Terms of Service example above, I copied the text for the ToS of Youtube into the prompt. If there were too much text for the prompt, I would not be able to get a complete analysis of the ToS, even with the options I listed above. However, if the ToS has its own page, then we can change the prompt to this:
>I will ask you questions about a Terms of Service agreement, which resides in: https://www.youtube.com/static?template=terms
>
>Base your answers upon the content provided. 

This is much more convenient and not as likely to run into prompt space restrictions. I also tried this technique with the online PDF version of a several hundred page book I had previously read. I provided the link and asked ChatGPT to take on the persona of the author. I asked ChatGPT a few things about the book, and it responded as expected.

__NOTE:__ I didn't realize it at the time, but I had used this technique in my subsequent blog entry in [Hexagonal Architecture Q&A](https://jhumelsine.github.io/2024/10/18/prompt-engineering-patterns2.html#hexagonal-architecture-qa) where I prompted ChatGPT with URLs to my Hexagonal Architecture blog entries and had a conversation based upon that content.

##### Drag-and-Drop Files
I tried the URL technique with some of my Google Drive documents with mixed results. In one scenario, ChatGPT could reference my Google Doc. In another ChatCPT refused to. It told me to make the reference publically viewable, which I did, and then it said it still wasn't able to access files on a Google Drive. The URL that worked was a Google Document and the URL that did not work was a Slideshow. Maybe that was the distinction.

However, there's another option. I was able to drag-and-drop my original Powerpoint file into the ChatGPT, and I was able to converse with ChatGPT about content on the PPT slides. I only tried this with the PPT file, but suspect it will work for almost any type of file.

Therefore, if you really want analysis of something large, and it doesn't exist in a URL form that ChatGPT will accept, then try this. Copy-and-paste the content from its original form into a Word document, and then drag-and-drop the word document into ChatGPT. It's a bit manual, but it may work.

## Question Refinement Pattern
Good prompts should contain clarity and context. We often forgo those in casual conversation. Our friends and family tend to know context based upon their previous experiences with us. LLMs don’t have that knowledge.

What if I ask an LLM:
>Should I go to Harvard?

ChatGPT gave me an answer based upon the assumption that I was considering attending Harvard for my education. That’s probably a reasonable assumption. But what if the context of my question were actually:
* Should accept a faculty position at Harvard?
* Should I visit Harvard on my upcoming vacation to Boston?

Add a prompt requesting that the LLM/GenAI refine your question:
> From now on, when I ask a question, suggest a better version of my question.

Or even better, rather than just ask for a refined question, have it ask the refined question for you:
> From now on, when I ask a question, suggest a better version of my question and ask me if I would like to use it.

Or:
> Whenever I ask a question, suggest a better question and ask me if I would like to use it instead.

ChatGPT still assumed that my original Harvard question was in the context of attending as a student, but it told me that was its assumption before answering.

The __Question Refinement Prompt__ seems to trigger something else. Even if I confirm that I’m interested in attending, it starts to interview me by asking me follow up questions.

The __From now on__ and __Whenever__ phrases are a type of prompt pattern too. They set up rules for the rest of the conversation.

I won't provide specific examples for the Question Refinement Prompt, since it's a refinement to previous prompt patterns. Choose one of the question related examples above, such as [Constitution Scholar](#constitutional-scholar) or [Religious Scholars](#interfaith-discussion). After the original prompt, add the Question Refinement Prompt and ask the same questions or ask your own.

# Summary
Prompt engineering provides context and details for extracting information from the LLM’s relationship of words that’s focused upon that context. It’s a conversation with the LLM using natural language. Prompt engineering patterns are natural language phrases that have been shown to produce good results.

I have provided several prompt examples of what I learned in the online courses. I’ll continue with more in the next blog. I don’t plan to list all that I’ve seen. I’ll focus on some of my favorites and the ones that wowed me. I’ll provide references to other prompt engineering patterns in the references.

LLM/GenAI technology is still new. I’m sure people will discover more prompt engineering patterns as LLM/GenAI technology and our experience with it advances. You may even create some of your own.

And for kicks, copy this blog into ChatGPT. Ask it for a summary. Ask it what's good and bad about the blog. I liked the good. I thought the bad was a bit harsh. LOL.

# References
I’ve only scratched the surface of Prompt Engineering Patterns. Here are references to more prompt pattern content.

See the previous blog’s [references](https://jhumelsine.github.io/2024/09/18/llm-genai.html#references) for introduction LLM/GenAI courses and several LLM platforms.

Google Searches:
* [Prompt Engineering Patterns](https://www.google.com/search?q=prompt+engineering+patterns)
* [Prompt Engineering Pattern Videos](https://www.google.com/search?q=prompt+engineering+patterns&tbm=vid)

Prompt Engineering Pattern Courses:
* Coursera [Prompt Engineering Pattern]( https://www.coursera.org/search?query=Prompt%20Engineering&sortBy=BEST_MATCH&myLearningTab=IN_PROGRESS) courses. As of this posting, this query has returned over 500 courses. Enroll for a price for the full experience, or audit for free, which doesn’t include tests or grading. Some courses may not provide an audit option.
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
* [A Prompt Pattern Catalog to Enhance Prompt Engineering with ChatGPT](https://www.dre.vanderbilt.edu/~schmidt/PDF/prompt-patterns.pdf) – by Jules White, et al. White is the presenter in the Vanderbilt Coursera course
* [Unlocking ChatGPT’s Potential with “Scope It, Shape It, Size It”](https://jdmeier.com/3s-for-chatgpt/) - by J.D. Meier
* [33 Rules to Better ChatGPT Prompt Engineering](https://www.ssw.com.au/rules/rules-to-better-chatgpt-prompt-engineering/) - by [SSW Rules](https://www.ssw.com.au/rules/)
* [Prompt Engineering Guide](https://www.promptingguide.ai/) - by [Democratizing Artificial Intelligence Research, Education, and Technologies](https://github.com/dair-ai)
* [Prompt Patterns: What They Are and 16 You Should Know](https://www.prompthub.us/blog/prompt-patterns-what-they-are-and-16-you-should-know) – by PromptHub
* [LLM Prompt Engineering Patterns]( https://www.linkedin.com/pulse/llm-prompt-engineering-patterns-stephen-redmond/) – by Stephen Redmond
* [A Step-by-step Guide to Prompt Engineering: Best Practices, Challenges, and Examples](https://www.lakera.ai/blog/prompt-engineering-guide) – by Mokolaj Kowalczyk
* [Prompt engineering: The process, uses, techniques, applications and best practices](https://www.leewayhertz.com/prompt-engineering/) – by Akash Takyar
* [Prompt Engineering Best Practices: Tips, Tricks, and Tools](https://www.digitalocean.com/resources/articles/prompt-engineering-best-practices) – DigitalOcean article
