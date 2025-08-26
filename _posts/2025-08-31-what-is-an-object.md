---
title: DRAFT – What is an Object?
description: TBD
unlisted: true
---
<img src="https://openclipart.org/image/800px/68929" alt="Andy Warhol" title="Image Source: https://openclipart.org/detail/68929/andy-warhol" width = "50%" align="center" style="padding: 35px;">
 
# Introduction
__TBD__

# What is an Object?
This blog entry is a precursor to the [Singleton Design Pattern](https://refactoring.guru/design-patterns/singleton) (TBD). It was originally going to be part of the Singleton blog entry itself, but I decided that it is long enough had has enough merits to stand on its own.

Before I dive into Singleton in the next blog, I’d like to return to Object-Oriented (OO) fundamentals and ask, ___“What is an object?”___ We use this term frequently, and I’m not sure that we always use it consistently. I try my best to use it consistently, but I know that I’ve not always been consistent.

Sometimes we might use the term _object_ when we really mean _class_. Sometimes we might use the term _object_ when we really mean _instance_ as in _an instance of a class_ or even _an object instance of a class_. These uses create two more questions:
* What is a class?
* What is an instance?

## What is a class?
Code within an OO implementation defines the behavior of a _class_. Our terminology is mostly consistent since `class` is a keyword for this concept in many OO languages. The class implementation defines relationships with other classes through inheritance, reference and delegation. Classes declare methods which define behavior.

Classes declare and define behavior and relationships with other classes as specified within the implementation.

## What is an instance?
Classes define potential in the form of code. That potential is realized when the code is executed. Except for `static` defined implementations, class potential can only be realized by an instance of a class.

An instance contains state information for a class usually in the form of the values for its `private` and `protected` field attributes. Multiple instances of the same class can be created, each with their own independent state. An instance executes the methods of the class it instantiates and exhibits behaviors relative to its state information.

Classes reside in code. Instances reside in memory.

## A few real-world examples of classes and their instances
While I consider _object_ and _instance_ as two terms for the same concept, I think we tend to focus so much upon _classes_ when using OO, that we sometimes use the term _object_ when we really meant _class_. Those of us with experience quickly adjust based upon the context, but using the term _object_ inconsistently can be confusing who are first learning OO.

I will use _class_ and _instance_ rather than _object_ for the rest of this blog entry to avoid any potential confusion.

Here are a few examples of classes and their instances.

### Chairman Mao
<img src="https://live.staticflickr.com/31/96727458_434393623a_b.jpg" alt="Warhol's Mao" title="Image Source: https://www.flickr.com/photos/andydoro/96727458" width = "20%" align="right" style="padding: 35px;">
 
[Andy Warhol](https://en.wikipedia.org/wiki/Andy_Warhol) pioneered silk screening as an art form. His Mao work displays multiple images of Chairman Mao printed from the same silkscreen template. The silk screen template is like a class. It defines potential. Images are realized when ink is forced through the silk screen template onto paper or canvas. Here is an example of nine images of Mao created by Warhol. He was not limited to this set. He could have created many more, and I have no idea how many color combinations he assembled.

### Design Patterns
<img src="/assets/DecoratorDrinks.png" alt="Starbuzz Labels via Decorator" width = "25%" align="right" style="padding-right: 20px;">

When I visualize Design Patterns, I tend to do so via classes. I have described [Design Patterns](https://jhumelsine.github.io/3000/01/01/preface.html#design-patterns) throughout my blog entries via [UML Class Diagrams](https://jhumelsine.github.io/2021/06/22/uml.html).

Behavior defined in classes within these patterns can only be realized via instances of those classes. For many design patterns, the instance is a runtime detail that’s often assumed. This is prevalent in the [Essential Design Patterns](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html) for which runtime instance realization was not addressed by the GoF. I recognized instance realization but mostly by introducing a [Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer) without much additional detail as to how it realized the instance.

There are some patterns where the instances are critical in defining behavior. This is most prevalent in the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html). Composable Design Patterns define classes with relatively limited atomic behavior. No one class in a Composable Design Pattern can do __everything__, but an organized group of instances of the classes defined within the pattern can do __anything__.

The [Interpreter Design Pattern](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) is probably the greatest example of this. Each Interpreter class implements a rule in the [Grammar](https://jhumelsine.github.io/2024/04/02/interpreter-design-pattern-grammars.html) for the [Domain-Specific Language](https://jhumelsine.github.io/2024/03/18/interpreter-design-pattern-dsls.html) (DSL). Instances of those grammar rules and created and assembled into a parse tree by the [Parser](https://jhumelsine.github.io/2024/04/25/interpreter-design-pattern-parser.html#parser) based upon a script written in the DSL. Each DSL script is parsed into its own parse tree of instances which collectively exhibits the unique behaviors defined in that DSL script. An almost infinite set of behaviors can emerge from the assembled configuration of instances created from the same set of finite rule-based classes.

### Data Structures
<img src="https://upload.wikimedia.org/wikipedia/commons/b/bf/C_language_linked_list_adding_a_link_step_2.png" alt="Linked List" title="Image Source: https://commons.wikimedia.org/wiki/File:C_language_linked_list_adding_a_link_step_2.png" width = "40%" align="right" style="padding: 35px;">
 
When I visualize Data Structures, I tend to do so via object instances.

I had earned high grades in my college Computer Science classes, until I hit Data Structures. I received Cs in the first few weeks of homework assignments, and I was more accustomed to higher grades. I even visited my professor during office hours to voice my concern. Data Structures weren’t clicking. I don’t recall too much of his advice other than to give it more time.

Soon thereafter, his lectures turned to specific [Data Structures](https://en.wikipedia.org/wiki/Data_structure), such as Stacks, Queues, Deques, Lists, Trees, etc. His presentations became more visual. He drew linked list nodes. He drew the links connecting them. He updated the nodes and links in logical order, maintaining references until the list was completed for each operation. As he drew each update to the list, he wrote the code that implemented each update to the visual representation of the linked list.

While data structure code can be intimidating, just take a look at the implementation for a [doubly linked list](https://en.wikipedia.org/wiki/Doubly_linked_list#Inserting_a_node), the visual diagram of nodes and reference links made sense to me. I could see how his diagram and corresponding implementation were two representations of the same concept. It clicked. My grades improved dramatically in the midterms and the remaining homework. I think I ended up with the highest grade in the class.

While we didn’t have terms such as _class_, _instance_ or _object_ in 1982, my professor presented data structures graphically using the equivalent of _instances_, whereas the accompanying implementation was the using the equivalent of _classes_.

I suspect that data structures are presented in academia these days as _classes_ and _instances_.

## Where is the line between class and instance?
I’ve defined _class_ as code implementing behavior and _instance_ as the memory location for state in realizing that behavior at runtime. But it’s not always obvious what should be defined in a _class_ and what should be realized in an _instance_.

I have already demonstrated this case myself in the previous [Builder/Director](TBD) (TBD) blog where my initial design contained classes for Tea, Coffee, Sugar, Lemon, etc., and then as I started to test and implement my design, I realized what I had thought were different behaviors that required classes each for `Tea`, `Coffee`, `Sugar`, `Lemon`, etc. were really different state values for two classes: `Drink` and `AddOn`. 

### Domain and Modeling
The distinction between behavior residing within a _class_ implementation or behavior residing within an _instance_ state depends upon the domain and how the domain modeled. I’ll write about this more in the future (TBD) when I blog about [Domain-Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design), but I can provide a summary here.

The Domain (TBD) is business concern of your customer. It’s why they exist. Their business concern could be communications, marketing, finance, health, entertainment, etc. Sometimes our customers might be external, such as creating software for a hospital or bank, and sometimes our customers might be internal, such as software we provide as employees of Google, Meta or Amazon.

The Domain Model (TBD) is how we represent the customer’s Domain and its associated behaviors within our design and implementation. There are many ways to model a Domain. Some better than others.

>_All models are wrong, but some are useful._ ― [George Box](https://en.wikipedia.org/wiki/All_models_are_wrong)

We must decide which domain elements are important enough to model and which domain elements do not need to be included in the model.

For example, many domains include a `Client`. The model for most Clients will require a name, contact information, history, etc. Clients will often be individual people, especially for domains, such as Linkedin, Meta, Amazon and Netflix.

While Clients are people, these domains won’t include height and eye color in the model. While these are attributes of people, they aren’t relevant attributes for Clients in these domains.

But what if our customer is a government agency that issues documents, such as Driver Licenses and Passports? Personal identifying features, such as height and eye color are critical to those domains, so they would be included in the Client model.

### It’s a Dog-Eat-Dog World
Let’s consider two domains, both relative to Dogs:
* Dog Shelters
* Show Dogs

#### Dog Shelters
<img src="https://images.pexels.com/photos/12195433/pexels-photo-12195433.jpeg" alt="Dogs in Shelter" title="Image Source: https://images.pexels.com/photos/12195433/pexels-photo-12195433.jpeg" width = "20%" align="right" style="padding: 35px;">
 
Dog Shelters want to find homes for abandoned dogs. They want a website that features their furry friends with photo and a description for each dog. The description will contain basic information, such as the dog’s name, estimated age, history (if known), temperament, etc.

A shelter’s domain will probably suffice with A single Dog class with each individual dog being an instance of Dog.

#### Show Dog
<img src="https://s0.geograph.org.uk/geophotos/06/18/45/6184562_88499b20_original.jpg" alt="Dogs Show" title="Image Source: https://www.geograph.org.uk/photo/6184562" width = "20%" align="right" style="padding: 35px;">
 
Show Dog are a different domain. A single Dog class would be woefully insufficient for their modeling needs. There may still be a Dog class, but there would be subclasses for each show group, such as: Working Group, Miniatures, etc. See: [American Kennel Club](https://www.akc.org/dog-breeds/). Then each show group would have subclasses for the individual breeds within those groups.

Each Group might define the characteristics of the Breeds within that Group. Each Breed might define the standards for that Breed. I don’t know enough about this domain to know whether Groups and Breeds would be their own classes or whether they could be sufficiently represented via state as was the case for Coffee, Tea and Sugar.

Regardless, each Dog instance in the show model would contain much more information than in the shelter model. The show model would contain the dog’s name, pedigree, show history, owner, trainer, and more.

# Summary
__TBD__

Now that I’ve presented _class_ and _instance_ in excruciating detail, I can come full circle introduce the scenario that will lead to Singleton, which is where I started this blog.

Sometimes, the domain/model contains a concept for which there’s just one instance. These tend to be lower-level elements, such as the system clock, file system, a data store, etc., but it could be single instance elements in the customer’s Domain too.

The next blog (TBD) will continue with Singleton describing how it can accommodate the one instance scenario.

# References
_TBD_
