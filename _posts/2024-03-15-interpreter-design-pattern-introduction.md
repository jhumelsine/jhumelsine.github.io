---
title: Interpreter Design Pattern - An Introduction
description: A "Mostly" Gentle Introduction to Interpreter
unlisted: true
---
<img src="/assets/InterpreterGreekPhilosophers.jpeg" alt="Greek Philosophers explaining Interpreter" width = "50%" align="center" style="padding-right: 35px;">

# Introduction
The Interpreter Design Pattern is the next of the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) series.

Interpreter sits not only at the pinnacle of the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html), but I think it resides at the top of the Gang of Four (GoF) design patterns sharing the position with the [Visitor Design Pattern](https://refactoring.guru/design-patterns/visitor).

Interpreter is the fulfillment of customized-rule/policy-based behaviors, which I mentioned in [Composable Design Patterns – Basic Concepts/Use Cases for Composability Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html#use-cases-for-composability-design-patterns). Interpreter has the potential to support customer and user self-service features and behaviors. Customer/User self-service is a double-edged sword, which I'll discuss in a bit more detail in a subsequent blog.

Interpreter is one of the most powerful and elegant of the design patterns. Interpreter is the extension of [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) with the addition of context. Composite is syntax. Interpreter adds semantics to that syntax.

But Interpreter doesn’t receive the attention it deserves.

It’s one of the nine GoF design patterns relegated to __Appendix A. Leftover Patterns__ in __Head First Design Patterns__. This Appendix is a round up of patterns that the book didn’t describe in detail. Each of these nine patterns gets about two pages of attention which is woefully insufficient for anything beyond an overview.

[Refactoring Guru](https://refactoring.guru/) excluded Interpreter from its pattern [catalog](https://refactoring.guru/design-patterns/catalog). Interpreter is the only GoF pattern missing.

[Sourcemaking](https://sourcemaking.com/) includes [Interpreter](https://sourcemaking.com/design_patterns/interpreter), but there’s an error of omission in one of their diagrams. Their structure diagram is missing the relationship line that’s the equivalent of the Composite class reference back up to the Component interface.

# Background
Ironically, I think the GoF authors are mostly responsible for this lack of attention.
While their 14-page description of Interpreter is not technically wrong, it just feels incomplete.
I suspect that many readers don't understand what the GoF were trying to convey.
Their class diagram is correct, but it only hints at the potential of the pattern. 
It’s like a biology textbook presenting stem cells without explaining how stem cells can become nerve cells, muscle cells, blood cells, etc.

The GoF assume their readers have knowledge of programming language design, grammars, scanners, parsers, etc.
These may have been reasonable assumptions in 1995 at the time of publication, but today’s software developers might not have the same foundations.
The authors were academics, and these are topics that most computer science students learned to some degree.
Self-taught developers, bootcamp developers and developers who have degrees in other fields, may not have the background to follow the GoF's description.

The implementation examples feel a bit dated, especially the Smalltalk implementation example.

I stumbled upon a blog years ago written by a manager at Amazon, that included statements along these lines:
> I thought the Gang of Four’s Design Pattern book was fantastic. I understood everything, except pages 243-257. ...

I grabbed my copy of the GoF book off the shelf and turn to 243. It was Interpreter.

The blog continued:

> ... I eventually figured it out. From then on, I’d ask all interviewees if they could explain Interpreter to me. If they could do so sufficiently, then I’d offer them the job on the spot.

Interpreter was the final design pattern I understood. That was 7 years after I had started to learn the design patterns. I had tried, really tried, to understand it earlier. I didn’t understand the GoF’s description. I couldn’t find any alternative online descriptions that clicked with me. I sort of stumbled into my understanding of it by accident. Then once I understood it, it was an epiphany. I understood what the GoF were trying to say, well mostly. Interpreter is so elegant. Why had I not seen its elegance before?

# Shoring up the Gang of Four
I will try to fill in some of the missing gaps from the GoF’s Interpreter presentation. Don’t worry, you won’t need access to their 14 pages. I’ll copy snippets from the book when I want to highlight sections that I’m addressing.

This is going to be a journey of multiple blogs entries. By the time I have finished, I hope I have filled enough gaps so that most can understand Interpreter and appreciate its elegance as well.

I think [Source Making Interpreter](https://sourcemaking.com/design_patterns/interpreter) distilled the elegance quite elegantly:
> Map a domain to a language, the language to a grammar, and the grammar to a hierarchical object-oriented design.

Even with this elegant description, a few more specifics would be useful.

I may modify this list below, but here’s what I’m currently considering as subsequent blog entries for Interpreter. I'll adjust and add links here when posted:
* Domain-Specific Languages
* A Domain-Specific Language example along the way with an arithmetic expression evaluator for rational numbers. This is, it will support fractions.
* Programming Language Grammars
* Grammars and Object-Oriented design mappings. I think this is where most of the elegance of the pattern resides.
* Scanners and Parsers
* A Use Case based upon a project-based Interpreter/DSL that I designed, and the project deployed.

# Specification is Interpreter
While I think I will need additional blogs to describe Interpreter sufficiently, I can provide a quick overview here.

Interpreter can sound intimidating with grammars, parsers, etc. But we’ve already encountered an Interpreter example in this blog series. [Specification](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html) is a specialized and limited scoped Interpreter.
If someone can understand the nuts-and-bolts of Specification, then Interpreter isn't much of a leap beyond that.
I featured Specification in the two previous blogs ([Specification](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html) and [Smart Playlist Use Case](https://jhumelsine.github.io/2024/03/07/specification-design-pattern-use-case.html)) because it’s a steppingstone toward understanding Interpreter as a whole.

Coincidentally, the GoF used a similar problem, a Boolean expression evaluator, for their C++ Interpreter example. Specification is a Boolean evaluator as well. The main difference between the GoF example and Specification is the leaf nodes. Leaf nodes in the GoF example are either Boolean variables or true/false Boolean constants. With Specification, the leaf nodes are Boolean values based upon the satisfiability of the field attributes within a Context object. In the case of the Smart Playlist Specification, the leaf node Boolean values derive from a Track's Genre, Rating, Artist, and other attributes.

The GoF don’t show all the steps from grammar to implementation in their C++ example. I’m going to fill in a few of the gaps with Specification here as a short primer. I’ll present more details in the subsequent blog entries.

## Domain-Specific Language Primer
Specification is a [Domain-Specific Language](https://en.wikipedia.org/wiki/Domain-specific_language) (DSL). A DSL is a small language that’s designed for a specific domain. In the [Smart Playlist Use Case](https://jhumelsine.github.io/2024/03/07/specification-design-pattern-use-case.html), it’s a domain that allows the user to organize Smart Playlists based upon a set of user defined rules that dynamically organize tracks in those playlists based upon their attribute values.

A DSL is a specialist. It solves problems in its domain very well. But venture beyond its domain, and it loses its power. A general-purpose language, such as C++, Java and Python, can accommodate any domain, but not without additional domain support or infrastructure. One could argue that well designed general-purpose language implementation should eventually become domain specific through consistent nomenclatures with its class, method, parameter and field attribute names. But unless a ubiquitous language is enforced, we often don't achieve this.

A DSL allows you to solve a problem succinctly if the problem is within the domain. A general-purpose language allows you to solve any problem, but it may take more time to establish domain foundations before achieving a solution.

<img src="https://live.staticflickr.com/4078/4909081845_3aeb704a66_o.png" alt="Fox and Hedgehog" title="Image Source: https://www.flickr.com/photos/38299630@N05/4909081845" width = "20%" align="right" style="padding-right: 20px;">

> ___The fox knows many things, but the hedgehog knows one big thing. — Archilochus, Greek Lyric Poet___

A Domain-Specific Language is a Hedgehog.

A General-Purpose Language is a Fox.

## Programming Language Grammar Primer

<img src="https://live.staticflickr.com/2905/14783088973_3f7d1f2e2a_o.jpg" alt="Sentence Diagrams" title="Image Source: https://www.flickr.com/photos/internetarchivebookimages/14783088973" width = "30%" align="right" style="padding-right: 20px;">

To the best of my knowledge all programming languages are defined via [grammars](https://en.wikibooks.org/wiki/Introduction_to_Programming_Languages/Grammars), including DSLs. A grammar is a set of rules that documents the syntax and structure of a programming language:
* All programs in a programming language can trace their structure back to grammar rules.
* Each program projects a unique parse tree consistent with its grammar rules. This is like sentence diagramming.
* The tree leaf nodes correspond to the language elements in the program itself, such as keywords, identifiers, symbols, etc.
* The tree non-terminal nodes indicate the relationship among the leaf nodes and other non-terminal nodes. That is, they support the organization structure.
* Behaviors are added to the organization structures. This is technically beyond grammar definition, but it's the last step achieving behavior from programming languages defined via a grammar.

The same grammar rules that support “Hello World!” are the same ones that support the most sophisticated software systems ever created. The only difference is in the size and complexity of the program itself and the unique parse tree that's projected from the program.

DSLs tend to have smaller grammars than general-purpose programming languages. They may consist of only a handful of rules. A surprisingly small Interpreter implementation can accommodate the elements of a DSL. This makes Interpreter an ideal implementation for a DSL as we'll eventually see.

Most DSL programs tend to be small, but there’s no restriction on the size of the program. Staying with the Specification example, it could be a specification with one statement, and therefore one object in the tree. Or it could be a specification of hundreds of statements and therefore hundreds of objects in the tree.

Grammars generally have three basic rule types:
* __OR__ rule, which lists the grammar rules that are more specific versions of the general rule.
* __AND__ rule, which defines the language elements and grammar rules that comprise this rule.
* __DEFINITION__ rule, which is a rule that’s not defined in terms of other rules.

Here’s a quick example of some grammar rule syntax:
```
RuleX ::= RuleA | RuleB  // i.e., RuleX can become RuleA OR RuleB
RuleA ::= “A” “(“ RuleX “)” // i.e, RuleA is comprised of "A" AND "(" AND RuleX AND ")".
RuleB ::= “B” // i.e., DEFINITION
```

The __OR__ operation is denoted by `|`. The __AND__ operation is assumed, much like multiplication is assumed in algebra when not specified. We assume that mass and acceleration are being multiplied in `F = ma`. The same applies to grammar rules. Assume __AND__ when not provided.

NOTE: Grammar rules originate from automata theory. Their use in defining programming languages is a practical application.

## Smart Playlist Specification
We’re going to start with the [Specification Smart Playlist Use Case](https://jhumelsine.github.io/2024/03/07/specification-design-pattern-use-case.html) and specifically this smart playlist:

<img src="https://core0.staticworld.net/images/article/2013/05/smart-playlist3-new-100036189-orig.png" alt="Smart Playlist" title="Image Source: https://core0.staticworld.net/images/article/2013/05/smart-playlist3-new-100036189-orig.png" width = "70%" align="center" style="padding-right: 35px;">

iTunes provides a GUI for the user. Interpreters tend to be text based. If iTunes had instead provided a text-based version to specify smart playlists, then the above GUI representation might look more like this text specification, which mirrors the GUI image above, but with text rather than graphics:
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
Before jumping into the grammar, let’s jot down a few observations:
* Genre is a Specification.
* Rating is a Specification.
* Artist is a Specification.
* All is a Specification.
* All has a collection of Specifications.
* Any is a Specification.
* Any has a collection of Specifications.
* Not is a Specification.
* Not has a single Specification.

Notice that all of these observations are described in terms of __IS-A__ or __HAS-A__. __IS-A__ will have a relationship with __OR__. __HAS-A__ will have a relationship with __AND__.

## Smart Playlist Grammar
Given this brief example and the grammar notation above, we can define a Specification grammar as:
```
Specification ::= Genre | Rating | Artist | All | Any | Not
Genre ::= “genre” “(“ genreType “)”
Rating ::= “rating” “(“ ratingValue “)”
Artist ::= “genre” “(“ artistName “)”
All ::= “all” “(“ SpecificationList “)”
Any ::= “any” “(“ SpecificationList “)”
Not ::= “not” “(“ Specification “)”
SpecificationList ::= Specification (“,” Specification)* // NOTE: Star indicates any number of repetitions, including zero.
```

The `Not` and `SpecificationList` rules include `Specification` in their definition. This means that the grammar is indirectly recursive. This is what allows `all`, `any` and `not` constructs to be nested. The grammar supports a tree structure that's as deep and wide as needed.

## Grammar to Design
The class design emerges from the grammar, especially since __IS-A__ is associated with __inheritance__ and __HAS-A__ is associated with __composition__. Interfaces tend to map to __OR__ grammar rules, and concrete classes tend to map to __AND__ grammar rules. Abstract classes are not part of the grammars and tend to emerge during design refactoring. Compare the grammar above with the class design. Hopefully the major relationships in the grammar and the design are obvious:

<img src="/assets/SpecificationPlaylistComposite.png" alt="Composite Playlist Classes" width = "90%" align="center" style="padding-right: 35px;">

## Scanner and Parser
We still need to convert the `fiveStarAltRock` text definition above into the following object tree (NOTE: The names, such as _genres_, _constraints_, etc. won't be in tree. This is a copy of the tree from the previous blog):

<img src="/assets/SpecificationPlaylistObjects.png" alt="Leaf Playlist Classes" width = "90%" align="center" style="padding-right: 35px;">

This will be done by:
* The scanner, which extracts individual tokens from the text definition.
* The parser, which creates objects based upon the tokens and assembles them. Since the grammar is recursive, the parser will be recursive as well.

There are whole books and college courses about scanners and parsers. I can only scratch their surface here or in subsequent blogs.

The scanner and parser together are a type of Configurer. Previous Configurer examples have mostly been hardcoded.
Scanners and parsers are different.
The result is still a set objects configured in a data structure — in this case a parse tree.
The difference is that the object tree will be based upon a program consistent with the grammar.
A program can be thought of as a blueprint for the object tree.
The scanner and parser will construct the object tree based upon that blueprint.

Their behavior is close to the [Builder Design Pattern](https://refactoring.guru/design-patterns/builder), for which I’ll probably provide a blog as I get into more scanner and parser details.

Once the object tree, as shown above, has been constructed, then Tracks can be passed to it to determine their satisfiability based upon a Track's attributes and the configuration of the Specifications in the tree.

# Summary
Interpreter is a great approach for implementing a Domain-Specific Language. Interpreter is part of the algorithmic process from Domain-Specific Language to Grammar to Design to Implementation.

# References
There are additional online resources with diagrams and implementations in different programming languages. Here are some free resources:
* [Wikipedia Interpreter Design Pattern](https://en.wikipedia.org/wiki/Interpreter_pattern)
* [Source Making Interpreter Design Pattern](https://sourcemaking.com/design_patterns/interpreter)
* [Baeldung Interpreter Design Pattern in Java](https://www.baeldung.com/java-interpreter-pattern)
* [OODesign Interpreter Design Pattern](https://www.oodesign.com/interpreter-pattern)
* [Learn CS Design Interpreter Design Pattern](https://www.learncsdesign.com/learn-the-interpreter-design-pattern/)
* [DoFactory Interpreter Design Pattern](https://www.dofactory.com/net/interpreter-design-pattern)
* [Reactive Programming Interpreter Design Pattern](https://reactiveprogramming.io/blog/en/design-patterns/interpreter)
* and for more, Google: [Interpreter Design Pattern](https://www.google.com/search?q=Interpreter+Design+Pattern)

Here are some resources that can be purchased or are included in a subscription service:
* Gang of Four Composite Design Pattern, page 243 ([O'Reilly](https://learning.oreilly.com/library/view/design-patterns-elements/0201633612/ch05.html#page_243) and [Amazon](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612))
* Head First Design Patterns, Appendix A: Leftover Patterns, Interpreter ([O'Reilly](https://learning.oreilly.com/library/view/head-first-design/9781492077992/app01.html#idm46060275907152) and [Amazon](https://www.amazon.com/Head-First-Design-Patterns-Object-Oriented-ebook/dp/B08P3X99QP))
