---
title: Interpreter Design Pattern – Grammar to Design
description: The Interpreter Design emerges from the Grammar.
unlisted: true
---
 
# Introduction
This blog continues the [Interpreter Design Pattern](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) series by expanding upon how the Interpreter Design emerges from the Grammar, which I introduced in [Interpreter Design Pattern – Grammar to Design]( https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html#grammar-to-design).

This is the blog in the Interpreter series that highlights the elegance of the Interpreter design pattern. This is where the magic happens.

# Design for the Interpreter Design Pattern by the Gang of Four
Let’s return the Gang of Four’s (GoF) words about Interpreter. Hopefully this paragraph makes a bit more sense than when I posted it a few blogs ago:
> If a particular kind of problem occurs often enough, then it might be worthwhile to express instances of the problem as sentences in a simple language. Then you can build an interpreter that solves the problem by interpreting these sentences.

I’ve written about the _kind of problem_ and _simple language_ as [Domain-Specific Languages]( https://jhumelsine.github.io/2024/03/18/interpreter-design-pattern-dsls.html) (DSL). My previous blog, [Interpreter Design Pattern – Grammars](https://jhumelsine.github.io/2024/04/02/interpreter-design-pattern-grammars.html), focused upon defining a grammar for a DSL. This blog will focus upon creating a class design that supports the grammar.

The GoF’s Interpreter Class Design is the following:
https://commons.wikimedia.org/wiki/File:Diagram_UML_klasy_Interpreter.svg
https://upload.wikimedia.org/wikipedia/commons/thumb/e/e2/Diagram_UML_klasy_Interpreter.svg/1280px-Diagram_UML_klasy_Interpreter.svg.png
 
Their design is more of a template for Interpreter rather than a complete design. Compare this design to the [Composite Design Pattern](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html), and you’ll notice that it is structurally identical, but with the addition of __Context__. __Context__ allows us to inject additional information. In the [Specification Smart Playlist Use Case](https://jhumelsine.github.io/2024/03/07/specification-design-pattern-use-case.html), the _Track_ was the injected __Context__. Different tracks with different attribute values would result in different specification matching results.

The GoF’s AbstractExpression, __TerminalExpression__ and __NonterminalExpression__ elements in the diagram suggests only one type of each. In practice, there will be many interfaces and classes of each type. There will be more domain specific relationships among these elements.

The GoF’s diagram is missing much of the richness and elegance of the pattern. They hinted at the richness and elegance in their Regular Expression Interpreter example, but they didn’t follow through.

The GoF provide a grammar for their Regular Expression Interpreter example without providing any details as to how they created the grammar:
```
expression ::= literal | alternation | sequence | repetition | ‘(‘ expression ‘)’
alternation ::= expression ‘|’ expression
sequence ::= expression ‘&’ expression
repetition ::= expression ‘*’
literal ::= ‘a’ | ‘b’ | ‘c’ | … { ‘a’ | ‘b’ | ‘c’ | … }*
```

They provide some description with the following:
>  The symbol __expression__ is the start symbol, and __literal__ is a terminal symbol defining simple words.

> The Interpreter pattern uses a class to represent each grammar rule. Symbols on the right-hand side of the rule are instance variables of these classes. The grammar above is represented by five classes: an abstract class RegularExpression and its four subclasses LiteralExpression, AlternationExpression, SequenceExpression, and RepetitionExpression. The last three classes define variables that hold subexpressions.
Then they added this diagram (provide my own):
 
Their class names don’t match the grammar terminology. They dropped the _’(‘ expression ‘)’_ grammar rule as well.

# What the Gang of Four Skimmed Over
The GoF’s description of grammar mapping to design in their example is correct, but it is sparse. Here is my interpretation of the mapping from grammar to design:
* Each grammar rule will tend to be an interface or a class.
* Grammar rules based upon an __OR__ (__|__) definition will be interfaces and the __OR__ed elements listed in that rule will implement it. This is an IS-A relationship.
* Grammar rules based upon an __AND__ definition will be classes. They will contain the elements listed in that rule. This is a HAS-A relationship.
* Explicit token elements, such as ‘(‘ or ‘)’ or keywords are used for parsing, but they won’t be in the design. The token elements are road signs that help the parser identify where rules start and end and verify that parsing is on track.

Here is where the elegance emerges:
* Once you have some grammar elements, even if the grammar is not complete, the initial design emerges based upon the rules above.
* This is an initial design, and additional refactoring may be desired.
* The grammar inspired design will adhere to good OO design principles by default, such as:
    * The design will be modular.
    * Each interface and class will be relatively small and manageable because it will only focus upon a single responsibility.
    * Classes will tend to depend upon interfaces, not other classes. And those dependencies will be limited in number.
    * There will not be any circular dependencies.
    * Each class can be easily unit tested.
* The design is maintainable. Due to the modular nature of the design with interface separation, updates to existing grammar rules and newly added grammar rules and their corresponding design components will tend to have minimal impact upon existing grammar rules and their design components.

# Grammar to Design via Use Case
The following sections will show how the Rational Expression Evaluation Grammar leads to a design.

## Grammar 
I introduced the [Rational Expression Evaluation Grammar Rules](https://jhumelsine.github.io/2024/04/02/interpreter-design-pattern-grammars.html#rational-expression-evaluation-dsl-grammar-rules) in the previous blog.

Here’s a copy with OR/AND and IS-A/HAS-A Relationship comments:
```
Statement ::= Assignment | Expression // OR Rule/IS-A Relationship
Assignment ::= Identifier = Expression // AND Rule/HAS-A Relationship
Expression ::= Identifier | Rational | AddOperation | SubtractOperation | MultipleOperation | DivideOperation // OR Rule/IS-A Relationship
Identifier ::= AlphaNumbericValue // Neither AND or OR relationship. More on this later.
Rational ::= Integer | Fraction | MixedFraction // OR Rule/IS-A Relationship
Fraction ::=  Integer / Integer // AND Rule/HAS-A Relationship
MixedFraction ::= Integer Integer / Integer // AND Rule/HAS-A Relationship
AddOperation ::= + ( ExpressionList ) // AND Rule/HAS-A Relationship
SubtractOperation ::= - ( Expression, Expression) // AND Rule/HAS-A Relationship
MultiplyOperation ::= * ( ExpressionList ) // AND Rule/HAS-A Relationship
DivideOperation ::= / ( Expression, Expression ) // AND Rule/HAS-A Relationship
ExpressionList ::= Expression [, Expression]* // AND Rule/HAS-A Relationship
```

## Design
The grammar rules will generate the first design. I have provided several design refactoring considerations as well.

### First Design
The following is a literal application of the mapping based upon the grammar rules above:
 
I started with the first rule, __Statement__, and I mapped the remaining grammar definitions into interfaces and classes one by one. It took only a few minutes, and most of that time involved organizing the graphic elements for presentation.

Recall that the implements triangle represents the IS-A relationship. The contains diamond represent the HAS-A relationship.

### Second Design
While this is the start of the design, it might not be the end of the design. There can be multiple grammars for the same DSL, and some may lead to a better design than others.

This is your DSL. Modify the grammar or the design as needed. You may be able to make enhancements to the grammar or design that you might have the freedom to do with a General-Purpose Language.

I haven’t started any implementation yet, but I’ve been thinking about how I might implement some of these classes. While the __Rational__ grammar rule made it easy to document the three forms of a rational number:
* Integer as: _Integer_
* Fraction as: _Integer / Integer_
* Mixed Fraction as: _Integer Integer / Integer_

I don’t know if I necessarily need a class for each. I think I can implement Rational as its own class that supports these concepts without requiring these additional classes.

This refactoring transforms __Rational__ from an interface to a class, and the derived classes disappear. The design is a bit smaller. I know that this refactoring update won’t affect the rest of the design, since nothing else in the design depends upon __Rational__ or its previous __Integer__/__Fraction__/__MixedFraction__ classes.
 
I’m planning for the parser to identify the tokens for all three forms of a __Rational__ and pass a String of those forms as a constructor argument to the __Rational__ class. The constructor will do additional specialized parsing on the three forms and construct the __Rational__ instance accordingly.

### Third Design
I’m not 100% pleased with the Add, Subtract, Multiple and Divide Op classes that popped out of the grammar based design. I’ve implemented designs like this before, and I know there are going to be redundancies in the implementations as designed above.

This refactoring adds two new classes __MultiOperandOp__ and __BinaryOp__. They are abstract classes for operations with multiple operands and two operands respectively. The specific Operation (Op) classes will implement them:
 
I may be jumping the gun on this refactoring. We generally want at least three repetitions before refactoring like this. But [The Rule of Three](https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)) is more of a guideline than a rule that must be obeyed. I anticipate that most of the implementation will reside in the abstract base classes with minimum implementation in the concrete classes. I also anticipate the concrete class implementations for each of these abstract classes will be based upon the [Template Method Design Pattern](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html).

I’m also adding this refactoring to show refactoring can add design elements that are not in the grammar. __MultiOperandsOp__ and __BinaryOp__ are not in the grammar, but they are design elements derived from the grammar.

Likewise, several grammar elements, such as __Integer__, __Fraction__, __MixedFraction__, and __ExpressionList__ have been removed from the design since I think they can be more easily implemented within other classes rather than as their own standalone class. For example, __ExpressionList__ is a List of some form. There’s no compelling reason for it to be its own class, when it can be a List of Expressions attribute declared within __MultiOperandOp__.

### Fourth Design
This fourth design idea won’t need a new diagram. It focuses upon the __Identifier__ concept in the design. Even the grammar is a bit vague for __Identifier__:
> Identifier ::= AlphaNumbericValue // Neither AND or OR relationship. More on this later.

__Identifier__ will be used for state. The DSL has the concept of a variable, and the __Identifier__ will be the handle for that variable. I anticipate that state will be stored in the __Context__ as a map of String to __Rational__. The __Identifier__ will be the key value for storing and retrieving state values from the __Context__.

The __Identifier__ class will have a String identifier value that will return the __Rational__ stored in the __Context__that contains that value.

The __Assignment__ class gets more interesting. While the grammar shows that it HAS-An __Identifier__, I don’t think __Assignment__ will contain an __Identifier__ attribute in the implementation. I think the __Assignment__ class will have a String identifier value that it will use as a key to store the __Rational__ that’s evaluated from its __Expression__.

These implementation considerations are only in my head. I haven’t spelled them all out here; therefore, my design refactoring intent may not be too clear. This should make more sense when I present the implementation of the classes in the design.

### Redesign Conclusion
Design refactoring is perfect time to do a lot of thinking and coming up with alternative ideas. Any change is simply a modification to a drawing tool. There’s almost zero sunk cost at this point.

If I find that my implementation ideas don’t work out, I can easily revert to a previous design. I may find that some of these design ideas are wrong and require modification. That still should not be a major issue since the design is modular and modifications in one area won’t tend to have much of a ripple effect upon the others.

# Design to Grammar
I’ve been presenting Interpreter as a process of __Domain => Domain-Specific Language => Grammar => Design => Implementation__. In practice, it won’t model this handoff process perfectly. The journey from Domain to Implementation may progress in bits, possibly with all stages having active work at the same time.

And the process is not necessarily be a one-way street. You might have a design first, and then you may want to see if it maps back to a grammar, even if you never intend to create a DSL. If a design doesn’t map to a grammar cleanly, then maybe the design might need some additional refinement.

The design-to-grammar exercise could be an additional means to confirm or clarify a design. Even if you don’t want to use grammar notation, transforming the design into sentences with IS-A and HAS-A phrasing will help indicate if it still makes sense.

# Summary
Grammar elements map to design elements resulting in a modular and well-structured design. I feel this is the most elegant aspect of the Interpreter Design Pattern, and the GoF didn’t give it enough attention.

Additional refactoring may be applied to the grammar-generated design, which it should accommodate well due to the design being modular and well-structured.

I feel the Interpreter Design Pattern is the apex of the [Design Pattern Principles](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html):
* Program to an interface, not an implementation
* Favor object composition over class inheritance
The next blog will feature the design’s implementation.

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* TBD

Here are some resources that can be purchased or are included in a subscription service:
* TBD

See: [Interpreter Design Pattern Introduction/References](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html#references).
