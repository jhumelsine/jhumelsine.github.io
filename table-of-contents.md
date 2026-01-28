---
title: Table of Contents
description: Organizing various blog entries by common themes
---
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4b/Long_Room_Interior%2C_Trinity_College_Dublin%2C_Ireland_-_Diliff.jpg/1017px-Long_Room_Interior%2C_Trinity_College_Dublin%2C_Ireland_-_Diliff.jpg" alt="Trinity College Library" title="Image Source: https://commons.wikimedia.org/wiki/File:Long_Room_Interior,_Trinity_College_Dublin,_Ireland_-_Diliff.jpg" width = "50%" align="center" style="padding-right: 20px;">

# Introduction
I began this blog in earnest as a retirement project in the fall of 2023. The blog's [home page](https://jhumelsine.github.io/) lists the entries chronologically from newest to oldest. Blog entries are often part of a series focused upon a theme. The first blog entry of a series is usually an introduction to the series and acts as a landing page to the other blogs in the series. There are crossover reference links to other blog entries as well.

This meta-blog entry organizes the themes of my blog entries. __NOTE:__ This page will be updated as new blog entries are added to existing themes and new themes are introduced.

# Table of Contents
* [AI Notebooks](#ai-notebooks)
* [Software Engineering](#software-engineering)
    * [Abstraction](#abstraction)
    * [Miscellaneous](#se-miscellaneous) 
* [Design Patterns](#design-patterns)
    * [Design Pattern Foundations](#design-pattern-foundations)
    * [Essential Design Patterns](#essential-design-patterns)
    * [Creational Design Patterns](#creational-design-patterns)
    * [Composable Design Patterns](#composable-design-patterns) 
* [Hexagonal Architecture AKA Ports and Adapters](#hexagonal-architecture-aka-ports-and-adapters-design)
* [Automated Testing](#automated-testing)
* [Large Language Models and Generative AI](#large-language-models-and-generative-ai)
* [True Stories](#true-stories)
* [Miscellaneous](#miscellaneous)
* [Complete Code Demos](#complete-code-demos)
* [Blog Statistics](#statistics)

# AI Notebooks
I have created AI/Google Notebooks for several of my blog series. Each provides an AI interactive experience with some of my central themes. There are several artifacts including an AI generated two-host podcast formatted summary. You can also interact with the generated AI hosts to some degree.

__NOTE:__ I think that a Google account may be required to access the AI/Google Notebooks.

AI/Google Notebooks:
* [Abstraction](https://notebooklm.google.com/notebook/94884f00-c719-471f-87f6-9225b67922be?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)
* [Design Pattern Foundations](https://notebooklm.google.com/notebook/ec8ed593-cd93-4a65-8615-2233bef3d910?_gl=1*u5eebw*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzAkdDE3NDk1NjA5MjQkajYwJGwwJGgw)
* [Creational Design Patterns](https://notebooklm.google.com/notebook/8453e55f-55b0-4995-9ca2-5eaca6eef48c)
* [Essential Design Patterns](https://notebooklm.google.com/notebook/a4db6342-c117-4664-b0ed-1aa79d720db7?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)
* [Composable Design Patterns](https://notebooklm.google.com/notebook/bc19b872-144e-4218-8622-023e963becdf?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)
* [Interpreter Design Pattern](https://notebooklm.google.com/notebook/452441d7-f9d9-4a31-9084-a02621271099?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)
* [Hexagonal Architecture AKA Ports and Adapters](https://notebooklm.google.com/notebook/224c31b5-fac9-441a-9886-8ed071bf1d52?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)
* [Automated Testing](https://notebooklm.google.com/notebook/cdead562-a40d-4f5c-84cb-4368812f4347?_gl=1*uicl85*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDg2Mjk1NzckbzMkZzAkdDE3NDg2Mjk1NzckajYwJGwwJGgw)
* [Large Language Models and Generative AI](https://notebooklm.google.com/notebook/36b181b7-07b1-443e-86d3-038adc3400ed?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)

# Software Engineering
A degree in Computer Science has been the initial exposure to software for many developers; however, Computer Science is not the same as Software Engineering.

## Abstraction
Abstraction is a core software engineering concept, and it's more challenging to describe abstraction concretely than I originally thought.

* [What is Abstraction?](https://jhumelsine.github.io/2024/10/30/abstraction.html) - An introduction to Abstraction
* [Cohesion and Coupling](https://jhumelsine.github.io/2024/11/07/cohesion-coupling.html) - My first attempt to describe Cohesion and Coupling
* [Cohesion and Coupling - Take 2](https://jhumelsine.github.io/2024/11/22/coupling-and-cohesion-2.html) - My second attempt to describe Cohesion and Coupling
* [What is Cohesive Construction?](https://jhumelsine.github.io/2024/11/27/abstraction-cohesion.html) - How to configure cohesive abstractions consistently
* [Getting the Right Abstraction is Hard](https://jhumelsine.github.io/2023/09/22/right-abstraction-is-hard.html) - How I adjusted the abstraction in a design I inherited

See: [Abstraction AI Agent](https://notebooklm.google.com/notebook/94884f00-c719-471f-87f6-9225b67922be?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)

## SE Miscellaneous
* [Bumper Sticker Computer Science and Software Engineering](https://jhumelsine.github.io/2023/12/15/bumper-sticker-computer-science-software-engineering.html) - I think this is my favorite entry.
* [My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html) - Everyone has their own way to approach design problems. This entry describes mine.

# Design Patterns
Design Patterns changed how I approach software. I feel that every Software Engineer should have some degree of competence and comfort with design patterns.

**NOTE**: Some patterns appear in multiple groups.

## Design Pattern Foundations
Design Patterns are not technically challenging, but they do require a slight paradigm shift in thinking. These blog entries lay some foundations toward that shift in thinking.

* [Are you a cook or a chef?](https://jhumelsine.github.io/2023/08/21/knock-knock-whos-there.html) - Are you following the recipe or creating something new?
* [Rules of the Game](https://jhumelsine.github.io/2023/08/24/its-your-move.html) - Do you know more than just the rules of the game? Do you know strategy and tactics to win the game?
* [Problem Solvers](https://jhumelsine.github.io/2023/08/26/problem-solvers.html) - We solve problems, usually in the form of code
* [Design Patterns are Everywhere](https://jhumelsine.github.io/2023/08/28/wheels.html) - Design Patterns are not limited to Software Engineering. They are ubiquitous in most domains.
* [Toolbox](https://jhumelsine.github.io/2023/08/29/toolbox.html) - Design Patterns are tools in your toolbox. Like most tools, we have mastered them when we not only know when to use them, but when not to use them.
* [Parts is Parts](https://jhumelsine.github.io/2023/09/01/parts-is-parts.html) - Most Object-Oriented Design Patterns are different combinations of the same set of OO constructs
* [Design Pattern Principles](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html) - There are only two Design Pattern Principles. Understanding them will help in understanding the underlying structures that reappear in Design Patterns.

See: [Design Pattern Foundations AI Agent](https://notebooklm.google.com/notebook/ec8ed593-cd93-4a65-8615-2233bef3d910?_gl=1*u5eebw*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzAkdDE3NDk1NjA5MjQkajYwJGwwJGgw)

## Essential Design Patterns
The Seven [Essential Design Patterns](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html) are the design patterns that will be used repeatedly in designs. They are the hammer and screwdriver of a software developers design pattern toolbox.

* [Command](https://jhumelsine.github.io/2023/09/18/command-design-pattern.html) - Objectify a function so that it becomes a first-class citizen in an Object-Oriented paradigm
* [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) - A variant of Command with multiple implementations for an interface
* [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) - Similar to Strategy, except the interface is replaced with an abstract base class, which houses the implementation for common behavior that applies in all cases.
* [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) - A translation helper class that allows other classes to interact indirectly when they cannot interact directly due to different protocols and/or method signatures.
* [Façade](https://jhumelsine.github.io/2023/10/03/facade-design-pattern.html) - Similar to Adapter in concept, but rather than performing basic translation, it shields the client application of its dependency upon a particularly nasty or complex class or set of classes
* [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) - Allows an application to acquire an object reference without knowing class type and without having to call a `new()` constructor directly
* [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) - Removes all dependency resolution from an application by transfering that responsibility to another entity whose sole responsibility is to configure and resolve object references for the application

See: [Essential Design Patterns AI Agent](https://notebooklm.google.com/notebook/a4db6342-c117-4664-b0ed-1aa79d720db7?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)

## Creational Design Patterns
The [Creational Design Patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html) define techniques to acquire objects while encapsulating the object's concrete class type. They also provide object management techniques when a new object is not always needed.

* [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) – Allows an application to acquire an object reference without knowing class type and without having to call a `new()` constructor directly
* [Abstract Factory](https://jhumelsine.github.io/2025/07/30/abstract-factory.html) – Applications often have multiple dependencies often resolved via a Factory. It's critical for these factory dependencies to be consistent within their environment. For example, resolving dependencies with some factories producing production references while other factories produce test references could lead to disaster. Abstract factory helps ensure that a set of factories return consistent depenency references.
* [Builder](https://jhumelsine.github.io/2025/08/08/builder-introduction.html) – Creates, assembles and returns a set of cohesive objects based upon a plan much like a blueprint. Builder is often used with [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) to create and assemble the objects in the composition. The Builder pattern supports abstraction as well since different concrete Builders can resolve the abstraction within the same design allowing them to create and assemble different concrete instances of the same structure.
* [Singleton](https://jhumelsine.github.io/2025/10/31/singleton.html) – Ensures that __only__ one object is created for a class type
* [Flyweight](https://jhumelsine.github.io/2025/11/14/flyweight.html) (Also Known As _Multiton_) – Ensures that only one instance of an object will be created for a given set of intrinsic attributes
* [Object Pool](https://jhumelsine.github.io/2025/11/28/object-pool.html) – Objects are created at start up and lent to the caller when requested
* [Prototype](https://jhumelsine.github.io/2025/12/23/prototype.html) – Acquires a new object instance from an exsting object instance
* [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) – Removes all dependency resolution from an application by transfering that responsibility to another entity whose sole responsibility is to configure and resolve object references for the application

## Composable Design Patterns
The [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) focus upon behavior emerging from the composition of objects and their interaction in a structured formation rather than implemented within an individual class.

* [Proxy](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html) - Place administrative wrapper objects around objects often to help manage their complexity or resources
* [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) -Layer additional behaviors upon core features
* [Chain of Responsibility](https://jhumelsine.github.io/2024/02/20/chain-of-responsibility-design-pattern.html) - Delegate a request through a linked chain of handlers until one of the handlers can complete the request
* [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) - Configure behavior emerging from a group of snippet behavior objects organized in a tree structure
* [Specification](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html) – Allow a Client to select or filter objects with specific attribute property values as specified by the Client
* [Interpreter](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) – Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language. This pattern will require several blog entries.

See: [Composable Design Patterns AI Agent](https://notebooklm.google.com/notebook/bc19b872-144e-4218-8622-023e963becdf?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)

### Interpreter Design Pattern
The [Interpreter](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) required its own series.

Interpreter allows software developers to design and implement their own Domain-Specific Languages. Interpreter is not just the pinnacle of Composable Design Patterns, but I feel it shares the top spot of all design patterns along with the Visitor Design Pattern.

Interpreter is not a difficult pattern to design or implement, but it's a difficult pattern comprehend. Interpreter required multiple blog entries to do it justice. 

* [Interpreter](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) - Introduction and overview
* [Domain-Specific Languages](https://jhumelsine.github.io/2024/03/18/interpreter-design-pattern-dsls.html) - Domain-Specific Languages solve domain-specific problems
* [Programming Language Grammars](https://jhumelsine.github.io/2024/04/02/interpreter-design-pattern-grammars.html) - Grammars define the structure rules for natural and programming languages
* [Grammars to Design](https://jhumelsine.github.io/2024/04/07/interpreter-design-pattern-design.html) - The Interpreter Design emerges from the Grammar
* [Design to Implementation](https://jhumelsine.github.io/2024/04/17/interpreter-design-pattern-implementation.html) - Implementing the Interpreter Design Pattern based upon a Design.Implementing the Interpreter Design Pattern based upon a Design
* [Scanners and Parsers, Theory and Practice](https://jhumelsine.github.io/2024/04/25/interpreter-design-pattern-parser.html) - The basics of Scanner and Parser theory and practice
* [Scanners and Parsers, Implementation: Rational Expression Implementation Example Completed](https://jhumelsine.github.io/2024/04/30/interpreter-design-pattern-parser-implementation.html) - Implementing the Scanner and Parser for the Rational Expression Evaluator Use Case
* [Interpreter Design Pattern – Production Example: My Experience Using Interpreter on a Work Project](https://jhumelsine.github.io/2024/05/14/interpreter-design-pattern-production.html) - My Experience Using Interpreter on a Work Project

See: [Interpreter Design Pattern AI Agent](https://notebooklm.google.com/notebook/452441d7-f9d9-4a31-9084-a02621271099?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)

# Hexagonal Architecture AKA Ports and Adapters Design
The [Hexagonal Architecture (AKA Ports and Adapters)](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html) is a design that all software developers should know.
It showcases how different [Essential Design Patterns](#design-patterns) can work as a cohesive design.

* [Hexagonal Architecture Introduction](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html)  - Introduces Hexagonal Architecture
* [Hexagonal Architecture - Structure](https://jhumelsine.github.io/2023/10/28/hexagonal-architecture-structure.html) - This describes the structure of the Hexagonal Architecture design, and how it's really a pattern or design patterns
* [Hexagonal Architecture - Why it works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html) - This describes why I feel that the Hexagonal Architecture design works so well. It's based upon dependency and knowledge management. I feel that dependency and knowledge management applies to all OO based designs, and not just within the context of Hexagonal Architecture.
* [Hexagonal Architecture - How it compares and contrasts with Clean Architecture](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html) - The two designs are mostly the same, but there are some differences worth pointing out
* [Hexagonal Architecture - Adapter Flexibility](https://jhumelsine.github.io/2023/11/30/hexagonal-architecture-adapter-flexibility.html) - We have quite a bit of flexibility with this design in the Adapter layer

See: [Hexagonal Architecture AKA Ports and Adapters AI Agent](https://notebooklm.google.com/notebook/224c31b5-fac9-441a-9886-8ed071bf1d52?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)

# Automated Testing
I didn't appreciate automated testing until late in my career. My automated testing series begins with my [Conversion](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html) from automated testing non-believer to almost a zealot. The series grew larger than I imagined. I ended up writing a short book on the topic.

Here is the complete Automated Test series:
* [The Conversion of a Unit Test Denier or … How I Learned to Stop Worrying and Love Unit Testing](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html) - This is a summary of my journey from avoidance to introduction to acceptance of automated testing
* [Attributes of Effective Unit Tests](https://jhumelsine.github.io/2024/06/14/unit-test-attributes.html) - Unit Test properties that make them more useful than not
* [Basic Elements of Automated Unit Tests](https://jhumelsine.github.io/2024/06/23/unit-test-elements.html) - Elevating automated tests to first-class citizen status
* [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html) - Emulate dependencies without depending upon dependencies
* [Suril, the Semaphore and Me](https://jhumelsine.github.io/2024/07/08/suril-semaphore.html) - When the theory became practice for me
* [Test-Driven Development - Writing Tests Before the Implementation](https://jhumelsine.github.io/2024/07/15/tdd.html) - I know it sounds completely backwards, but please give it some consideration
* [Yuri, the Programming Assignment and Me](https://jhumelsine.github.io/2024/07/22/yuri-evaluator.html) - My evening introducing [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html) to a young Computer Science student
* [Be On Your Best Behavior](https://jhumelsine.github.io/2024/08/08/bdd.html) - Test-Driven Development - How to create tests; Behavior-Driven Development - What tests to create
* [What is Behavior in Behavior-Driven Development](https://jhumelsine.github.io/2024/08/20/behavior.html) - More details about the nature of behavior
* [Testing Benefits](https://jhumelsine.github.io/2024/08/30/test-benefits.html) - Spoiler Alert – It’s not really about testing the code
* [Testing Concerns](https://jhumelsine.github.io/2024/09/08/test-concerns.html) - Test concerns may be a result of previous bad test experiences; there are ways to accommodate them
* [A House Divided Against Itself Will Not Stand](https://jhumelsine.github.io/2025/03/07/house-divided.html) - Resolving the conflict between [My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html) and [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) blog entries
* [Working Effectively with Legacy Code](https://jhumelsine.github.io/2025/03/24/legacy-code.html) - How to leverage automated test techniques when working with Legacy Code
* [How do you know if your test code is really testing your code?](https://jhumelsine.github.io/2025/03/28/mutation-testing.html) - How Mutation Testing can help test your test suite
* [Approval Testing - A Test Strategy for those who are reluctant to try Test-Driven Development](https://jhumelsine.github.io/2025/04/02/approval-testing.html) - A testint strategy that's in the intersection of Unit Testing and Characterization Testing
* [Mastering Time in Software Testing - Strategies for Temporal Behavior Verification](https://jhumelsine.github.io/2025/04/08/time-lord.html) - Become a Time Lord and control time in your tests
* [Humble Objects - Designing Code You Don’t Hate Testing](https://jhumelsine.github.io/2025/04/14/humble-object.html) - A pattern to help test code that's difficult to test
* [Test Layers: From Unit to System - Building Confidence with the Right Tests at the Right Level](https://jhumelsine.github.io/2025/06/23/test-layers.html) - There's more to testing than Unit Tests
* [Consumer-Driven Contract Testing - Giving the consumer exactly what they asked for](https://jhumelsine.github.io/2025/07/14/cdc-testing.html) - When the Consumer supplies the Test Specifications to its Provider

See: [Automated Testing AI Agent](https://notebooklm.google.com/notebook/cdead562-a40d-4f5c-84cb-4368812f4347?_gl=1*uicl85*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDg2Mjk1NzckbzMkZzAkdDE3NDg2Mjk1NzckajYwJGwwJGgw)

# Large Language Models and Generative AI
AI has become a major topic since late 2022 with the introduction of ChatGPT among others. I learn a bit more about how to use them almost daily.

See: [Large Language Models and Generative AI AI Agent](https://notebooklm.google.com/notebook/36b181b7-07b1-443e-86d3-038adc3400ed?_gl=1*1dhb7x1*_ga*NjM2MjkwMzI5LjE3NDg1MzE4MjM.*_ga_W0LDH41ZCB*czE3NDk1NjA5MjQkbzUkZzEkdDE3NDk1NjEzMjQkajYwJGwwJGgw)

## LLM/GenAI Introduction
[LLM/GenAI Introduction](https://jhumelsine.github.io/2024/09/18/llm-genai.html) - Like it or not, the AI Genie is out of the  bottle

## Prompt Engineering Patterns
Prompt Engineering Patterns are the key to getting the best results from Generative AI. These blogs present a few patterns I learned via online courses.

* [Prompt Engineering Patterns 1](https://jhumelsine.github.io/2024/09/25/prompt-engineering-patterns1.html)
* [Prompt Engineering Patterns 2](https://jhumelsine.github.io/2024/10/18/prompt-engineering-patterns2.html)

# True Stories
The following blog entries feature a few __True Stories__, or maybe they should be called: __True Confessions__. I try to include personal experiences where I can in my blog entries. Sometimes story is the entire blog entry. Sometimes it's just a part of the blog entry.

I keep my previous employers and others anonymous. If I do mention someone by name, I only use their first name, and I get their permission, or I use a pseudonym.

* [What is UML and why you may want to care](https://jhumelsine.github.io/2021/06/22/uml.html) - When [I realized](https://jhumelsine.github.io/2021/06/22/uml.html#when-i-realized-how-to-use-them), in an ___Aha!___ moment, that OMT, the predecessor of UML, is more than just a design tool. It's a way to model the problem that based upon the domain elements and their relationships
* [It's Your Move](https://jhumelsine.github.io/2023/08/24/its-your-move.html) - When I realized that software engineering was more than just knowing the rules of a programming language
* [Getting the Right Abstraction is Hard](https://jhumelsine.github.io/2023/09/22/right-abstraction-is-hard.html) - Where I had one of my [first experiences](https://jhumelsine.github.io/2023/09/22/right-abstraction-is-hard.html#fiber-optic-switch) of working with someone else's code and realizing that it could be designed better
* [Façade Design Pattern](https://jhumelsine.github.io/2023/10/03/facade-design-pattern.html) - Where I encapsulated a really [nasty API](https://jhumelsine.github.io/2023/10/03/facade-design-pattern.html#i-have-to-implement-how-many-classes) away from the rest of my codebase
* [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) - Where tightly coupled code limited my team's testing abilities
* [Hexagonal Architecture – Adapter Flexibility](https://jhumelsine.github.io/2023/11/30/hexagonal-architecture-adapter-flexibility.html) - Where adopting [Hexagonal Architecture and an Adapter Dispatcher](https://jhumelsine.github.io/2023/11/30/hexagonal-architecture-adapter-flexibility.html#inspired-by-true-events) provided the flexibility we needed to solve a rather challenging design problem
* [Chain of Responsibility Design Pattern](https://jhumelsine.github.io/2024/02/20/chain-of-responsibility-design-pattern.html) - Where a Chain of Responsibility design solved a problem of different configurations of [Address Books](https://jhumelsine.github.io/2024/02/20/chain-of-responsibility-design-pattern.html#use-case--address-book).
* [Interpreter Design Pattern - Domain-Specific Languages (DSLs)](https://jhumelsine.github.io/2024/03/18/interpreter-design-pattern-dsls.html#my-domain-specific-language-experiences) - Where I briefly summarize a few DSL experiences in my career.
* [Interpreter Design Pattern - Parser and Scanner Theory and Practice](https://jhumelsine.github.io/2024/04/25/interpreter-design-pattern-parser.html#parser-and-scanner-theory-and-practice) - Where I describe my experience of taking Automata Theory and Compiler Design and Implementation in the same semester at college. I was exposed to the same problem space from theory and its practice in two separate courses simultaneously.
* [Interpreter Design Pattern – Production Example](https://jhumelsine.github.io/2024/05/14/interpreter-design-pattern-production.html) - Where I created a Domain-Specific Language, even when that was not my original intent
* [My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html) - How I evolve my designs
* [The Conversion of a Unit Test Denier](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html) ... or How I Learned to Stop Worrying and Love Unit Testing
* [Suril, the Semaphore and Me](https://jhumelsine.github.io/2024/07/08/suril-semaphore.html) - How I learned the true meaning of Software Under Test
* [Yuri, the Programming Assignment and Me](https://jhumelsine.github.io/2024/07/22/yuri-evaluator.html) - When I indoctrinated a young mind with TDD
* [Losing Your Job Stinks](https://jhumelsine.github.io/2024/08/11/jobsearch.html) - Where I describe my unemployment experience during a down market after the dot-com bubble burst in the early 2000s
* [Testing Concerns](https://jhumelsine.github.io/2024/09/08/test-concerns.html) - Where I list the lessons I learned when I was on the [QA team](https://jhumelsine.github.io/2024/09/08/test-concerns.html#manual-test-code)
* [Abstraction](https://jhumelsine.github.io/2024/10/30/abstraction.html) - Where I describe the conversation, I had with a mentee at my college alma mater, which inspired this blog
* [What Is Cohesive Abstraction?](https://jhumelsine.github.io/2024/11/27/abstraction-cohesion.html) - Where I describe my experience with [Safety Critial Code](https://jhumelsine.github.io/2024/11/27/abstraction-cohesion.html#safety-critical)
* [A House Divided Against Itself Will Not Stand](https://jhumelsine.github.io/2025/03/07/house-divided.html) - Where I resolve the conflict between [My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html) and [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) blog entries
* [Prepping for a demo](https://jhumelsine.github.io/2025/03/28/mutation-testing.html#prepping-for-a-demo) - Where I describe how I stumbled onto [Mutation Testing](https://jhumelsine.github.io/2025/03/28/mutation-testing.html) by accident.
* [Heartbeat or Heart Attack?](https://jhumelsine.github.io/2025/04/08/time-lord.html#heartbeat-or-heart-attack) - Where I describe a bug that took 42 days of continuous execution before it manifested itself in [Mastering Time in Software Testing - Strategies for Temporal Behavior Verification](https://jhumelsine.github.io/2025/04/08/time-lord.html).

## Cautionary Tales and Principle of Least Astonishment
Cautonary Tales may help others avoid some poor choices I encountered, some of my own making. This section will also contain a few examples of violations of the [Principle of Least Astonishment](https://en.wikipedia.org/wiki/Principle_of_least_astonishment) (POLA):

* [What Is Cohesive Abstraction?](https://jhumelsine.github.io/2024/11/27/abstraction-cohesion.html) - Where I describe a [A Cautionary Tale](https://jhumelsine.github.io/2024/11/27/abstraction-cohesion.html#a-cautionary-tale), which darn near cost me my job
* [Working Effectively with Legacy Code](https://jhumelsine.github.io/2025/03/24/legacy-code.html) - Where an _obvious_ error lurked in the code for months due to the lack of testing in [Hey. This doesn't look right.](https://jhumelsine.github.io/2025/03/24/legacy-code.html#hey-this-doesnt-look-right)

# Miscellaneous
This is for blog entries that were one-offs, and weren't part of series:

* [Job Loss](https://jhumelsine.github.io/2024/08/11/jobsearch.html) - Software Engineering has been a great career for me, but it wasn't without a few bumps in the road. I wrote this blog when others I knew in the industry were running into the same bumpy road in their careers.

# Complete Code Demos
Many blogs have complete code examples to demo the principles. They are in Java and collected into one file, even if the file contains many multiple interfaces and classes. All examples are self contained within Java. There are not external dependencies to inport.

Copy the demo into a Java environment and run it. If you don’t have Java, try this [Online Java Environment](https://www.programiz.com/java-programming/online-compiler/).

Play with the implementation. Add tests. Refactor the demo. Copy the demo into Generative AI platform for analysis and comments.

**NOTE:** While developing this section, it became obvious to me that I had not provided as many complete code demos as I should have. I will slowly add them retroactively and provide references here when provided. I may also reorganize the list once it gets more content.

* [Command Design Pattern](https://jhumelsine.github.io/2023/09/18/command-design-pattern.html#complete-demo-code)
* [My Design Process](https://jhumelsine.github.io/2024/05/28/design-process.html#references), which includes a Circuit Design implementation for the design featured in the blog.
* [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html#complete-demo-code)
* I featured a **Rational Expression Evaluator** as a use case example in the [Interpreter Design Pattern](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) series. I developed the evaluator across several Interpreter blogs:
    * [Rational Expression Evaluator Grammar Rules](https://jhumelsine.github.io/2024/04/02/interpreter-design-pattern-grammars.html#rational-number-evaluator-grammar-use-case), which define the grammar for my rational expression evaluator example.
    * [Rational Expression Evaluator Grammar to Design](https://jhumelsine.github.io/2024/04/07/interpreter-design-pattern-design.html#grammar-to-design-via-use-case), which defines an OO design that emerges directly from the grammar.
    * [Rational Expression Evaluator](https://jhumelsine.github.io/2024/04/17/interpreter-design-pattern-implementation.html#the-complete-implementation), which is a complete Rational Expression Evaluator implementation, but it does not have a reasonable interface with the user.
    * [Interactive Rational Expression Evaluator](https://jhumelsine.github.io/2024/04/30/interpreter-design-pattern-parser-implementation.html#the-complete-implementation), which defines a Scanner and Parser that evaluates the Rational Expression DSL and executes it via a Read-Eval-Print Loop.
* [Yuri, the Programming Assignment and Me](https://jhumelsine.github.io/2024/07/22/yuri-evaluator.html#the-complete-implementation), which features a recursive implementaiton for an arithetic expression evaluator.
* [Working Effectively with Legacy Code](https://jhumelsine.github.io/2025/03/24/legacy-code.html#complete-code-example), which features a small example of iterating through all possible arguments for a legacy method so it can be refactored safely.
* [Mastering Time in Software Testing - Strategies for Temporal Behavior Verification](https://jhumelsine.github.io/2025/04/08/time-lord.html#complete-demo-code), which features an injected `Clock` to take control of time.
* [Abstract Factory Design Pattern](https://jhumelsine.github.io/2025/07/30/abstract-factory.html#complete-demo-code), which features a weapon system consistent with launcher/project pairs, such as rifle/bullet, bow/arrow, etc.
* The [Build Design Pattern](https://jhumelsine.github.io/2025/08/08/builder-introduction.html) required several blogs with several complete phases showing its evolution:
    * [Builder Design Pattern - Basic Implementation](https://jhumelsine.github.io/2025/08/13/builder-basic-impl.html#complete-demo-code), which features building a Pizza using several techniques.
    * [Builder Design Pattern - Complete Implementation](https://jhumelsine.github.io/2025/08/20/builder-complete-impl.html#complete-demo-code), which competes the Pizza Builder example.
    * [Director](https://jhumelsine.github.io/2025/08/27/builders-director.html#complete-demo-code), which champions the Director component of the Builder Pattern as its own pattern featuring a Coffee Shop Drink example.
* [Singleton Design Pattern](https://jhumelsine.github.io/2025/10/31/singleton.html#complete-demo-code), which features several versions of Singleton demonstrating incorrect use, correct use, state injection, etc.
* [Flyweight Design Pattern](https://jhumelsine.github.io/2025/11/14/flyweight.html#complete-demo-code), which features a cloud DVR example.
* [Object Pool Design Pattern](https://jhumelsine.github.io/2025/11/28/object-pool.html#complete-demo-code), which features several versions of an Object Pool.
* [Prototype Design Pattern](https://jhumelsine.github.io/2025/12/23/prototype.html) required a couple blogs, each with its own code example:
    * [Prototype Basics](https://jhumelsine.github.io/2025/12/23/prototype.html#complete-demo-code), which featured a Prototype implementation.
    * [Prototype Shape Rendering Use Case](https://jhumelsine.github.io/2025/12/30/prototype-use-case.html#complete-demo-code), which featured a Shape Rendering example demonstrating how Prototype and Composite can work together.

# Statistics

<table border="1">
  <thead>
    <tr>
      <th>Year</th>
      <th>Blogs</th>
      <th>Words</th>
      <th>Countries Reached</th>
      <th>Most Views</th>
      <th>Comments</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><strong>2021</strong></td>
      <td style="text-align: right;">1</td>
      <td style="text-align: right;">1,278</td>
      <td>N/A</td>
      <td><a href="https://jhumelsine.github.io/2021/06/22/uml.html">What is UML and why you may want to care</a></td>
      <td>First prototype blog about UML to set up the environment on GitHub.</td>
    </tr>
    <tr>
      <td><strong>2022</strong></td>
      <td style="text-align: right;">0</td>
      <td style="text-align: right;">0</td>
      <td>N/A</td>
      <td>N/A</td>
      <td>Still employed; did not start blogging yet.</td>
    </tr>
    <tr>
      <td><strong>2023</strong></td>
      <td style="text-align: right;">21</td>
      <td style="text-align: right;">38,229</td>
      <td style="text-align: right;">46</td>
      <td><a href="https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html">Hexagonal Architecture – An Introduction</a></td>
      <td>Topics covered were Design Pattern Introduction, Essential Design Patterns and Hexagonal Architecture.</td>
    </tr>
    <tr>
      <td><strong>2024</strong></td>
      <td style="text-align: right;">35</td>
      <td style="text-align: right;">109,944</td>
      <td style="text-align: right;">73</td>
      <td><a href="https://jhumelsine.github.io/2024/03/18/interpreter-design-pattern-dsls.html">Interpreter Design Pattern – Domain-Specific Languages</a></td>
      <td>Topics covered were Composable Design Patterns, Interpreter Design Pattern, Automated Testing, LLMs/GenAIs and Abstraction.</td>
    </tr>
    <tr>
      <td><strong>2025</strong></td>
      <td style="text-align: right;">21</td>
      <td style="text-align: right;">71,033</td>
      <td style="text-align: right;">98</td>
      <td><a href="https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html">Specification Design Pattern – Allow a Client to select or filter objects with specific attribute property values as specified by the Client.
</a></td>
      <td>Specification is from early 2024, but it was by far the most viewed page. I think it's in first page results of a few search engines.
      </td>
    </tr>
    <tr>
      <td><strong>Totals</strong></td>
      <td style="text-align: right;"><strong>78</strong></td>
      <td style="text-align: right;"><strong>220,484</strong></td>
      <td></td>
      <td></td>
    </tr>
  </tbody>
</table>
