---
title: You Studied Computer Science. Your Career Will Be Software Engineering.
description: An Introduction to a Software Engineering Career Path
---

# Introduction
I mentor several Computer Science undergraduates attending the university where I received my Bachelor's degree in Computer Science almost 45 years ago.

I keep getting the same questions: _How do I get an internship? How do I land my first job? What happens when I start?_
Addressing these career path questions will be the theme of my next series.
One of the entries already exists, which I wrote about a year and a half ago with [Losing Your Job Stinks](https://jhumelsine.github.io/2024/08/11/jobsearch.html).

This series focuses upon career advice.
It won't be technical.
It's based upon my own lived experiences.
Since each person's journey is unique, my advice will not be universal.
Others will have their own opinions and advice, some of which may conflict with mine.
Some of my advice may age quickly.
[Generative AI](https://jhumelsine.github.io/2024/09/18/llm-genai.html) was just coming onto the scene when I retired in 2023.

# Computer Science is not Software Engineering
_In science if you know what you are doing you should not be doing it. In engineering if you do not know what you are doing you should not be doing it._ — Richard Hamming

My degrees are in [**Computer Science**](https://en.wikipedia.org/wiki/Computer_science).
My mentees' degrees will be in **Computer Science**. But what we practice in industry is [**Software Engineering**](https://en.wikipedia.org/wiki/Software_engineering).

**Computer Science** and **Software Engineering** are related yet different.

***Computer Science is to Software Engineering as Chemistry is to Chemical Engineering***.

They have a lot in common, but the former of each pair is the science, and the latter of each pair is the application.
Even professionals blur this distinction.

**Computer Science** is how you get the computer to do what you want it to do.

**Software Engineering** is how a team gets the computer to do what they think the customer wants the computer to do in such a way that they can easily change it when the customer tells them that what it does isn't what they wanted.

In a **Computer Science** degree, you're usually expected and often required to do your own work alone.

In a **Software Engineering** career, you're usually expected to work with a team.

**Computer Science** programs tend to have a lifespan of days to weeks.
They tend to be relatively small, well defined, static, implemented completely by the student and once handed in, never revisited.

**Software Engineering** programs tend to have a lifespan of months to years to decades.
They tend to be large, a bit ill-defined, dynamic, implemented by teams, and require constant maintenance.

**Computer Science** is about technology and mathematical rigor. It focuses upon precision, correctness, and individual mastery.

**Software Engineering** is about communication with others. It's a social science. It manages coordination, change, and ambiguity.

You need a foundation in **Computer Science** to practice **Software Engineering**.
They are related yet different disciplines.
I don't know that academia necessarily conveys that distinction these days.
I think it's getting better with the introduction of **Software Engineering** courses, but I don't think academia has a full grasp on the distinction between **Computer Science** and **Software Engineering** quite yet.
Maybe I'm wrong.
I hope so.

## Computer Science Problems
In academia, Computer Science courses typically address well-defined problems of bounded scope/domain that don't tend to change over time.
These problems tend to have provable solutions.

Typical problems and topics include data structures, algorithms, discrete mathematics, programming language design, etc.
Computer Science curricula introduce students to the entire computer tech stack from logic gates at the bottom up to applications at the top.

<img src="https://media3.giphy.com/media/v1.Y2lkPTc5MGI3NjExcDlxNHN2d2g1N2xhN3NjMG1mYmRxaThtMWdpeGVtbGwxaWNmb3c1YSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/99aniB2u9OztK/giphy.gif" alt="Electrons Dancing Among Rocks" title="Image Source: https://giphy.com/gifs/circuit-99aniB2u9OztK" width = "35%" align="right" style="padding: 0 20px;">

**We make electrons dance among rocks.**
Each additional layer on the tech stack is for the benefit of humans, not the computer.
The computer is just as content to execute machine code written by a human as it is to execute machine code generated from a compiler from source code written by a human.
However, a human will almost certainly find reading a program written in a higher-level language easier than reading machine code.

My first paying job as a software developer was a summer position between my sophomore and junior years in college.
It was the closest I ever came to coding on bare metal.
I hand wrote my programs in an assembly language on paper and then converted the assembly code into machine code by hand.
I loaded machine code into the machine using a Hexadecimal keypad.

The programs weren't complex, but I needed to divide numbers with decimal results.
The chip only supported integers, and it did not have a divide operation.
It took me several days to find a divide routine for the machine and figure out how to eke out decimals via remainders from the division.
Even the most basic higher-level language would have supported float division, which would have reduced several days of assembly code development to: `rate = distance / time`.

Each new layer adds an additional layer of [Abstraction](https://jhumelsine.github.io/2024/10/30/abstraction.html) removing us further from the dancing electrons and rocks.
This separation gives us greater ability to specify **what** we want the computer to do without knowing the specific details of **how** it's going to do it.
It shields us from low level toil.

_Abstraction is the elimination of the irrelevant and the amplification of the essential._ ― Robert C. Martin

There's an additional advantage.
The tech stack is not a single column.
It's more like a brick wall.
There may be multiple options to support an abstraction layer.

The programming language in my first professional position was C.
The project debated different hardware vendors.
The eventual vendor was not an issue, because our C code would compile to object code that would execute upon any vendors' machines.
In contrast, my summer job machine code could only execute on hardware that used the same chip set as my machine.

The Computer Science curriculum explains these layers and shows how each layer interacts with the layers above and below it.
These are foundational concepts.
They lift the veil of mystery.
A computer isn't a magical black box.
We gain some understanding of what’s going on under the hood.
But knowing how the layers work is different from knowing how to build and evolve a system that spans many of them with other people.
Abstraction doesn't just make things easier; it makes large systems possible by limiting how much any one human has to understand at a time.

Even so, most courses just scratch the surface.
I used to chuckle to myself that the title of every college Computer Science text book I had through my undergraduate degree (and some in my graduate degree) often began with _An Introduction to ..._ .
When were we going to get past the introduction?

This was the case until recently.
The transitions between layers of abstraction were well understood, deterministic, and often provable.
[Large Language Models and Generative AI](https://jhumelsine.github.io/2024/09/18/llm-genai.html) (LLMs & GenAI) are changing the landscape rapidly.
They are adding an additional layer of abstraction on top of application coding.
Higher-level languages removed the toil of assembly code.
Will LLMs and GenAI remove the toil of traditional programming?
I don’t know.
This new layer of abstraction is not like previous layers of abstraction.
LLMs/GenAIs are not well understood, deterministic or provable.
But I do know that those who learn to leverage them effectively will have an advantage over those who do not.

<img src="https://upload.wikimedia.org/wikipedia/commons/4/47/River_terrapin.jpg" alt="Turtles All the Way Down" title="Image Source: https://en.wikipedia.org/wiki/File:River_terrapin.jpg" width = "25%" align="left" style="padding: 0 20px;">

There are so many layers of abstraction in the tech stack that it's sometimes referred to as [turtles all the way down](https://en.wikipedia.org/wiki/Turtles_all_the_way_down).
Most careers tend to narrow and focus upon a specific section of the tech stack.
For most of us, I suspect it tends to be at the application layer.
For others, it's lower in the stack.

Knowledge of the operating system is useful to an application developer, but it's usually not necessary.
Additional layers of abstraction, such as [Virtual Machines](https://en.wikipedia.org/wiki/Virtual_machine) and [Containers](https://en.wikipedia.org/wiki/Containerization_(computing)) make lower-level details even less relevant.

This lack of relevance doesn't restrict itself to tech stack layers.
Data Structures was a major course in the early 1980s.
If you needed a data structure, you had to build it yourself.
Now almost every major data structure is either built into the language or available via a utility library.
If not, then there's probably an open-source library that provides it.
However, the knowledge I learned in being able to build my own data structures has greatly aided me in understanding the behaviors and performance characteristics of language and library provided data structures.

Sorting is a major topic of study.
I **never** had to implement `sort()` in my 38-year career.
The only time I have ever been required to implement [Quick Sort](https://en.wikipedia.org/wiki/Quicksort) has been when it was a programming assignment for a Computer Science course.

I suspect that sorting is still a major topic in Computer Science because:
* It was a major problem to solve in its day.
* It's tradition/inertia. That is, they've always been teaching it, and they're going to continue teaching it.
* It's a well-defined problem that students can understand.
* It's a good teaching tool, since there are many approaches that solve sorting with different space and time efficiencies. It's a good model to teach algorithms, complexity analysis, etc.

Just as I have never been required to write a `sort()` program, design a data structure or provide [Big-O](https://en.wikipedia.org/wiki/Big_O_notation) analysis, the knowledge of how to do these served me well throughout my career.

Computer Science is closer to a hard science.
Its closest hard-science cousin would be mathematics.

**Don't be rattled when the types of problems you learned to solve in academia are rarely the types of problems you'll encounter in your career.** Disorientation is typical, and we all have to make the adjustment.

## Software Engineering Problems
Software Engineering addresses vaguely defined problems with fuzzier scope/domain that do tend to change over time.
These problems are too complex for proofs but tend to rely upon testing to gain confidence that they solve customer problems.
I'm specifically thinking of applications that customers request and pay for.

A Computer Science degree creates a good foundation, but a degree alone is insufficient for Software Engineering.
**The degree is not the end of your education, it's the beginning.**

The half-life for technology in the Computer Science field is about three to five years and possibly even shorter with the advent of [Generative AI](https://jhumelsine.github.io/2024/09/18/llm-genai.html).
All technology I learned in college was mostly obsolete a decade or so into my career.

The primary programming language at my university was [PL/C](https://en.wikipedia.org/wiki/PL/C), which was a Cornell derived subset of [PL/I](https://en.wikipedia.org/wiki/PL/I), which stood for _Programming Language One_.
We also had a brief foray into [Fortran](https://en.wikipedia.org/wiki/Fortran) 77.
My Programming Languages course introduced us to [SNOBOL](https://en.wikipedia.org/wiki/SNOBOL) and [APL](https://en.wikipedia.org/wiki/APL_(programming_language)), which always felt like reading Greek from right to left.
The source language for my Compiler class was [Pascal](https://en.wikipedia.org/wiki/Pascal_(programming_language)), which was the new and upcoming programming language at the time.

Though I've not seriously thought about these languages in decades, many share common properties that made it easier to learn new languages as needed.
They created a knowledge base I could build upon and adapt.

The only courses that retained their relevance through my career were not technology intensive. These were the theory and general principle courses, such as [Graph Theory](https://en.wikipedia.org/wiki/Graph_theory), [Automata Theory](https://en.wikipedia.org/wiki/Automata_theory), [Data Structures](https://en.wikipedia.org/wiki/Data_structure) and [Algorithms](https://en.wikipedia.org/wiki/Algorithm).
Since they're based on mathematical rigor, they tend to be evergreen.
While I didn't use content from these courses directly, they provided a solid foundation upon which I could continue to learn as the field advanced.

Throughout my career I had to pick up new technology, such as:
* [Object-Oriented Programming](https://en.wikipedia.org/wiki/Object-oriented_programming)
* [The Internet](https://en.wikipedia.org/wiki/Internet)
* [Cloud Computing](https://en.wikipedia.org/wiki/Cloud_computing)/[Distributed Computing](https://en.wikipedia.org/wiki/Distributed_computing)
* [Mobile Apps](https://en.wikipedia.org/wiki/Mobile_app)
* [Containers](https://en.wikipedia.org/wiki/Containerization_(computing))
* [NoSQLs](https://en.wikipedia.org/wiki/NoSQL)

But Software Engineering involves more than just changing technology.
Software Engineering is a social endeavor.
It involves process, patterns, policies, and practices such as:
* [Design Patterns](https://jhumelsine.github.io/table-of-contents#design-patterns-and-architecture), which has been a major topic in my blog
* [Automated Testing](https://jhumelsine.github.io/table-of-contents#automated-testing), which has been another major topic
* Modularity, Abstraction and a few other related concepts
* Technical Debt and Cruft
* Domain-Driven Design
* Observability
* Agile
* and more

I plan to blog about most of these topics in the future, but you don't have to wait for me.
Many of these Software Engineering topics are covered by others through blogs, videos, podcasts, etc.

Software Engineering is a soft science in many ways. It is fundamentally about working with other people to solve problems, often using computers.
Its closest soft-science cousin would be sociology, with maybe a bit of economics mixed in.

# Career Path Series
The Computer Science and Software Engineering comparison sets the stage for career path development.
Here is what I'm considering so far in the series.
This is just a working list. 
None of these are set in stone yet:
* Getting an Internship (TBD)
* Getting Your First Job (TBD)
* Onboarding (TBD)
* Your Career Journey (TBD)
* [Losing Your Job Stinks](https://jhumelsine.github.io/2024/08/11/jobsearch.html)
* Retiring (TBD)

# Summary
I have been fascinated with computers for about as long as I can remember.
I knew I wanted a career with computers when I saw [my first real program](https://jhumelsine.github.io/about#about-jim) execute.
Learning more about computers in college only reinforced my desire.

But when I started my career at 24, I started to realize that industry wasn't exactly like college.
This wasn't necessarily bad.
It meant that there was a whole new world of computers and software out there ready for me to learn about and explore.
Most new Software Engineers will take a similar journey, even if it doesn’t look exactly the same.
And that journey begins the day you realize your education didn’t end at graduation.
