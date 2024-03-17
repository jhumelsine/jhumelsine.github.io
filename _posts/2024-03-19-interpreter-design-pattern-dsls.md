---
title: Interpreter Design Pattern – Domain-Specific Languages
description: Domain-Specific Languages solve domain-specific problems.
unlisted: true
---

<img src="https://www.worldhistory.org/img/r/p/1500x1500/1280.jpg" alt="Fragment of Euclid's Elements" title="Image Source: https://www.worldhistory.org/image/1280/fragment-of-euclids-elements/" width = "70%" align="center" style="padding-right: 35px;">

# Introduction
This blog expands upon Domain-Specific Languages (DSLs), which I introduced in [Interpreter Design Pattern – Domain-Specific Language Primer](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html#domain-specific-language-primer).

The Interpreter Design Pattern feels like it was tailor made for DSLs. Before I can show this, I need to do a deeper dive into DSLs, which will be the topic for the remainder of this blog.

If you already feel confident about Domain-Specific Languages, then skim this blog or skip it completely. 

__Here's a summary of the DSL concepts presented here:__
* __The Gang of Four (GoF) don't mention Domain or DSLs in their Interpreter Design Pattern documentation.__
* __I suspect they omitted it because Domain and DSLs weren't in the common vernacular when they published their book.__
* __DSLs are common. Most developers use them frequently and may not even realize it. Common DSLs are: HTML, CCS, SQL, etc.__
* __Your project might involve a Domain for which a DSL could be useful.__
* __DSLs can be more efficient at addressing Domain specific problems than General-Purpose Languages.__
* __DSLs allows the same code base to accommodate different customer configuration scenarios within the Domain.__
* __DSLs might allow for customer/user self-service.__
* __When creating a DSL, you take on all responsibility. There will be no external DSL support such as Google search results, Stack Overflow posts, etc.__

## What the Gang of Four said about Domain Specific Languages

The GoF said nothing about __Domans__ or __Domain-Specific Languages__, but they did use other similar terms. I suspect that the GoF didn’t mention __Domain__ or __Domain-Specific Languages__, since I don’t think either was a common term at the time of their publication. I can’t find many DSL references before 1995. Eric Evans’ book, __Domain-Driven Design__, which popularized __Domain__, wasn’t published until 2003.

But here's what they did say about Interpreter, even if they didn't mention __Domain__ or __Domain-Specific Languages__.

The GoF define the intent of Interpreter as:
> Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language.

<img src="https://images.rawpixel.com/image_800/czNmcy1wcml2YXRlL3Jhd3BpeGVsX2ltYWdlcy93ZWJzaXRlX2NvbnRlbnQvbHIvcHgxNDAzMjA2LWltYWdlLWt3dncwNnhqLmpwZw.jpg" alt="Head Scratching" title="Image Source: https://www.rawpixel.com/image/5911853/image-public-domain-red-free" width = "30%" align="right" style="padding-right: 35px;">

Well, that’s a mostly a circular definition that really doesn’t help me understand what they mean by  _language_, _grammar_, or _interpreter_ or the pattern’s actual __intent__.

Their motivation section continues with:
> If a particular kind of problem occurs often enough, then it might be worthwhile to express instances of the problem as sentences in a simple language. Then you can build an interpreter that solves the problem by interpreting these sentences.
>
> The Interpreter pattern describes how to define a grammar for simple languages, represent sentences in the language, and interpret these sentences.

Adding _sentences_ to the motivation really didn’t help me understand the __motivation__ or the __intent__. And I would argue that they really don’t describe how to define a _grammar_ either.

## Domain and Domain-Specific Languages
[SourceMaking's Interpreter](https://sourcemaking.com/design_patterns/interpreter) description is a bit better, and they do mention __Domain__:
> Map a domain to a language, the language to a grammar, and the grammar to a hierarchical object-oriented design.

The above is a nice summary of the process but it only makes sense once you understand the process. Then they add this:
> The Interpreter pattern discusses defining a domain language (i.e. problem characterization) as a simple language grammar, representing domain rules as language sentences, and interpreting these sentences to solve the problem. The pattern uses a class to represent each grammar rule. And since grammars are usually hierarchical in structure, an inheritance hierarchy of rule classes maps nicely.

They added ___domain___, which I think is a critical concept for understanding Interpreter. Domain is the business environment of an industry. eBay’s domain is auctions. Amazon’s domain is selling books, or at least it started that way. Facebook, LinkedIn and Twitter/X’s domains are social networks.

Domain is an ecosystem. Domain is an economy. Domain is a mathematical system. Domain is a often closed system. A domain is a set of rules that define a bounded conceptual space specifying what can and cannot happen to the domain elements within that bounded space. A Domain-Specific Language is a representation of those rules.

DSLs are domain specific where the domain is embedded in the constructs of the language itself. A domain-based language is narrowly scoped since it’s designed to support a specific domain. A problem scenario within a domain can be expressed or modeled in a DSL based specification. Then a DSL Interpreter can compute/execute/interpret the specification to produce a result.

The language itself should scream its domain. Problem expression in a DSL should be more natural and straightforward than in a General-Purpose Language (GPL), such as Java, C++, Python or Ruby. It’s difficult to stray beyond domain boundaries since the DSL won’t allow it. A DSL will tend to be smaller and more straightforward than a GPL.

Since a DSL reflects the domain, it may not look like a traditional programming language. It might look like a specification language. It might have syntax constructs not seen in GPLs. Or it could look very similar to a traditional programming language too. It all depends upon the nature of the domain.

A GPL can support domains too, but that is not its primary intent. A GSL can support any domain. A project can set coding standards that mirror domain constructs, but unless there’s effort to maintain a consistent ubiquitous language by the members of a project, a GSL based implementation will tend to project language constructs first and domain second. With a DSL, the domain is embedded within the language. The DSL __is__ the ubiquitous language.

## Do You Need a DSL?
Though I shout the praises of DSLs from the mountain tops, do you really need one?

When you design your own programming language, even a DSL, you are going out on a limb. You take on all responsibility for features, maintenance, documentation, training, and support for anyone who uses your DSL. I have more on this near the end of the blog.

A DSL can be useful in the right situation, but make sure you are in the right situation. Is the domain flexible? By that, do you need to support many scenarios with a domain, such as customer-specific configurations or rapidly changing behavior requests, such as regulatory policies, etc.? See: [Use Cases for Composability Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html#use-cases-for-composability-design-patterns).

This type of domain flexibility is not the same as config values, feature flags, or branching logic, which tends to add complexity to the implementation. This flexibility resides in DSL specifications, each of which are transformed into unique and individual [composite object trees](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) from which the bespoke behaviors emerge. This is the separation of [Computation and Coordination](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html#computation-and-coordination).

<img src="https://c.pxhere.com/images/32/a9/e1864c52b80e38516c9858588672-1599519.jpg!d" alt="Tax Forms" title="Image Source: https://pxhere.com/en/photo/1599519" width = "40%" align="right" style="padding-right: 35px;">

Consider tax preparation software that implements tax regulations. Regulations vary from jurisdiction to jurisdiction. Regulation specifics cannot be negotiated. Regulations change regularly. Regulation implementation must be complete. Delivery dates cannot slip since tax deadlines do not slip.

A tax regulation DSL would help maneuver the regulation minefields better than a GPL. See: [Designing a DSL for accounting: use a DSL to describe taxes, pension contributions, and general financial calculations](https://tomassetti.me/financial-accounting-dsl/).

# Domain-Specification Languages
Most if not all industries have domains, and some may even have DSLs for those domains. Some industries with DSLs include systems engineering, healthcare, finance, insurance, and payroll ([source](https://tomassetti.me/domain-specific-languages/)).

Here are some examples of common DSLs found in the wild as more concrete examples.

The following are DSLs primarily in software engineering domains.

## HTML
HTML is a DSL. Its domain is web layout. HTML is a language that specifies data to be displayed on a web browser. HTML includes the data and formatting instructions. It’s up to the browser to render the content correctly on the screen.

## CSS
CSS is a DSL. Its domain is style specification for pages rendered on web browsers.

## SQL
SQL is a DSL where its domain is relational database tables. SQL allows users to manipulate tables of data. SQL feels more like a programming language than HTML or CSS.

## Shell Languages
While most shell languages are programming languages, they’re on the border between DSLs and GPLs. I’m going to lean a bit toward the DSLs here, because their domain is mostly with an operating system, such as Unix/Linux, and its directories and files.

The [Unix Philosophy](https://en.wikipedia.org/wiki/Unix_philosophy) provides a cohort of single-purpose commands, usually/originally written in C. These commands usually focus upon a single aspect of Unix, such as:
* Listing the files in a directory
* Returning disk usage
* Providing a regular expression filter
* Counting lines, words, and characters
* And many more

The standard output of these commands can be piped as standard input to other commands making it easy to construct more sophisticated behaviors with a little piping glue. It’s simple for users to build their own commands from these basics.

## AWK
AWK is a programming language, but its main intent is domain specific. Its domain is its ability to process standard input in a Unix environment. It’s useful when you need more processing power than traditional shells give you. [AWK](https://en.wikipedia.org/wiki/AWK) is a great little language that more developers should know about.

## JSON
JSON is a DSL where its domain is the ability to represent data in hierarchical structures while maintaining a human-readable form.

## And Many More
[The complete guide to (external) Domain Specific Languages](https://tomassetti.me/domain-specific-languages/) lists 19 external DSLs, some of which I mentioned here as well.

# My Domain-Specific Language Experiences
Even with this substantial list of DSL examples, opportunities for DSLs and Interpreter implementations are usually few and far between. Interpreter is not an [Essential Design Pattern](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html). It’s a more narrowly focused pattern that addresses more narrowly focused situations.

## I Implemented It Once
I have designed and implemented the Interpreter design pattern only once in my career. We couldn’t believe how well it worked. Our DSL was simple, but it was highly effective for our needs. And even then, I sort of stumbled into the DSL backwards. I will provide more details in a subsequent blog.

## I Got Shot Down When Suggesting DSLs
After the above DSL experience, I moved to another company, and I was eager to work on another DSL. I found a few areas that I thought could be DSL candidates.

A new software engineering executive joined the company a few months after I started. He met with each of the teams individually to get to know the members of his department.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fb/Hammer-1629587.jpg/1200px-Hammer-1629587.jpg" alt="Hammer and Nails" title="Image Source: https://commons.wikimedia.org/wiki/File:Hammer-1629587.jpg" width = "40%" align="right" style="padding-right: 35px;">

I pitched the DSL idea to him, and he shot my suggestion down faster than the Red Barron in an aerial dogfight with a pilot fresh out of flight school. I never mentioned DSLs to him again. 

_The nail that sticks out will get hammered down. – Japanese Proverb_.

I advocated DSLs to a few other managers, but I never got traction with them either.

Years later, I’d sit in company meetings as he and the other C-suite executives bemoaned how much customer support was required to onboard new customers and maintain existing ones. “Our product needs to be more self-service for our customers,” they decried. I'd sit there, resist the urge to roll my eyes, and think to myself, "Had you given DSLs even modest consideration, you might have had some customer self-service features by now."

I understand my executive’s reluctance with DSLs, which I’ll address near the end of this blog, but I think he was throwing out the baby with the bath water.

## Close But No Cigar
I moved to a different group at the same company. The team had been part of an acquisition a few years previously. One of their core features supported customer bespoke _workflows_. DSL concepts were hinted in the implementation. They had the germ of the right idea, but the design and implementation were a bit off.

I suspect that they needed the workflow behavior quickly since it was a core feature to their business model in their early days. Being a startup, they got something working, even if it wasn’t the cleanest design or implementation. They probably didn’t have the bandwidth to refactor it later. Even if they had, I suspect they didn’t know about the Interpreter design pattern. It is one of the [last design patterns](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) developers learn and understand. I was most of the way through my career before I understood Interpreter.

<img src="https://c.pxhere.com/photos/cc/97/mark_marker_hand_leave_production_planning_control_organizational_structure_work_process-774947.jpg!d" alt="Workflow" title="Image Source: https://pxhere.com/en/photo/774947" width = "40%" align="right" style="padding-right: 35px;">

Workflows were constructed and maintained via a GUI. The GUI user experience looked like a flowchart displaying labeled rectangles connected via unlabeled lines. Some rectangles were dead ends floating in space even if they weren’t actual dead ends. The graphical rendering code had issues drawing some of the connecting lines.

Rectangles tended to bundle too many behaviors. You couldn’t see behavior details unless you opened a rectangle, which would block your view of the overall workflow’s flowchart. It was like working on a jigsaw puzzle where the photo on the box cover was a bit obscured. You could only take one piece out of the box at a time to examine it and then without the ability to view the box cover photo.

The workflow elements were stored as database records, which was just as awkward to view as the GUI if not more awkward. I never saw the big picture of a workflow other than what I could construct in my mind from the piece parts. Fortunately, most workflows weren’t very large, but some were challenging to comprehend.

The GUI wasn’t suitable for customer self-service. Our onboarding/support personnel and often engineering staff had to define and maintain each customer’s workflow. Many customers had multiple workflows.

Even our staff struggled with the GUI to the point that sometimes customer activation dates were delayed because we couldn’t get their workflows quite right. They would eventually work, but it was a brittle process.

I sort of see our executive’s DSL concerns in hindsight now. Although the workflow design really didn’t feature a DSL as best I could tell.

# Bespoke Domain-Specific Languages
Your application may feature a Domain and could benefit from a Domain-Specific Language that’s unique to your business. But the DSL may not yet exist. You could design your own and implement it with the Interpreter Design Pattern. But do not take this on lightly. Consider the implications.

## Who Uses the DSL?
Who is going to construct the DSL specifications? I can think of several types of users who could write DSL specifications.

### Developers
The DSL may be for internal use only. The developers who defined DSL may be the only people who construct the DSL specifications. For example, a DSL might define different types of configuration specifications that are only a development concern.

My DSL fell into this category.

### Customer Onboarding/Support Team
This expands the DSL user base beyond developers, but it still keeps DSL users within the company. The DSL may be straightforward enough for onboarding and customer support teams to create and maintain. The DSL may embrace the domain without having too many traditional coding elements to it. Onboarding and customer support domain experts, with enough training and support, may be able to create and maintain DSL specifications for their customers.

This might be the sweet spot for many situations. While Customer/User self-service might be desirable, this may not be practical. Some DSLs may be too sophisticated for customer self-service. Or there may be security or safety concerns. Exposing the DSL to customers might allow the customer to do some real damage to themselves. DSLs allow you to do anything within the domain even if that’s something that’s not in your best interests. Remember that SQL allows you to [drop tables](https://learn.microsoft.com/en-us/sql/t-sql/statements/drop-table-transact-sql), even those with critical data.

The _workflow_ example above fell into this category, even if workflows weren’t technically a DSL.

### Customers and Users
Customer and User self-service provides the greatest flexibility. Customers and users can design exactly what they want and need. There’s no delay in waiting for onboard or customer support staff to understand customer needs and attempt to specify them correctly in the DSL. Odds are, the first drafts won’t be correct, so several iterations may be required.

However, this also introduces the ability for the customer or user to self-service mistakes of their own doing. Self-service DSLs should be narrow in scope, focused, easy to understand and use, and nearly impossible for the customer/user to shoot themselves in the foot. As an example, the [Specification Use Case](https://jhumelsine.github.io/2024/03/07/specification-design-pattern-use-case.html) allows users to design their own Smart Playlists. Smart Playlist specifications are straightforward to define, and if a user makes a mistake, there’s no real damage. The worst that can happen is too many tracks in a smart playlist or not enough.

## Tradeoffs
DSLs are a double-edged sword. They provide a lot of power, but that comes with responsibility.

### Pros
It can be a blast designing a DSL and seeing it come to life as you implement it. I’m not advocating creating a DSL for this reason alone, but if find yourself implementing a DSL, it may be some of the most fun and interesting code you’ll work on.

A DSL usually addresses problems in the domain with less effort than a GPL.

It’s one of the best ways I know of for a single codebase to provide bespoke features to customers and users even with the possibility of customer and user self-service.

### Cons
It will probably take time to get the domain right.

If you define and implement a DSL, then you’re taking on full responsibility for it:
* You will be responsible when there are new domain-based requests.
* You will have to provide the maintenance.
* You will have to write the documentation.
* You will have to provide the training.
* You will have to provide support when anyone using the DSL needs assistance.
* Assign a rational number to a variable and retrieve it.
* There will be no DSL IDE, DSL test framework, Google results, Stack Overflow posts, Wikipedia entries, O’Reilly publications, Generative-AI or any other external resources for your DSL users. It's all on you.

When someone makes a mistake defining a DSL specification, and it will happen, they will blame your implementation first. You may end up devoting a lot of time debugging your own code before realizing that the problem resides in the DSL specification. And then you’ll have the challenge of convincing that person that their DSL specification is incorrect probably without the benefit of knowing the specification’s actual intent.

# Rational Number Evaluator Use Case
I will feature a Domain and DSL Use Case as I progress through these Interpreter blog entries.

I previously stated that Mathematical Systems are Domains. My Domain will be Rational Numbers and the DSL will be a Rational Number Evaluator. I’m committing to this before doing any design or implementation, so the road could get a bit rocky in subsequent blogs.

Most programming languages support integers and floats. I don’t know of any popular programming languages that support rational numbers. Recall that a rational number is a number that can be represented as the ratio of two integers, i.e., a fraction.

I want the Domain to feature:
* The ability to create a rational number, but not necessarily in its simplest form. For example, __2/4__ would be accepted, but it would be simplified to __1/2__. Likewise, __4/2__ would also be accepted, but simplified to __2__.
* The ability to display a rational number in its simplest form.
* Adding a set of rational numbers returning a sum of rational numbers.
* Multiplying a set of rational numbers returning a product of those rational numbers.
* Subtracting one rational number from another returning a rational number, which could be negative.
* Dividing one rational number with another returning a rational.
* A CLI for real-time interactions.

# Summary
The Interpreter design pattern allows us to design and implement Domain-Specific Languages. But just because we can do something doesn’t mean that we should do it.

Make sure you have a domain with most of the following:
* It applies to the customer(s) problem space.
* It is reasonably scoped and bounded.
* It contains unique problem scenarios within the domain. This could be for bespoke customer and user requested behaviors. Or it could be for an industry that’s regulated, such as insurance or taxes.
* Problem scenarios within the domain are fluid. For example, political pressures can update regulations without much lead time. Government agency regulations may also be vague, incomplete, contradictory, etc.

# References
There are many online Domain-Specific Language resources. Here are some free resources:
* [Wikipedia Domain-Specific Languages](https://en.wikipedia.org/wiki/Domain-specific_language)
* [The complete guide to (external) Domain Specific Languages](https://tomassetti.me/domain-specific-languages/)
* [Developing a Complex External DSL](https://www.infoq.com/articles/External-DSL-Vaughn-Vernon/) by Vaughn Vernon
* [Why DSLs? A Collection of Anecdotes](https://www.infoq.com/articles/why-dsl-collection-anecdotes/)
* [Domain Specific Languages](https://martinfowler.com/books/dsl.html) overview of the book by Martin Fowler and Rebecca Parsons
* [Domain-Specific Languages Guide](https://martinfowler.com/dsl.html) by Martin Fowler
* [About Domain-Specific Languages](https://learn.microsoft.com/en-us/visualstudio/modeling/about-domain-specific-languages?view=vs-2022) First in a series of nine Domain-Specific Language articles by Microsoft that may be associated with a Visual Studio extension
* [Domain-Specific Languages](https://www.jetbrains.com/mps/concepts/domain-specific-languages/) JetBrains article
* and for more, Google: [Domain Specific Languages](https://www.google.com/search?q=Domain+Specific+Languages)

Here are some resources that can be purchased or are included in a subscription service:
* Domain-Specific Languages ([O'Reilly](https://learning.oreilly.com/library/view/domain-specific-languages/9780132107549/) and [Amazon](https://www.amazon.com/Domain-Specific-Languages-Addison-Wesley-Signature-Fowler/dp/0321712943))

See: [Interpreter Design Pattern Introduction/References](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html#references).
