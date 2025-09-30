---
title: Parts is Parts
description: Design patterns are comprised of different configurations of the same repeating Object-Oriented constructs. 
---

<img src="https://live.staticflickr.com/3377/3432530375_74eea3ea1a_c.jpg" alt="Taco Bell Menu" title="Image Source: https://www.wallpaperuse.com/wallp/42-424153_m.png" width = "50%" align="center" style="padding-right: 20px;">

# Parts is Parts
Many cuisines are comprised of different combinations of the same basic ingredients. Fast food businesses, such as Taco Bell, come to mind. These kitchens have vats of meat, cheese, rice, beans, and other basic ingredients. Wrap them in a tortilla, and it’s a burrito. Put them in a corn shell, and it’s a taco. Put some on a bed of chips, and it’s a nacho platter.

Design patterns are much the same. The Gang of Four’s (GoF) [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) address repeating problems in an Object-Oriented (OO) paradigm. Most of their design patterns are comprised of different configurations of the same repeating OO constructs. Developers who understand these constructs will be able to learn design patterns a bit quicker than those who don’t.

<img src="https://live.staticflickr.com/3761/32764082670_a83b6da827_b.jpg" alt="Harry Potter Spells" title="Image Source: https://www.flickr.com/photos/javcon117/32764082670" width = "35%" align="right" style="padding-right: 20px;">

There are no magic incantations. Design patterns are about intent and the problem being addressed than they are the constructs. In some cases, the OO construct configuration may feel so obvious, that’s not even worth the effort to have cataloged it as a design pattern.

There is value with simple obvious configurations. Even the simplest pattern frames its OO constructs within a context. The pattern provides intent, especially within the context of the problem being addressed. The pattern provides a common name. The pattern provides a chunkable unit for the configuration.

# Object-Oriented Constructs

There are two types of OO constructs. The first is about references between classes. The second is about the method calls the objects of those referenced classes.
The following UML class diagrams illustrates these references (See: [UML Class Diagram](https://jhumelsine.github.io/2021/06/22/uml.html) for notation references).

![Basic UML Relationships](/assets/UML.png)

## Class Relationships
A class relationship indicates when one class knows of the other. It’s a pair-wise relationship, much like the edge of a graph indicates the relationship between two nodes. The relationship is usually directional. That is, one class knows about the other class, but usually not the other way. The arrowheads, as shown above, indicate the direction of knowledge. When **A -> B**, then A knows of B, but B does not know of A.
### HAS-A
The **HAS-A** relationship is when one class **has a** reference to another class. This occurs with field attributes, method parameters, field attributes, etc.

This also occurs somewhat indirectly with the class of objects within Containers. That is, when a class uses a Container of Cats, then that class knows about Cats as well in that it **has a** container of Cats. This type of relationship is often called composition or aggregation.
### IS-A
The **IS-A** relationship is when one class **is a** type of another class. This occurs with classes that implement or extend interfaces or other classes respectively. This type of relationship is often called inheritance.
### Self-Referential
The **self-referential** relationship is both **IS-A** and **HAS-A**. This happens when a concrete class that inherits an interface/base-class also **has a** reference to the interface/base-class from which it inherits. It has dual attributes that the concrete class **is a** type of the interface/base-class and **has a** relationship to the same interface/base-class.

Self-referential structures are common in data structures. A Tree is comprised of nodes, where each node has a reference to a left sub-Tree and a right sub-Tree.

Self-referential structures are not unheard of in OO design, but I don’t recall seeing their use before encountering them with the GoF. It’s an extremely powerful concept, which I’ll detail more when writing about composition-based patterns.
## Object Interaction
Classes don’t just have reference relationships. The objects of those classes invoke one another along these lines via those references. While it should be obvious, the same knowledge directions with class relationships applies here as well, with one exception noted below.
### HAS-A Object Interaction
Here are typical method invocations in the **HAS-A** relationships:
* **Delegation** – This is one object calls the method of another object it references.
* **Propagation** – This is a specific type of delegation, and it tends to be associated with Containers, especially when behavior in the client “propagates” to the objects it is containing.
* **Escalation** (my term) – This is when the contained object invokes the class that contains the reference to the contained object’s Container. This is rare, especially since contained objects often don’t know the classes that manage their containers. Contained objects often don’t even know they are in containers. Additional infrastructure/plumbing is required when escalation is desired. I chose this term, since I view Container structures, especially when the classes being contained have their own Containers, as the hierarchical structure of an organization. Escalation is passing a request up the management chain in the hierarchy.

Each of the above examples is one class calling the method of a reference that it knows. I view **Delegation** as passing responsibility to another method, possibly in another class, to complete. I view **Propagation** more like tipping dominoes, especially when they fan out. It’s still delegating, but it’s delegating the same theme throughout the Container structure.
### IS-A Object Interaction
Here are typical method invocations in the **IS-A** relationships:
* **Polymorphism** – This is when the client is calling a method declared in an interface or abstract base class, but the method implementation being executed resides in a concrete class implementing or extending the interface or base class. This is the noted knowledge direction exception that I mentioned above. The client should only know the interface or base class. It shouldn’t know the concrete class type where the method being executed resides. There are ways to resolve the paradox of a client being able to acquire a reference to a class it does not know, but I’ll write about that later.
* **Tradition** (my term) – This is when a reference to an extended class executes a method declared in one of the base classes. This could be by default, or it could be the case where the child class method explicitly calls the method of a parent class in the inheritance hierarchy. I chose this term because I view inheritance as familial. Tradition indicates that the child class behavior is the same as one of its parent class behaviors. It follows the tradition of its ancestors.
  
### Static Method Invocation
The last method invocation is not a relationship between objects. It is a **Static Method Invocation**. A statically declared method is accessible from the class itself without the need for an object reference. Static method invocation usually occurs with [Creational Design Patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html). That is, it allows clients to acquire a reference to a class without having to invoke the constructor directly.

# Conclusion
Understanding these basic Object-Oriented building block components will help developers understand how the GoF used them to design most of their design patterns. These basic components won’t cover all the details, but they will cover most.

For more of my take on Design Patterns, See: [Index of my Design Patterns Blogs](https://jhumelsine.github.io/table-of-contents#design-patterns).
