---
title: What Is an Object?
description: Clarifying Class, Object, Instance, and Their Real-World Meaning
---
<img src="https://openclipart.org/image/800px/68929" alt="Andy Warhol" title="Image Source: https://openclipart.org/detail/68929/andy-warhol" width = "40%" align="center" style="padding: 35px;">
 
# Introduction
Object-oriented programming has been around for decades, yet the most basic question—_what is an object?_—may cause some confusion. The terms “class,” “instance,” and “object” are often tossed around casually, but they’re not synonyms. In fact, misunderstanding the distinctions can lead to sloppy modeling, unclear code, and fragile designs. In this blog entry, we’ll revisit these core concepts, ground them in analogy and practice, and set the stage for exploring design patterns like Singleton.

# What is an Object?
This blog entry is a precursor to the Singleton Design Pattern (TBD). It was originally going to be part of the Singleton blog entry itself, but I decided that it has enough merits to stand on its own.

Before I dive into Singleton in the next blog, I’d like to consider Object-Oriented (OO) fundamentals and ask, ___What is an object?___ We use ___object___ frequently, but I’m not sure that we always use it consistently. I try my best to use it consistently, but I know that I’ve not always been consistent myself.

Sometimes we might use the term ___object___ when we mean ___class___. Sometimes we might use the term ___object___ when we mean ___instance___ as in ___an instance of a class___ or even ___an object instance of a class___. These two new terms suggest two more questions:
* ___What is a class?___
* ___What is an instance?___

## What is a Class?
A _class_ implementation defines relationships with other classes through inheritance, reference and delegation. Classes declare methods whose implementation define their behavior.
Code written for a _class_ within an OO implementation defines the boundaries of the _class_. Our terminology is mostly consistent with OO languages since `class` is a keyword for this concept in many of them.

__Classes declare and define behavior and relationships with other classes as specified within the implementation.__

## What is an Instance?
Classes define potential in the form of code. That potential is realized when the code is executed. Except for `static` defined implementations, class potential can only be realized by an instance of a class.

An instance contains state information for a class usually in the form of the values for its `private` and `protected` field attributes. Multiple instances of the same class can be created, each with their own independent state.

__An instance executes the methods of the class it instantiates and exhibits behaviors relative to its state information.__

__Classes reside in code. Instances reside in memory.__

## A Summary of Class, Instance and Object

| Term      | Definition                                                                 | Key Characteristics                                                                 | Example (Dog Domain)                              |
|-----------|-----------------------------------------------------------------------------|-------------------------------------------------------------------------------------|--------------------------------------------------|
| **Class** | A blueprint or template that defines structure (fields) and behavior (methods). | - Describes what all its instances share.<br>- No concrete data until instantiated. | `class Dog { name, age, bark() }`                |
| **Instance** | A specific, occurrence of a class. Created when the program uses the class to allocate memory. | - Has actual values for fields.<br>- Behaves according to its class definition.      | `Dog d1 = new Dog("Buddy", 5)`                   |
| **Object** | The general term often used to refer to an instance of a class, though usage can be ambiguous. | - In many contexts, “object” = instance.<br>- Sometimes used loosely to mean any entity in a system. | `d1` is an object of class `Dog`                 |


## A few real-world examples of classes and their instances
While I consider _object_ and _instance_ as two terms for the same concept, I think that since we tend to focus so much upon _classes_ when using OO, we sometimes use the term _object_ when we really mean _class_. Some of our programming languages contribute to this confusion. All classes in Java inherit from a root superclass, named [Object](https://docs.oracle.com/javase/8/docs/api/java/lang/Object.html), even if it's really a __class__. Experienced developers understand the intent based upon the context, but using the term _object_ inconsistently can be confusing to those who are learning OO.

I will use _class_ and _instance_ rather than _object_ for the rest of this blog entry to avoid any potential confusion.

Here are a few examples of classes and their instances.

### Chairman Mao
<img src="https://live.staticflickr.com/31/96727458_434393623a_b.jpg" alt="Warhol's Mao" title="Image Source: https://www.flickr.com/photos/andydoro/96727458" width = "35%" align="right" style="padding: 35px;">
 
[Andy Warhol](https://en.wikipedia.org/wiki/Andy_Warhol) pioneered silk screening as an art form. His Mao prints display multiple images of Chairman Mao printed from the same silkscreen template. The silk screen template is like a class. It defines potential. Images are realized when ink is forced through the silk screen template onto paper or canvas. Here is an example of nine images of Mao created by Warhol. He was not limited to this set. He could have created many more, and I have no idea how many color combinations he created.

The silk-screen template was Warhol's class, and the prints created from it were his instances.

### Design Patterns
<img src="/assets/DecoratorDrinks.png" alt="Starbuzz Labels via Decorator" width = "30%" align="right" style="padding-right: 20px;">

When I visualize Design Patterns, I tend to do so via classes. I have described [Design Patterns](https://jhumelsine.github.io/3000/01/01/preface.html#design-patterns) throughout my blog entries using [UML Class Diagrams](https://jhumelsine.github.io/2021/06/22/uml.html).

Behavior defined in classes within these patterns can only be realized via instances of those classes. For many design patterns, the instance is a runtime detail that’s often assumed. This is prevalent in the [Essential Design Patterns](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html) for which runtime instance realization was not addressed by the GoF. I included instance realization mostly by introducing a [Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer) without much additional detail other than to show how it realized the instance and injected it into the primary portion of the design.

<img src="/assets/SpecificationPlaylistObjects2.png" alt="Leaf Playlist Classes" width = "45%" align="right" style="padding-right: 35px;">

There are some patterns where the instances are critical in defining emergent behavior. This is most prevalent in the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html). Composable Design Patterns define classes with relatively limited atomic behavior from which the desired behavior emerges from the composition and coordination of instances of those limited behavior classes. No one class in a Composable Design Pattern can do __everything__, but an organized group of instances of the classes defined within the pattern can do __anything__.

The [Interpreter Design Pattern](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) is probably the greatest example of this. Each Interpreter class implements a rule in the [Grammar](https://jhumelsine.github.io/2024/04/02/interpreter-design-pattern-grammars.html) for a [Domain-Specific Language](https://jhumelsine.github.io/2024/03/18/interpreter-design-pattern-dsls.html) (DSL). Instances of those grammar rules are created and assembled into a parse tree by the [Parser](https://jhumelsine.github.io/2024/04/25/interpreter-design-pattern-parser.html#parser) based upon a script written in the DSL. Each DSL script is parsed into its own parse tree of instances which collectively exhibits the unique behaviors defined in that DSL script. An almost infinite set of behaviors can emerge from the assembled configuration of instances created from one finite set of rule-based classes.

### Data Structures
<img src="https://upload.wikimedia.org/wikipedia/commons/b/bf/C_language_linked_list_adding_a_link_step_2.png" alt="Linked List" title="Image Source: https://commons.wikimedia.org/wiki/File:C_language_linked_list_adding_a_link_step_2.png" width = "40%" align="right" style="padding: 35px;">
 
On the other hand, when I visualize Data Structures, I tend to do so via object instances.

I initially struggled in my Data Structures course in college. Something wasn't clicking. A few weeks into the semester, my professor's lectures focused upon specific [Data Structures](https://en.wikipedia.org/wiki/Data_structure), such as Stacks, Queues, Deques, Lists, Trees, etc. His presentations became more visual. He drew linked list nodes and the links connecting them. For each linked list operation, he updated the nodes and links in logical order, maintaining references until the list was completed for each operation. As he drew each update to the list, he wrote the corresponding code that implemented each update that matched the visual representation of the linked list.

While data structure code is not overly complex, just imagine how confusing a [doubly linked list](https://en.wikipedia.org/wiki/Doubly_linked_list#Inserting_a_node) operation would be without a visual model. The visual diagram of nodes and reference links made sense to me. I could see how his diagram and corresponding implementation were two representations of the same concept. It clicked.

While we didn’t have terms such as _class_, _instance_ or _object_ in 1982, my professor's data structure diagrams mirrored _instances_, whereas his accompanying implementation mirrored _classes_. I suspect that data structures are presented in academia these days as _classes_ and _instances_. 

I visualize data structures as instances because most data structures don't contain too many classes in the design. They often only contain one. A Linked List is a `ListNode` class with a reference to the `next` `ListNode`. A Tree is also a `TreeNode` with a reference to a `left` sub `TreeNode` and a reference to a `right` sub `TreeNode`. Many data structures are comprised of composable instances of self-referential classes.

__Side Note:__ The _class_ for a Tree and Doubly Linked List would the same. It's a `Node` with two references to two other `Nodes`. The only distiction is how the references assigned in the operations. Trees reference sub-Tree `Nodes`. Doubly Linked Lists reference their left and right neighbor `Nodes`.

## Where is the line between class and instance?
I’ve defined _class_ as code implementing behavior and _instance_ as the memory location for state in realizing that behavior at runtime. But it’s not always obvious what should be defined in a _class_ and what should be realized in an _instance_. When should behavior reside in the class implementation, the instance state or the organization of a set of instances?

I have already illustrated an example of this decision in the previous [Builder/Director](https://jhumelsine.github.io/2025/08/27/builders-director.html) blog where my initial design contained classes for `Tea`, `Coffee`, `Sugar`, `Lemon`, etc., and then as I started to test and implement my design, I realized what I had thought were different behaviors that required classes each for `Tea`, `Coffee`, `Sugar`, `Lemon`, etc. were really different attribute states for two classes: `Drink` and `AddOn`. 

### Domain and Modeling
The distinction between behavior residing within a _class_ implementation or behavior residing within an _instance_ state depends upon the domain and how the domain is modeled. I’ll write about this more in the future (TBD) when I blog about [Domain-Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design), but I can provide a summary here.

The Domain (TBD) is business concern of your customer. It’s why they exist, and how they make money. Their business domains could be communications, marketing, finance, health, entertainment, etc. Sometimes our customers might be external, such as when we create software for a hospital or bank, and sometimes our customers might be internal, such as when we create software as employees of Google, Meta or Amazon.

The Domain Model (TBD) is how we represent the customer’s Domain and its associated behaviors within our design and implementation. There are many ways to model a Domain. Some are better than others. We often don't know the Domain well enough, especially as we start, to design a good or even accurate Model. As we learn more about the Domain via the iterative process, we gain additional insights that will result in a better Model. That's why we must strive to keep our design and implementation modular and flexible.

__As we learn that our Model does not accurately reflect our newfound insights about the Domain, we want the flexibility to easily update the Model, design and implementation to better reflect those newfound insights.__ 

>_All models are wrong, but some are useful._ ― [George Box](https://en.wikipedia.org/wiki/All_models_are_wrong)

We must decide which domain elements are important enough to model and which domain elements do not need to be included in the Model.

For example, many domains include a `Client`. The model for most Clients will require a name, contact information, history, etc. Clients will often be individual people, especially for domains, such as Linkedin, Meta, Amazon and Netflix.

While Clients are people, these domains won’t include height and eye color in the model. While these are attributes of people, they aren’t relevant attributes for Clients in these domains.

But what if our customer is a government agency that issues identification documents, such as Driver Licenses and Passports? Personal identifying features, such as height and eye color are critical to those domains, so they would be included in the Client model.

### It’s a Dog-Eat-Dog World
Let’s consider two canine domains:
* Dog Shelters
* Show Dogs

#### Dog Shelters
<img src="https://images.pexels.com/photos/12195433/pexels-photo-12195433.jpeg" alt="Dogs in Shelter" title="Image Source: https://images.pexels.com/photos/12195433/pexels-photo-12195433.jpeg" width = "35%" align="right" style="padding: 35px;">
 
Dog Shelters seek homes for abandoned dogs. They want a website that features their furry friends with a photo and a description for each dog. The description will contain basic information, such as the dog’s name, estimated age, history (if known), temperament, etc.

A shelter’s domain is relatively simple; therefore, a simple model will probably suffice with a single Dog class with each individual dog being an instance of Dog.

#### Show Dog
<img src="https://s0.geograph.org.uk/geophotos/06/18/45/6184562_88499b20_original.jpg" alt="Dogs Show" title="Image Source: https://www.geograph.org.uk/photo/6184562" width = "35%" align="right" style="padding: 35px;">
 
A single Dog class would be woefully insufficient for Show Dog modeling needs. There may still be a Dog class, but there would be subclasses for each show group, such as: Working Group, Miniatures, etc. See: [American Kennel Club](https://www.akc.org/dog-breeds/). Then each show group would have subclasses for the individual breeds within those groups.

Each Group might define the characteristics of the Breeds within that Group. Each Breed might define the standards for that Breed. I don’t know enough about this domain to know whether Groups and Breeds would be their own classes or whether they could be sufficiently represented via state as was the case for Coffee, Tea and Sugar.

Regardless, each Dog instance in the show model would contain much more information than in the shelter model. The show model would contain the dog’s name, pedigree, show history, owner, trainer, and more.

The Show Dog's domain is more complex; therefore, a more complex model will be needed to represent the Dog Groups, Breeds, and their traits.

# Summary
Now that I’ve presented _class_ and _instance_ in excruciating detail, I can come full circle introduce the scenario that will lead to Singleton, which is where I started this blog.

In some domains or design structures, you only ever need one instance of a class—like the system clock or the configuration loader. That setup leads to the __Singleton pattern__ (TBD), which ensures single-instance behavior while addressing concerns like thread safety and testability. That’s where we’ll go next.
