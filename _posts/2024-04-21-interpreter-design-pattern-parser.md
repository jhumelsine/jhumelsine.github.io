---
title: Interpreter Design Pattern – Scanner and Parser
description: The basics of Scanner and Parser theory and practice.
unlisted: true
---.

# Introduction
This blog continues the [Interpreter Design Pattern](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) series by venturing into territory that’s often not covered in Interpreter write ups – scanners and parsers.

My previous [blog]( https://jhumelsine.github.io/2024/04/17/interpreter-design-pattern-implementation.html) showed how to implement the Domain-Specific Language (DSL) classes from the design. I added [One Final Test]( https://jhumelsine.github.io/2024/04/17/interpreter-design-pattern-implementation.html#one-final-test) to get a sense of how the components could be assembled to evaluate several Rational expressions. I concluded that section with:
> … it’s __HORRIBLE__. Unfortunately, this is where the Gang of Four and other Interpreter Design Pattern write ups tend to leave you. Do not fret, dear reader. I shall not abandon you. I am bound and determined to complete this DSL use case and implement it in such a way that it will be a fully functioning DSL, even if the domain is limited to Rational number evaluations.
> 
> The final implementation will be the parser, which will process the DSL and build the elements as shown above. The DSL should be easier to express intent than the raw code.

My original intent was to describe scanners and parsers and then include the scanner and parser implementation for the Rational Expression Evaluator, but the scanner and parser write up required more space than I anticipated. The next blog will include the implementation.

__Trigger Warning__: Unlike the previous blog, which contained no theory, this one has some, but I’ve tried to provide the theory in context and there are no proofs.

Theory and practice are tightly coupled for scanners and parsers. Scanners and parsers can be auto-generated due to the mathematical rigor defined in the theory. Even if you code your own, the theory dictates most of the design.
> 
# Gang of Four Parser References
Technically, the Gang of Four (GoF) didn’t leave the reader completely hanging.  There are a few parser references from them in their Interpreter description:
> For complex grammars, the class hierarchy for the grammar becomes large and unmanageable. Tools such as __parser__ generators are a better alternative in such cases. They can interpret expressions without building abstract syntax trees, which can save space and possibly time.
> 
> Classes defining nodes in the abstract syntax tree have similar implementations. These classes are easy to write, and often their generation can be automated with a compiler or __parser__ generator.
> 
> … grammars containing many rules can be hard to manage and maintain. Other design patterns can be applied to mitigate the problem. But when the grammar is very complex, other techniques such as __parser__ or compiler generators are more appropriate.
> 
> The Interpreter pattern doesn’t explain how to _create_ an abstract syntax tree. In other words, it doesn’t address __parsing__. The abstract syntax tree can be created by a table-driven __parser__, by a hand-crafted (usually recursive descent) __parser__, or directly by the client.

I suspect that the GoF didn’t go into parser details, since they may have assumed that the reader was already familiar with parsers. This may have been a reasonable assumption in 1995 when the book was published. But today, many developers are self-taught or bootcamp graduates, and they may not have been exposed to parsers.

The Parser creates objects and assembles them into a tree, which goes by several names, such as: abstract syntax tree, parse tree, or in pattern parlance, [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) object tree. I will probably use the terms parse tree and composite object tree interchangeably in this blog.

The GoF grouped several [Creational Patterns](https://refactoring.guru/design-patterns/creational-patterns) together, which featured different techniques to create objects without the client needing to call `new()` directly. I’ve presented [Factories](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) previously, which is a Creational Pattern.

# Configurer
Fundamentally the Parser is a [Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer). The GoF rarely showed how Creational Patterns could be used in context as [Configurers](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer) in their other design patterns. I guess I should not be too surprised that they didn’t go into more detail with Parsers, since they didn’t feature other Configurers in other design pattern presentations.

My previous Configurer representations have mostly been straightforward applications of the [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) pattern. These examples have mostly been hardcoded, and any variation has generally been achieved via a switch statement.

Parser is more sophisticated. It will dynamically build a Composite object tree based upon a script/program written in the DSL grammar. There is one design pattern that’s close to this concept, but the GoF didn’t fully flesh out its ability to be a Parser. It’s the Builder Design Pattern, and I’ll blog about it in the future. For now, here are a few references:
* [SourceMaking Builder](https://sourcemaking.com/design_patterns/builder)
* [Refactoring.guru Builder](https://refactoring.guru/design-patterns/builder)

Builder hints at Parser behavior in that it creates and assembles objects based upon a configuration specification, but that does not seem to be its primary intent.

# Parser and Scanner Theory and Practice
By pure serendipity, I took [Automata Theory](https://en.wikipedia.org/wiki/Automata_theory) and [Compiler Design and Implementation](https://en.wikipedia.org/wiki/Compiler) in the same semester at college. There were a few times where I literally had the same lecture material in both classes on the same day. The Automata Theory course presented the theory of a concept in my first period class, and then the Compiler course presented the same concept as practice for compilers in my third period class. Seeing theory and practice of the same concept helped me understand both.

Given that [Jeffery Ullman](https://en.wikipedia.org/wiki/Jeffrey_Ullman) was the co-author for my text books in both classes (the [Cinderella](https://en.wikipedia.org/wiki/Introduction_to_Automata_Theory,_Languages,_and_Computation) book and the [Dragon](https://en.wikipedia.org/wiki/Principles_of_Compiler_Design) book respectively), I guess I should not be surprised. Both books still reside on my bookshelf over 40 years later.

## Theory
[Automata Theory](https://en.wikipedia.org/wiki/Automata_theory) is about mathematical models of computation and what is [computable](https://en.wikipedia.org/wiki/Theory_of_computation). There are three basic mathematical models, also known as automata:
* [Finite Automata](https://en.wikipedia.org/wiki/Finite-state_machine)
* [Pushdown Automata](https://en.wikipedia.org/wiki/Pushdown_automaton)
* [Turing Machines](https://en.wikipedia.org/wiki/Turing_machine)

Automata Theory defines these machines and proves what they can and cannot compute.

### Finite Automata
https://upload.wikimedia.org/wikipedia/commons/thumb/c/c2/Deterministic_Finite-state_Automaton.svg/1029px-Deterministic_Finite-state_Automaton.svg.png?20150524185635
https://commons.wikimedia.org/wiki/File:Deterministic_Finite-state_Automaton.svg
 
A [Finite Automata](https://en.wikipedia.org/wiki/Finite-state_machine) is a state machine. It consists of a set of states with transitions that move from state to state. From a start state, it reads a list of symbols (or characters), which are event triggers that move the machine from one state to another. Each read symbol triggers one state transition.

After all the symbols have been read with their corresponding state transitions applied, the finite automaton will be in an _Accepting_ state (double circle in the diagram) or a _Rejecting_ state (single circle in the diagram). If _Accepting_, then the list symbols, i.e., string, are part of a language that the finite automaton accepts. Otherwise, it is not part of that language.

If the finite automaton encounters a symbol in mid-list processing for which there is not an event transition to another state, then that string is not in the language either.

Compressing several weeks of lecture proofs into one sentence, the only types of languages that a finite automata can accept are [regular languages/expressions](https://en.wikipedia.org/wiki/Regular_language). This is the boundary of finite automata computation.

There’s even a stronger relationship between the two. Given any finite automaton, we can always construct a regular language that describes the strings that the automaton accepts. Given any regular language, we can always construct a finite automaton that accepts strings in that language. Finite automata and regular expressions are two sides of the same coin.

### Pushdown Automata
https://upload.wikimedia.org/wikipedia/commons/thumb/7/71/Pushdown-overview.svg/1280px-Pushdown-overview.svg.png
https://en.wikipedia.org/wiki/Pushdown_automaton#/media/File:Pushdown-overview.svg
 
Can we design automata that have more computational power than Finite Automata?

This leads to the [Pushdown Automata](https://en.wikipedia.org/wiki/Pushdown_automaton), which is a Finite Automata with a stack added to it. This additional bit of memory allows us to do more.

The only types of languages that a Pushdown Automata accepts are [Context-Free Grammars](https://en.wikipedia.org/wiki/Context-free_language), which I introduced in the [Interpreter Grammar](https://jhumelsine.github.io/2024/04/02/interpreter-design-pattern-grammars.html) blog.

The same sort of relationship holds. Given any Pushdown Automaton, we can construct a Context-Free Grammar that defines what it accepts. Given any Context-Free Grammar, we can construct a Pushdown Automaton that accepts that grammar. Pushdown Automata and Context-Free Grammars are two sides of their own same coin.

Recall that Context-Free Grammars are rules defined in terms of other rules with recursive references as well. When a referenced rule has been completed, the grammar returns to the rule from which it was referenced. This is conceptually equivalent to a procedure call. Programming languages manage procedure calls via the call stack. The Pushdown Automata stack provides the same functionality. It remembers the state of where it left off so that it can return to that state.

But stack memory has two limitations:
* We can only access the top element of the stack.
* Once the top element has been popped off the stack, that information is lost.

### Turing Machine
https://upload.wikimedia.org/wikipedia/commons/thumb/b/be/Example_of_a_Turing_machine.svg/524px-Example_of_a_Turing_machine.svg.png?20230901133749
https://commons.wikimedia.org/wiki/File:Example_of_a_Turing_machine.svg
 
Can we design an automaton that has more computational power than Pushdown Automata?

This leads to the [Turing Machine](https://en.wikipedia.org/wiki/Turing_machine), named after its creator [Alan Turing](https://en.wikipedia.org/wiki/Alan_Turing).
A Turing Machine is a Finite Automaton, but we’re not going to add a stack like we did with the Pushdown Automata.
The main difference between a Finite Automata and a Turing Machine is how each interacts with its list of symbols/characters. A Finite Automata can only read the list one symbol at a time moving forward in the list. A Turing Machine can do the following with its list of symbols:
* Change the current symbol to another symbol.
* Move one position forward in the list or backward in the list.

That relatively simple list change from Finite Automata to Turing Machines unlocks all known forms of computation. See: [Church-Turing Thesis](https://en.wikipedia.org/wiki/Church%E2%80%93Turing_thesis). Turing proved that even when you try to define a more powerful Turing Machine, he could still use a basic Turing Machine to emulate the more “powerful” version. No matter what new direction we go into, we return to where we started. For example, a Turing Machine with multiple lists of symbols is no more powerful than a Turing Machine with one list of symbols. That is, given any multiple list Turing machine, we can always create a single list Turing Machine that returns the same computable results. The single list Turing Machine will have more components, and it will probably take more cycles to complete, but it won’t compute anything new.

Turing Machines are hardcoded. They are the mathematical equivalent of breadboards. Turing proved there existed a Turing Machine that could read the configuration specification of any hardcoded Turing Machine and emulate its behavior. It’s known as a [Universal Turing Machine](https://en.wikipedia.org/wiki/Universal_Turing_machine). A Universal Turing Machine can be programmed! All modern computers are Universal Turing Machines.
Turing also proved that some problems are not computable. That is, there is no solution to them. The most famous unsolvable problem is the [Halting Problem](https://en.wikipedia.org/wiki/Halting_problem).

Alan Turing is considered the father of Theorical Computer Science, and he did most of this work before the first electronic computers were built. Tragically, we lost him too soon.

There are so many wild and wonderful topics here, but I need to stop before I venture too deep down into the rabbit hole.

## Practice
https://upload.wikimedia.org/wikipedia/commons/d/d6/Parser_Flow%D5%B8.gif
https://en.wikipedia.org/wiki/Parsing#/media/File:Parser_Flow%D5%B8.gif
 
In general, Interpreters will scan, parse and evaluate a statement each time it encounters it. For example, the body of a `while` loop will be scanned, parsed and reevaluated in each loop execution. Interpreters tend to have more runtime flexibility, but these programs tend to execute slower.

In general, Compilers will scan and parse the entire program once, and create object code for the entire program. Compilers tend to have less runtime flexibility, but these programs tend to execute faster.

In the [Grammar to Design](https://jhumelsine.github.io/2024/04/07/interpreter-design-pattern-design.html) blog, I stated:
> Explicit token elements, such as ‘(‘ or ‘)’ or keywords, are used for parsing, but they won’t be in the design. The token elements are road signs that help the parser identify where rules start and end and verify that parsing is on track.

These explicit tokens will be critical in parsing. It’s how the parser determines which grammar rules to apply.

Interpreters and Compilers are comprised of three basic parts:
* Scanner, also known as Lexical Analyzer, Tokenizer among other terms
* Parser, also known as the Syntactical Analyzer
* Semantic Analyzer

Allow me to introduce these three concepts using natural language. Consider the following letters in order: __p__, __o__, __o__, __l__.

A scanner would combine those four letters into the word: __pool__. But this alone is not enough to know how it is used.
Let’s continue with __pool__ in the following sentences:
* _We need to __pool__ our resources to complete this task._
* _The kids are really going to enjoy the __pool__ this summer._
* _We need to __pool__ our resources to complete the __pool__ so that the kids can enjoy it this summer. I may need to __pull__ some money from our savings account to pay for it._

A parser would determine how each word functions in each sentence. In the first sentence, __pool__ is a verb. In the second sentence, __pool__ is a noun. In the third sentence, __pull__ is a verb.

Where did __pull__ come from? While __pool__ and __pull__ are spelled differently, they are pronounced the same. In spoken language, we’d need to distinguish which use applies.

Parsers identify parts of speech, but not their meaning. They focus upon syntax not semantics. The semantic analyzer provides meaning and context.

The Interpreter/Compiler difference has an equivalent with natural language. Translating conversation between two speakers of different languages is like an Interpreter. Each sentence must be translated even if repeated. Translating a written document from one language to another is like a Compiler. Each sentence only needs to be translated once.

### Scanner
A script/program is one long ASCII string. The scanner groups those string characters into higher-level concepts. This entire blog is one long ASCII string, but readers group words, punctuation, etc. This is most often done via whitespace and agreed upon punctuation rules.
Scanners do the same. Here are a few examples of the type of tokens a scanner may decide applies to a sequence of characters:
* Reserved words, such as `if`, `while`, `for`, etc.
* Symbols, such as `=`, `==`, `<`, `<=`, `,`, `.`, `+`, `++`, `->`, etc. Note that even though `==`, `<=`, `++` and `->` are two characters, they’re still considered one symbol token. The same would apply for `===` in Javascript.
* Integers and floats.
* Identifiers, which are usually hexadecimal strings starting with a letter. These would be names of variables, classes, methods, etc. chosen by developers.
Scanners also strip out comments.

Each language has its own scanner token rules. For example, reserved words will vary among languages and while `===` is a token in Javascript, it is not a token in C++ or Java.

Scanner token rules can be defined using Regular Languages, and from the theory above, we know that given a Regular Language, we can always construct a Finite Automaton, or Finite State Machine, that will accept and identify the tokens defined by that Regular Language.

State Machines are an ideal implementation choice for Scanners.

### Parser
The Parser adds syntactic structure to the tokens created by the Scanner. Returning to natural language, if a Scanner organizes letters into words, then the Parser organizes words into sentences.

Programming language syntax is defined using Context-Free Grammars. Each language has its own grammar rules. From the theory above, we know that given a Context-Free Grammar, we can always construct a Pushdown Automaton that will organize the scanner tokens into a parse tree.

Pushdown Automata are an ideal implementation choice for Parsers. When was the last time you coded a Pushdown Automaton? I never have.

The GoF use the phrase: _ hand-crafted (usually __recursive descent__) _parser_. A [Recursive Descend Parser](https://en.wikipedia.org/wiki/Recursive_descent_parser) is a [top-down parser](https://en.wikipedia.org/wiki/Top-down_parsing). More specifically, I’ll describe an [LL(k) Parser](https://en.wikipedia.org/wiki/LL_parser) which parses rules __Left__ to Right applying the __Leftmost__ rule looking no more than __k__ tokens ahead.

These parsers basically work this way:
* Each grammar rule will have its own method.
* All rules will tend to be __OR__ or __AND__ rules (See: [What the Gang of Four Skimmed Over](https://jhumelsine.github.io/2024/04/07/interpreter-design-pattern-design.html#what-the-gang-of-four-skimmed-over)):
    * When considering an __OR__ rule, determine which of the possible more specific rule options applies, and call that rule’s method. This may require peeking at the next token or two, the __k__ lookahead, until we know which rule method to call.
    * When considering an __AND__rule, call each rule’s method that’s in the __AND__ rule’s definition. There’s generally no token look ahead needed here, but tokens are usually consumed from the scanner.
* Start with the top/main rule.
* Continue until all tokens have been consumed and all rule methods have been processed.

This description is a form of Pushdown Automata. The flow of control in the methods is the “state machine” and the implicit call stack is the stack. A Parser is still a Pushdown Automata, but it’s in the form of more conventional programming language constructs.

The design approach should make more sense with the Parser I plan to create and post in the next blog.

### Semantic Analyzer
The Semantic Analyzer adds meaning to the parse tree. In a Compiler, the Semantic Analyzer will tend to generate object code, which could be machine code, [Java Bytecode]( https://en.wikipedia.org/wiki/Java_bytecode), or a higher level language. The object code in my college compiler course was Pascal.

In an Interpreter, especially with our design pattern, it will be the classes already implemented from the grammar as seen in the [Grammar to Implementation](https://jhumelsine.github.io/2024/04/17/interpreter-design-pattern-implementation.html) blog.

# Scanner and Parser Generators
The real payoff with this theory and practices is that this is so well defined with mathematical rigor that scanners and parsers can be generated from regular languages and context-free grammars automatically.

We don’t get the same break with semantic analysis. Technically we do, but all programs are Turing Machines, and there’s no semantic definition from which we can generate the program. We must code the semantics ourselves.
The GoF mentioned generators briefly with:
> For complex grammars, the class hierarchy for the grammar becomes large and unmanageable. Tools such as __parser generators__ are a better alternative in such cases. They can interpret expressions without building abstract syntax trees, which can save space and possibly time.
> 
> Classes defining nodes in the abstract syntax tree have similar implementations. These classes are easy to write, and often their __generation can be automated with a compiler or parser generator__.
> 
> … grammars containing many rules can be hard to manage and maintain. Other design patterns can be applied to mitigate the problem. But when the grammar is very complex, other techniques such as __parser or compiler generators__ are more appropriate.

## Lex
[Lex](https://en.wikipedia.org/wiki/Lex_(software)) is a scanner generator.

## Yacc
[Yacc](https://en.wikipedia.org/wiki/Yacc) (Yet Another Compiler Compiler) is a parser generator.

## ANTLR
[ANTLR](https://www.antlr.org/) (ANother Tool for Language Recognition) is a parser generator.

## And More
If these don’t work in your environment, maybe one in this [Parser Generators List]( https://en.wikipedia.org/wiki/Category:Parser_generators) will work.

## Home Spun
Scanners and Parsers are so deterministic, that they aren’t too difficult to write your own if your DSL doesn’t get too large. From the GoF:
> The Interpreter pattern doesn’t explain how to _create_ an abstract syntax tree. In other words, it doesn’t address parsing. The abstract syntax tree can be created by a table-driven __parser__, by a __hand-crafted (usually recursive descent) parser__, or directly by the client.

I’ll be writing my own from scratch for the Rational Expression Evaluator example.

# Summary
If you made it this far, you have my respect. This can be some pretty dense material. It’s challenging to compress the main points of two challenging college courses into one blog without it getting a bit long.

Scanners and Parsers will allow us to convert DSL scripts/programs/specifications into objects in a Composite object tree. Their mathematical rigor may support their being auto-generated or we can write our own without much difficulty.

I had originally planned for this blog to include the scanner and parser discussion along with the implementation, but I’m well over 3,000 words for this entry, and it’s getting too large.

I’m going to wrap up this blog and continue with the Rational Expression Evaluator scanner and parser code in the next blog.

# References
See: [Interpreter Design Pattern Introduction/References](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html#references) for overall references.
