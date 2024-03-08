---
title: Interpreter Design Pattern - Introduction
description: A Gentle Introduction to Interpreter
unlisted: true
---
<img src="/assets/InterpreterGreekPhilosophers.jpeg" alt="Greek Philosophers explaining Interpreter" width = "50%" align="center" style="padding-right: 35px;">

# Introduction
The Interpreter Design Pattern is the next of the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) series.

Interpreter is not only the pinnacle of the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html). 
I think it’s the pinnacle of the Gang of Four (GoF) design patterns sharing the top position with the [Visitor Design Pattern](https://refactoring.guru/design-patterns/visitor).

Interpreter is one of the most powerful and elegant of the design patterns. Interpreter is an extension of Composite with context. Composite is syntax. Interpreter adds semantics to that syntax.

But Interpreter doesn’t get the attention it deserves. It’s one of the nine relegated to the __Appendix A. Leftover Patterns__ in __Head First Design Patterns__. This Appendix is the round up of GoF patterns that the book didn’t describe in detail. Each of these nine patterns gets about 2 pages of attention.

[Refactoring Guru](https://refactoring.guru/) excluded it from its [catalog](https://refactoring.guru/design-patterns/catalog). Interpreter was the only pattern excluded.

[Sourcemaking](https://sourcemaking.com/) includes [Interpreter](https://sourcemaking.com/design_patterns/interpreter), but there’s a major mistake in one of their diagrams. Their structure is missing the relationship line that’s the equivalent of the Composite class reference back up to the Component interface.

# Background
I think the GoF authors are mostly responsible. While their 14-page description of Interpreter is not technically wrong, it just feels incomplete.
Their class diagram is correct, but it’s more like only presenting a stem cell without continuing how that stem cell could become a nerve cell, muscle cell, blood cell, etc.

They assume the reader has knowledge of programming language design, grammars, scanners, parsers, etc. Maybe this was a reasonable assumption in 1995 at the time of publication. The authors were academics, and these are topics that most computer science students learned to some degree. Today’s software developers might not have the same foundations.

The examples feel a bit dated, especially the example in Smalltalk.

I stumbled upon a blog years ago by a manager at Amazon, that included a paragraph along these lines:
> I thought the Gang of Four’s Design Pattern book was fantastic. I understood everything, except for pages 243-257.

I had to grab my copy of the GoF book and turn to 243. It was Interpreter. The blog continued:

> I eventually figured it out. From then on, I’d ask all interviewees if they could explain to me. If they understood it, then I’d offer them the job on the spot.

Interpreter was the final design pattern I understood. That was 7 years after I had started to learn the design patterns. I had tried, really tried, to understand it earlier. I didn’t understand the GoF’s description. I couldn’t find any alternative online descriptions that clicked with me. I sort of stumbled into my understanding of it. Then once I understood it, it was an epiphany. I understood what the GoF were trying to say, well mostly. Interpreter was so elegant. Why had I not seen its elegance before?

# Shoring up the Gang of Four
I will try to fill in some of the missing gaps from the GoF’s Interpreter presentation. Don’t worry, you won’t need access to their 14 pages. I’ll copy snippets from the book when I want to highlight sections that I’m addressing.

This is going to be a journey of multiple blogs entries. By the time I have finished, I hope I have filled enough gaps so that you can understand Interpreter and appreciate its elegance as well.

I think [Source Making Interpreter](https://sourcemaking.com/design_patterns/interpreter) distilled the elegance elegantly:
> Map a domain to a language, the language to a grammar, and the grammar to a hierarchical object-oriented design.

I may modify this list below, but here’s what I’m currently considering as subsequent blog entries:
* Domain-Specific Languages
* An Domain-Specific Language example along the way with an arithmetic expression evaluator for rational numbers. This is, it will support fractions.
* Programming Language Grammars
* Grammars and Object-Oriented design mappings. I think this is where most of the elegance of the pattern resides. I will expand beyond their stem cell diagrams.
* Scanners and Parsers
* A Use Case based upon an Interpreter/DSL I designed for a project and deployed.

# Specification is Interpreter
While I think I will need additional blogs to describe Interpreter sufficiently, I can provide an overview here.

Interpreter can sound intimidating with grammars, parsers, etc. But we’ve already encountered an Interpreter. [Specification](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html) is a specialized and limited scoped Interpreter. I mostly featured Specification in the two previous blogs because it’s a steppingstone toward understanding Interpreter as a whole.

Coincidentally, the GoF used a similar problem for their C++ example. Theirs was a Boolean expression evaluator. Specification is a Boolean evaluator as well. The main difference between the GoF example and Specification is the leaf nodes. Leaf nodes in the GoF example are either Boolean variables or true/false Boolean constants. With Specification, the leaf nodes are Boolean values based upon the satisfiability of the field attributes within a Context object. In the case of the Smart Playlist Specification, it was the Boolean results of a Track's Genre, Rating, Artist, etc.

The GoF don’t show all the steps from grammar to implementation. I’m going to fill in a few of the gaps with Specification here as a short primer, but I’ll get into more details in subsequent blog entries.

## Domain-Specific Language Primer
Specification is a [Domain-Specific Language](https://en.wikipedia.org/wiki/Domain-specific_language) (DSL). A DSL is a small language that’s designed for a specific domain. In this case, it’s the domain to specify tracks so that the user can easily build Smart Playlists.

A DSL is a specialist. It performs tasks in its domain very well, but beyond its domain, it loses its power. A general-purpose language, such as C++, Java and Python, can accommodate any domain, but not without additional domain support or infrastructure. One could argue that well design implementation will eventually become domain specific through consistent use of class, method and attribute names.

<img src="https://live.staticflickr.com/4078/4909081845_3aeb704a66_o.png" alt="Fox and Hedgehog" title="Image Source: https://www.flickr.com/photos/38299630@N05/4909081845" width = "20%" align="right" style="padding-right: 20px;">

> ___The fox knows many things, but the hedgehog knows one big thing. - Archilochus___

A Domain-Specific Language is a hedgehog.

A General-Purpose Language is a fox.

## Programming Language Grammar Primer
To the best of my knowledge all programming languages are defined via [grammars](https://en.wikibooks.org/wiki/Introduction_to_Programming_Languages/Grammars), including DSLs. A grammar is a set of rules that document the syntax and structure of language concepts. Grammar rules define a unique tree of language concepts for any program for that language. Unambiguous behaviors are assigned to all possible tree configurations.

All programs in a language can trace their structural origins to their shared grammar. The same grammar rules that produce “Hello World!” are the same ones that produce some of the most sophisticated software systems ever created. The only difference is in the size and complexity of the tree of language concepts.

DSLs tend to have smaller grammars than general-purpose programming languages. They may consist of only a handful of rules. This makes Interpreter an ideal implementation for a DSL as we'll eventuall see. A surprisingly small Interpreter implementation can accommodate a DSL program of any size. Most DSL programs tend to be small, but there’s no restriction on size. Staying with Specification, it could be a program with one statement, and therefore one object in the tree. Or it could be a program of hundreds of statements and therefore hundreds of objects in the tree.

Grammars generally only have three basic rule types:
* __IS-A__ rule, which lists the grammar rules that are special cases of a more general rule.
* __HAS-A__ rule, which defines the language elements and grammar rules that comprise this rule.
* __DEFINITION__, __ which is a rule that’s not defined in terms of other rules.

Here’s a quick example of some grammar rule syntax:
```
RuleX ::= RuleA | RuleB  // i.e., IS-A
RuleA ::= “A” “(“ RuleX “)” // i.e, HAS-A, which contains indirect recursion
RuleB ::= “B” // i.e., DEFINITION
```
The __IS-A__ and __HAS-A__ relationships are implied by the __OR__ (`|`) and __AND__ (which is usually defaulted) operations in the rule definitions.

NOTE: Grammar rules originate from automata theory. Their application to define programming languages is a practical application.

## Smart Playlist Specification
We’re going to start with the [Specification Smart Playlist Use Case](https://jhumelsine.github.io/2024/03/07/specification-design-pattern-use-case.html) and specifically this smart playlist:

<img src="https://core0.staticworld.net/images/article/2013/05/smart-playlist3-new-100036189-orig.png" alt="Smart Playlist" title="Image Source: https://core0.staticworld.net/images/article/2013/05/smart-playlist3-new-100036189-orig.png" width = "70%" align="center" style="padding-right: 35px;">

iTunes provides a GUI for the user. Interpreters tend to be more text based. If iTunes had instead provided a text-based version to specify smart playlists, then the above GUI representation might look more like this, which is basically the GUI image above, but with text rather than graphics:
```
fiveStarAltRock = all(
                     any(
                        genre(Alternative),
                        genre(Rock),
                        genre(New Wave)
                     ),
                     all(
                        rating(5),
                        all(
                           artist(not(“The Rolling Stones”)),
                           artist(not(“U2”))
                        )
                     )
                  )
```

## Smart Playlist Attributes
Before jumping into the grammar, let’s see if we jot down a few observations:
* Genre is a Specification.
* Rating is a Specification.
* Artist is a Specification.
* All is a Specification.
* All has a collection of Specifications.
* Any is a Specification.
* Any has a collection of Specifications.
* Not is a Specification.
* Not has a single Specification.

## Smart Playlist Grammar
Given this brief example and the grammar notation above, let’s start with this Specification grammar:
```
Specification ::= Genre | Rating | Artist | All | Any | Not // IS-A
Genre ::= “genre” “(“ genreType “)”
Rating ::= “rating” “(“ ratingValue “)”
Artist ::= “genre” “(“ artistName “)”
All ::= “all” “(“ SpecificationList “)”
Any ::= “any” “(“ SpecificationList “)”
Not ::= “not” “(“ Specification “)” // NOTE: Indirectly Recursive.
SpecificationList ::= Specification (“,” Specification)* // NOTE: Indirectly Recursive as well. Star indicates any number of repetitions, including zero.
```
## Grammar to Design
The class design emerges from the grammar. Interfaces and concrete classes tend to map directly from the grammar. Abstract classes tend to emerge during design refactoring. Compare the grammar above with the class design. Hopefully the major relationships in the grammar and the design are fairly obvious:

<img src="/assets/SpecificationPlaylistComposite.png" alt="Composite Playlist Classes" width = "90%" align="center" style="padding-right: 35px;">

## Scanner and Parser
We still need to convert the `fiveStarAltRock` text definition above into the following object tree:

<img src="/assets/SpecificationPlaylistObjects.png" alt="Leaf Playlist Classes" width = "90%" align="center" style="padding-right: 35px;">

This will be done by:
* The scanner, which extracts individual tokens from the text definition.
* The parser, which creates objects based upon the tokens and assembles them. Since the grammar is recursive, the parser will be recursive as well.

There are whole books and college courses about scanners and parsers. I can only scratch their surface, and even then in a subsequent blog.
The scanner and parser together are a type of Configurer. Previous Configurers have mostly been hardcoded. Scanners and parsers are different. The result is still the same in that they will construct the objects. The difference is that they will do it based upon a program, which can be thought of as a blueprint for the parse tree object structure.

Their behavior is close to the [Builder Design Pattern](https://refactoring.guru/design-patterns/builder), for which I’ll probably provide a blog as I get into more scanner and parser details.

# Summary
Interpreter is a good approach for implementing a Domain-Specific Language. There’s an almost algorithmic process from Domain-Specific Language to Grammar to Design to Implementation.

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* TBD

Here are some resources that can be purchased or are included in a subscription service:
* TBD
