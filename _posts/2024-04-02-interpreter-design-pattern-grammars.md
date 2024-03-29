---
title: Interpreter Design Pattern – Grammars
description: Grammars define the structure rules for natural and programming languages
unlisted: true
---

# Introduction
This blog continues the [Interpreter Design Pattern](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) series by expanding upon Grammars, which I introduced in [Interpreter Design Pattern – Programming Language Grammar Primer Primer](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html#programming-language-grammar-primer).

Programming languages allow developers to communicate functional intent with computers and other developers. One could argue that communication with other developers is more important than with the computer. A computer would work just as well with machine language as it does with a higher-level programming language. Two quotes by Donald Knuth come to mind:
* _Programming is the art of telling another human being what one wants the computer to do._ ― Donald Knuth
* _Programs are meant to be read by humans and only incidentally for computers to execute._ ― Donald Knuth

Programming languages are bound by their grammar rules that document their syntax and structure.
If you already feel confident about Programming Language Grammers, then skim this blog or skip it completely.

__Here's a summary of the DSL concepts presented here:__
* __Natural Languages and Programming Languages are both described with grammars.__
* __People communicate thoughts with natural languages and programming intent with programming languages.__
* __Natural Languages emerge from social interactions. They contain ambiguities, irregular forms, etc.__
* __People learn the grammars of their native language intrinsically often knowing what's right and wrong by ear.__
* __Developers learn the grammars of their programming language intrinsically as well.__
* __Programming language grammars are based upon automata theory. They contain deterministic rigor.__
* __All programs can trace their structure to the grammars that define the language.__

# Natural Language Grammars
People have been communicating with each other via natural language long before we did so with programming languages. Natural languages and programming languages have a lot in common, but they have a lot of differences as well. Except for constructed languages, such as [Esperanto](https://en.wikipedia.org/wiki/Esperanto), natural languages emerge from cultural and social interaction adding warts and all. Natural languages require context, contain ambiguity, and are littered with irregular forms, such as the conjugation of the verb: _to be_.

Here are some aspects of natural languages, for comparison, which for the most part don’t exist in programming languages.

## Implicit/Internalized Grammars
All natural languages have grammar, but most of us don’t think about them too much. Most children are near experts of the first languages they learn by around age three or four, but none of these toddlers would be able to describe its grammar rules. They choose their words based upon whether they sound right or wrong, which is based upon countless hours of hearing the language from their parents, other family members, friends, etc. They have been in training mode for a very long time.

Children learn grammar in school. When I was in elementary school, we learned common English sentence grammars such as __Noun-Verb-Noun__ as in _John likes pizza_. As we progressed, we learned more grammar rules and parts of speech, such as prepositions, prepositional phrases, subordinate clauses, infinitives, gerunds, active/passive voice, etc.

We implicitly understood these grammar rules and parts of speech, mostly by ear, but in the classroom, we were learning the formal grammar.

Sometimes we never learned formal grammar rules. For example, all native English speakers would choose _big blue marble_ rather than _blue big marble_ in a phrase. If asked why, they would probably say that the first one sounds better.

<img src="https://upload.wikimedia.org/wikipedia/commons/e/e3/Adjective_Order.png?20120322054144" alt="Adjective Order Rules" title="Image Source: https://upload.wikimedia.org/wikipedia/commons/e/e3/Adjective_Order.png?20120322054144
" width = "40%" align="right" style="padding-right: 35px;">
 
But there is a [grammar rule for the order of adjectives in English](https://en.wikipedia.org/wiki/Adjective#Order) based upon the adjective’s denoting category. In that rule, size adjectives (i.e., __big__) come before color adjectives (i.e., __blue__).

I first saw this grammar rule a few years ago, well into my 50s. And yet, if I encountered a sentence that shuffled any of these adjective categories in a phrase, I suspect my ear would have flagged as sounding a bit odd. I don’t know if non-native English speakers are taught this grammar rule or whether they develop an ear for it as well.

We know the grammar rules of our native natural languages even if we really don’t _know_ them.

In contrast, when I learned Latin in secondary school, we were bombarded with grammar rules almost from the start. Latin’s grammar rules usually take form of tables of suffixes, which is a critical indicator of a word’s use in a Latin sentence. Word order isn’t as critical in Latin as it is with English. While I was able to memorize these myriad tables of suffixes, I never developed an _ear_, or _auris_, for the language.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/1/15/DECLINATIO_I%2C_II_-_NOMINA_ADIECTIVA.png/1200px-DECLINATIO_I%2C_II_-_NOMINA_ADIECTIVA.png?20211206185847" alt="Latin Grammar Tables" title="Image Source: https://commons.wikimedia.org/wiki/File:DECLINATIO_I,_II_-_NOMINA_ADIECTIVA.png" width = "100%" align="center" style="padding-right: 35px;">

## Natural Language Ambiguity
Natural languages have ambiguity which requires context for resolution. Consider the sentence: _Tom saw Jane with a telescope_. This has at least two possible interpretations:
* _Tom saw Jane __by means of__ a telescope_.
* _Tom saw Jane, __who had__ a telescope_.

We need context to resolve the ambiguity. However, a slightly different version of the same sentence has no ambiguity: _Galileo saw the moons of Jupiter with a telescope_.

Sometimes a clever phrase depends upon ambiguity, such as [Martin Fowler’s advice](https://www.goodreads.com/quotes/10516579-you-can-change-your-organization-or-change-your-organization) when in a difficult work environment:

_You can Change Your Organization, or you can Change Your Organization_. 

This repeats the same phrase twice, but with two different meanings: _Change how your Organization operates, or move to a new Organization_.

## Grammar Parsing Backtracking
Sometimes our interpretation is directed down the wrong path, and we must backtrack and start it again.

_Time flies like an arrow. Fruit flies like a banana. ― Groucho Marx_

These two sentences have identical structure, but they require different parsing. The two-word phrase: _flies like_ has two structures:
* _files like_ as a verb with simile.
* _files like_ as a noun and verb.

The first sentence frames our mind for the verb/simile parsing, and when we get to the second sentence, we don’t realize that’s it’s really the noun/verb parsing until we hit the last word. The verb/simile parsing no longer makes sense, and we need to reparse it mentally.

# Programming Language Grammars
Natural languages came first, and then their grammar rules came later. It’s like physicists observing natural phenomena and then creating models that formalize their observations. The constant evolution of natural language is what leads to ambiguity, irregular forms, backtracking, etc.

Programming languages must be deterministic. They can’t contain ambiguity, irregular forms, backtracking, etc. The behavior of every program must be clear and obvious. It would be disastrous if the same program exhibited different behaviors each time it’s executed. I know there are exceptions, such as random algorithms, concurrency issues and such. I’m more interested in language features that are intended to be deterministic.

Programming language grammars are defined using [context-free grammars](https://en.wikipedia.org/wiki/Context-free_grammar). Context-free grammars originate from [automata theory](https://en.wikipedia.org/wiki/Automata_theory) with the mathematical rigor that comes with the theory. [Pushdown automata](https://en.wikipedia.org/wiki/Pushdown_automaton) are the types of theoretical machines that accept context-free grammars.

Automata theory is indifferent to programming languages. But it turns out that program language designers can leverage that theory in practice. The theory provides a rigorous foundation upon which program languages can be constructed. For example, programming languages are defined using context-free grammars and parsers are implemented as pushdown automata. They are two sides of the same coin. This is why parsers can be autogenerated from grammars.

I mentioned the basic parts of programming language grammars in [Interpreter Design Pattern Introduction - Programming Language Grammar Primer](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html#programming-language-grammar-primer). For more details about grammars see:
* [Introduction to Programming Languages/Grammars](https://en.wikibooks.org/wiki/Introduction_to_Programming_Languages/Grammars) – Wikibook entry
* [Context-Free Grammar](https://en.wikipedia.org/wiki/Context-free_grammar) – Wikipedia entry
* [Context-Free Grammars](https://pages.cs.wisc.edu/~fischer/cs536.s08/course.hold/html/NOTES/3.CFG.html) – University of Wisconsin Computer Science Notes. It includes several practical examples including how to remove ambiguity arithmetic expression grammar and ensure the proper order of evaluation. 

<img src="https://upload.wikimedia.org/wikipedia/commons/e/e9/Delete_statement.gif?20170424062623" alt="Delete Railroad tracks" title="Image Source: https://commons.wikimedia.org/wiki/File:Delete_statement.gif" width = "40%" align="right" style="padding-right: 35px;">

* [Syntax Diagram](https://en.wikipedia.org/wiki/Syntax_diagram) – Wikipedia entry. Most grammars are defined via rules, but syntax diagrams provide a more visual presentation. They’re also called _railroad diagrams_, since they tend to resemble an ariel view of railroad tracks in a railroad depot.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/42/Rail_Tracks_map_JR-E_Ikebukuro_Station.svg/1200px-Rail_Tracks_map_JR-E_Ikebukuro_Station.svg.png" alt="Railroad Tracks in Japan" title="Image Source: https://commons.wikimedia.org/wiki/File:Rail_Tracks_map_JR-E_Ikebukuro_Station.svg" width = "40%" align="center" style="padding-right: 35px;">

## Programs Emerge from Grammars
Grammar rules are more than just syntax. They define the semantic framework structure of a program by building a unique parse tree that matches the program. The non-terminal nodes in the tree will be grammar rules. The leaf nodes will be the tokens in the program. For example, if given this program:
```java
A = B + C * 2;
D = 1
```
The constructed parse tree could be:

<img src="https://upload.wikimedia.org/wikipedia/en/thumb/f/fc/Parse_Tree_Example.svg/768px-Parse_Tree_Example.svg.png" alt="Delete Railroad tracks" title="Image Source: https://en.wikipedia.org/wiki/File:Parse_Tree_Example.svg" width = "40%" align="center" style="padding-right: 35px;">
 
Technically in the theory, we don’t generate the tree from the program, even if that’s what happens in practice with the parser. In theory, we start with an initial rule, such as __statements__ and expand it one rule at a time growing the tree from its root until each non-terminal node has been expanded to a leaf node.

__statements__ would expand to __stmt ; stmt__. Then each __stmt__ expands to __assign__, and so on. All programs that can be expressed with the grammar rules can be created by expanding from the root.

The grammars for General-Purpose Languages (GPL) are surprisingly small considering that every possible program that can be written can be generated uniquely from the root. Here are a few GPL grammar rules:
* [Java Syntax Specification](https://cs.au.dk/~amoeller/RegAut/JavaBNF.html) – Aarhus University Department of Computer Science
* [Java Syntax](https://docs.oracle.com/javase/specs/jls/se7/html/jls-18.html) – Oracle
* [Python 3.0 Syntax](https://docs.python.org/3/reference/grammar.html) – Python.org documentation
* [Python 2.5 Syntax](https://inst.eecs.berkeley.edu/~cs164/sp11/python-grammar.html) – University of California – Berkley Electrical Engineering and Computer Science Department
* [Ruby Syntax]( https://ruby-doc.org/docs/ruby-doc-bundle/Manual/man-1.4/syntax.html) – ruby-doc.org

## A Developer’s Notion of Programming Language Grammars
I suspect that most developers know just enough grammar of a programming language to get by. They don’t know the entire set of grammar rules, and I suspect they internalize what they do know. It’s like people who don’t know all the grammar rules of their first natural language, but they can _know_ them by ear, such as the grammar rule for adjective order, which I mentioned above.

Most if not all programming languages have the concept of an __if__ statement with an optional __else__ clause. Most developers learned this concept in their earliest programming experience. If like me, they saw a few examples, internalized it, and then could apply it almost universally. And if we made a syntax or structural mistake, then the compiler or IDE would correct us almost immediately.

I doubt that anyone thinks of the __if__ and __else__ Java grammar rules from [Aarhus University Department of Computer Science](https://cs.au.dk/~amoeller/RegAut/JavaBNF.html), as:
```
<if then statement>::= if ( <expression> ) <statement>
<if then else statement>::= if ( <expression> ) <statement no short if> else <statement>
<if then else statement no short if> ::= if ( <expression> ) <statement no short if> else <statement no short if>
```

If anything, we tend to think of the __if__ and __else__ Java grammar rules more like this version from [Oracle](https://docs.oracle.com/javase/specs/jls/se7/html/jls-18.html):
```
Statement ::= if ParExpression Statement [else Statement] // Snippet of one Statement of the set.
ParExpression: ( Expression )
```

# Rational Number Evaluator Grammer Use Case
DSL grammars tend to be smaller than GPL grammars. Let’s see some examples using the [Rational Number Evaluator Use Case](https://jhumelsine.github.io/2024/03/18/interpreter-design-pattern-dsls.html#rational-number-evaluator-use-case).

## A Few Examples
I’d like to get a feel for what the Rational Number Evaluator might look like in use first, and then I’ll come up with the grammar rules. Each line will represent a single expression that will be evaluated:
```java
2                 // Should evaluate to 2
2 5/6             // Should evaluate to 2 5/6
2 6/8             // Should evaluate to 2 3/4
4 10/4            // Should evaluate to 6 1/2
-1                // Should evaluate to -1
5/7               // Should evaluate to 5/7
7/5               // Should evaluate to 1 2/5
+(2, 3)           // Should evaluate to 5. Note: Using prefix notation. More on this shortly.
-(2, 3)           // Should evaluate to -1
*(3 1/3, 5 5/7)   // Should evaluate to 19 1/21
/(3 1/3, 5 5/7)   // Should evaluate to 7/12
*(2, 3)           // Should evaluate to 6
*(-2, 3)          // Should evaluate to -6
*(2, -3)          // Should evaluate to -6
*(-2, -3)         // Should evaluate to 6
a = 3             // Should evaluate to 3
+(a, 5)           // Should evaluate to 8
+(3, *(2, 6), 5)  // Should evaluate 20
```

I’m using prefix notation for arithmetic operations for at least two reasons:
* It’s easier to parse, which I’ll get to in a future blog. Arithmetic operations are infix only because that’s been the tradition for arithmetic symbols for centuries. For an example of how to define a grammar for infix operations see: [University of Wisconsin CS 536 Notes](https://pages.cs.wisc.edu/~fischer/cs536.s08/course.hold/html/NOTES/3.CFG.html).
* It’s different than most languages, so it shows a different way to do something.

## Rational Expression Evaluation DSL Grammar Rules
Here are what I think are the Rational Expression Evaluation DSL Grammar Rules:
```
Statement ::= Assignment | Expression
AssignmentStatement ::= Identifier = Expression
Expression ::= Identifier | Rational | AddOperation | SubtractOperation | MultipleOperation | DivideOperation
Identifier ::= AlphaNumbericValue // Must start with a letter.
Rational ::= Integer | Fraction | MixedFraction
Fraction ::=  Integer / Integer // Whitespace on either side of slash is optional.
MixedFraction ::= Integer Integer / Integer // Whitespace between two first Integers is mandatory. Whitespace on either side of slash is optional.
AddOperation ::= + ( ExpressionList ) // Whitespace is optional.
SubtractOperation ::= - ( Expression, Expression) // Whitespace is optional.
MultiplyOperation ::= * ( ExpressionList ) // Whitespace is optional.
DivideOperation ::= / ( Expression, Expression ) // Whitespace is optional.
ExpressionList ::= Expression [, Expression]* // Whitespace is optional. Star is any number of repeating Comma/Expression extensions.
```

Notice that the arithmetic operations take Expressions as operands. That will allow us to nest expressions.

These rules will be the basis of my upcoming design and implementation. As that continues, I may have to return and tweak it a bit.

# Summary

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* TBD
Here are some resources that can be purchased or are included in a subscription service:
* TBD
