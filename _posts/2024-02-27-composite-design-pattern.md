---
title: Composite Design Pattern
description: Configure behavior emerging from a group of snippet behavior objects organized in a tree structure.
---
<img src="https://live.staticflickr.com/7154/6838743675_d5c4b4e6cd_b.jpg" alt="Jet Engine Cutaway" title="Image Source: [https://www.flickr.com/photos/davegray/6463738329](https://www.flickr.com/photos/multiplyleadership/6838743675
)" width = "60%" align="center" style="padding-right: 20px;">

# Introduction
The Composite Design Pattern is the next of the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html)  series.

The Composite Design Pattern embodies the concept of composable design patterns more completely than the previous patterns we’ve seen. Several concepts first introduced in [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) will be revisited here as well.

Let’s review the composable patterns we’ve seen so far:
* [Proxy](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html), which featured a wrapper class in front of a core feature class.
* [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html), which featured a list of decorator objects that adorned a core feature with additional behavior. It’s a dynamically composable alternative to inheritance.
* [Chain of Responsibility](https://jhumelsine.github.io/2024/02/20/chain-of-responsibility-design-pattern.html), which featured a list of handlers, each of which has the potential to complete a task, such that the first handler in the list which can complete task is the one that does complete the task. It’s a dynamically composable alternative to `switch`/`if/else-if/else` statements.

The previous composable design patterns are based upon a linear structure. They use single delegation or a list as their data structure mechanism. Each tends to focus upon one core feature class, which is enhanced by other supporting classes composed around it.

Composite is almost exclusively about composition. There is no single core feature class. Composite features multiple classes with each providing a snippet of behavior. Objects implementing those snippets of behavior are composed into _a core_ behavior that’s emerges from an aggregation of those object snippets organized via Composite. There could be many behaviors depending upon the organization of the object snippets.

Composite breaks the shackles of a linear structure. Composite’s underlying data structure is a self-referential tree. One codebase can accommodate any number of Composite object trees that are as wide or as deep as needed for each given situation. A Composite tree could be one object or thousands of objects.

Composition is not a foreign concept in OO. It’s the HAS-A relationship, as in:
* A Car HAS-A(n) Engine.
* A Chordate HAS-A Spine.
* A Public Company HAS-A set of C Suite Officers.

HAS-A relationships are often rendered in OO programming language as a private field attribute class or a collection of classes.
The other primary OO relationship is IS-A, which is traditionally rendered with inheritance.

Just as Decorator is a dynamically composable alternative to rendering IS-A using inheritance, Composite is a dynamically composable alternative to traditional rendering of HAS-A relationships in OO design.

Traditional HAS-A implementation tends to be statically locked into place at compile time as hardcoded field attributes. There is nothing wrong with this technique, especially with a domain for which these relationships do not change. Cars will always have engines. Chordates will always have spines. I often used this technique.

But sometimes HAS-A relationships aren’t always set in stone. They may vary. They may not be known until runtime. These are the situations where Composite may be more valuable for HAS-A relationship designs than traditional OO practices.

# Real World Analogies to Composite
Like the previous composable design patterns, Composite is not difficult to implement. In fact, its implementation is almost trivial. Its main challenge is comprehension. Here are a few real-world examples to ease one into the Composite concept.

## Jet Engine
I featured this jet engine cutaway at the top. It’s a sophisticated piece of machinery. I’m not a mechanical engineer, so I asked ChatGPT to list some parts of a jet engine:
<img src="https://live.staticflickr.com/7154/6838743675_d5c4b4e6cd_b.jpg" alt="Jet Engine Cutaway" title="Image Source: [https://www.flickr.com/photos/davegray/6463738329](https://www.flickr.com/photos/multiplyleadership/6838743675
)" width = "40%" align="right" style="padding-right: 20px;">
* Inlet
* Compressor
* Combustion Chamber
* Turbine
* Exhaust Nozzle
* Accessory Gearbox
* Fan (In turbofan engines)
* Afterburner (in some military engines)

Imagine deconstructing a jet engine completely. It will be a collection of parts such as blades, metal plates, nuts, bolts, gaskets, etc. It will cease to be an engine. Most of the parts listed above will cease to exist as well. There will no longer be a Compressor or Turbine. It will just be the parts that used to be configured in such a way that they had the properties of a Compressor or Turbine.

This is akin to the old joke that an airplane is 10,000 loose parts flying in close formation.

I would probably model a jet engine more traditionally with: JetEngine HAS-A Compressor, JetEngine HAS-A Turbine and even more decomposition with Turbine HAS-A set of Blades, etc. rather than Composite, since the composition of a Jet Engine is not likely to change frequently.

The purpose of my example is to highlight the compositional HAS-A nature of a Jet Engine regardless of how one might model it using a traditional OO approach or Composite. The Compressor and Turbine really do not exist as an entity. Heck, even the Jet Engine does not really exist. They are the compositional sum of their components in a specific configuration. While we often refer to this relationship as HAS-A, it’s closer to IS-COMPRISED-OF.

## Unix Filesystem
<img src="https://upload.wikimedia.org/wikipedia/commons/1/17/Unix-fs.png?20100226103342" alt="Unix Filesystem Layout Example" title="Image Source: https://www.wikiwand.com/en/Logic_gate#Media/File:Four_bit_adder_with_carry_lookahead.svg" width = "40%" align="right" style="padding-right: 35px;">

The [Unix Filesystem](https://en.wikipedia.org/wiki/Unix_filesystem) is organized as a tree structure. Files are terminal nodes. Directories are non-terminal nodes, which can contain files, directories or nothing. But it's more than that. Directories are files too. In Unix, [everything is a file](https://web.archive.org/web/20120310211947/http://www.bga.org/~lessem/psyc5112/usail/concepts/filesystems/everything-is-a-file.html).

The self-referential filesystem structure, where directories can contain other directories, means that a command issued in a directory can propagate through all directories and files composed within the directory from which the command was executed especially if the recursive `-r` option was provided.

The [directory](https://en.wikipedia.org/wiki/Directory_(computing)) concept is not limited to Unix, but it may have different names, such as _Folder_ on a Mac or on Microsoft Windows

## Logic Gates
<img src="https://upload.wikimedia.org/wikipedia/commons/1/16/Four_bit_adder_with_carry_lookahead.svg" alt="Full Adder Logic Gates" title="Image Source: https://www.wikiwand.com/en/Logic_gate#Media/File:Four_bit_adder_with_carry_lookahead.svg" width = "30%" align="right" style="padding-right: 35px;">

The [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) blog featured an image of logic gates for a four-bit adder.

A Multi-bit Adder is a composite of Full Adder components, which are each a composite of AND, OR and NOT logic gates. The Multi-bit Adder and the Full Adders are composite configurations of these three simple logic gates. This applies to almost all logical components of a computer. They only have meaning as a concept because we choose to assign them meaning based upon the behaviors that emerge from their configurations.

## Lego Brick Sets
Lego has been a popular toy for decades. The bricks were mostly basic when I was a child. Since then, Lego has expanded to specific themed sets often tied in with marketing and merchandizing of other brands, such as Star Wars themed Lego sets.

There are hybrid sets too. My son had a Police Boat set which featured a specific configuration on the front cover of the box, with step-by-step instructions on how to build the featured Police Boat. But the back cover provided photos of alternative boats that could be assembled from the same bricks. Other than the photos, these alternative boats were left up to the child’s imagination.

No individual Lego Brick has much agency. Form doesn’t emerge until the bricks are snapped together to create a cohesive structure. 

<img src="https://live.staticflickr.com/7057/7053151615_0e25fcf31d_o.jpg" alt="Lego Millennium Falcon" title="Image Source: https://commons.wikimedia.org/wiki/File:Unix-fs.png" width = "40%" align="left" style="padding-right: 35px;">

You embark upon a new Lego project. You start with thousands of individual bricks which will eventually become the Millennium Falcon. They are assembled in stages. Individual bricks are components assembled into composite ship parts. These composite parts are assembled into even larger composites. Eventually the composites become identifiable parts of the ship, such as the cockpit, landing gear, engines, etc. After hours of assembly, the final parts are snapped together, and you have the Millennium Falcon.

<img src="https://live.staticflickr.com/4764/39195349224_39bf74764a_o.png" alt="Lego Patent" title="Image Source: https://www.flickr.com/photos/brickset/39195349224" width = "30%" align="right" style="padding-right: 35px;">

Even though the individual bricks have different shapes and sizes, they follow the consistent Lego snapping mechanism, which has been their common interface since the beginning. All bricks, regardless of their sets, can snap together with other bricks. There are even [apps](https://petapixel.com/2021/07/01/brickits-ai-camera-scans-your-lego-to-suggest-things-you-can-build/) that will use AI to create new projects with instructions from a photo of random bricks.

These properties aren’t unique to Lego Bricks. They apply to many construction toys including:
* [Tinkertoys](https://en.wikipedia.org/wiki/Tinkertoy)
* [Lincoln Logs](https://en.wikipedia.org/wiki/Lincoln_Logs)
* [Erector Sets](https://en.wikipedia.org/wiki/Erector_Set)

## Cooking
One of my first blogs featured cooking – [Knock Knock … Who’s There?]( https://jhumelsine.github.io/2023/08/21/knock-knock-whos-there.html)

Meals are a composite of dishes, which are a composite of ingredients, which are a composite of molecules, which are ….

How far do we take the decomposition of components into composites? Where do we stop?

“If you wish to make an apple pie from scratch, you must first invent the universe.” – [Carl Sagan]( https://www.youtube.com/watch?v=5_vVGPy4-rc).

With all deference to Carl Sagan, we don’t have to go that far as we'll soon see.

## Life to Chemistry
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/f/fb/201_Elements_of_the_Human_Body-01-body.jpg/558px-201_Elements_of_the_Human_Body-01-body.jpg" alt="Elements of the Human Body" title="Image Source: https://commons.wikimedia.org/wiki/File:201_Elements_of_the_Human_Body-01-body.jpg" width = "40%" align="right" style="padding-right: 35px;">

Complex living organisms are composites of systems, such as: nervous system, circulatory system, pulmonary system, etc.

Systems are composites of tissues, such as: nerves, muscle, arteries, etc.

Tissues are composites of cells.

Cells are composites of organelles.

Organelles are composites of molecules.

Molecules are composites of atoms.

Much like an airplane being thousands of loose parts flying in tight formation, the human body is an unfathomable number of loose atoms composed in tight formation.

## Turtles All the Way Down
But why stop with atoms in the Chemistry/Biology example? Atoms are composites of protons, neutrons and electrons. Protons are composites of quarks.

Who knows? Maybe quarks and other subatomic particles are composites of other components not yet identified.

Composite’s self-referential tree structure means that we can easily ride these composite turtles a long way down _ad absurdum_.

<img src="https://live.staticflickr.com/4031/4288661567_dceef21cfb_o.jpg" alt="Sagan Apple Pie Recipe" title="Image Source: https://www.flickr.com/photos/neven/4288661567" width = "50%" align="right" style="padding-right: 35px;">

One could argue that since most of the previous composite examples are based upon materials in the real world, they all could be reduced to subatomic particles. But unless we’re writing software for the particle collider, we’re probably not concerned with subatomic particles in our models.

We stop when we've reached behavior that's a single behavior snippet within our domain.

While Carl Sagan may be technically correct, the first step of an Apple Pie recipe doesn’t necessarily need to be: __Trigger the Big Bang__.

# Design Structure
<img src="/assets/ComposableDesignPatternsIntroduction.png" alt="Composable Design Patterns Template"  width = "30%" align="right" style="padding-right: 35px;">

Composite’s structure is the foundation for the other Composable Design Patterns.

When I presented an image for Composable patterns in [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html), I had to be careful not to present Composite itself.
 
Here is the Gang of Four’s (GoF) Composite design:
* The overall design looks like a version of [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) except for the propagating reference from `Composite` back up to `Component`.
* An interface, `Component`, resides at the top of the design which is consistent with the design patterns we've seen thus far.
* `Leaf` is a `Component`. I suspect that the GoF called it a _Leaf_, since all trees in this pattern terminate with a leaf node. _Terminal_ could have been another term, which the GoF will use for the same concept in a future pattern. If I weren’t using the GoF’s nomenclature, I might have considered calling it _Atomic_ in that these leaf/non-terminal nodes contain a single nugget of behavior that cannot be reduced to smaller parts. They are our Lego Bricks. They are our logic gates. They are the atoms of our periodic table.
* `Leaf` is a placeholder. In most designs, there will be many _Leaf_ classes. Each Lego Brick type could be its own leaf class. Each AND, OR and NOT gate could be its own leaf class. Each atom in the periodic table could be its own leaf class.
* `Composite` is the heart of the design. `Composite` is a `Component` that propagates to a list of `Component`s. It’s self-referential definition ensures that propagation traverses throughout the entire Composite object tree.
* The number of `components` within the `Composite` is not fixed, so the tree structure can expand as deep or as wide as needed. I only showed one mechanism for adding `components` to the `Composite`. There could be others. I also didn’t show additional mechanisms to read, update or delete `components`. A production quality design may want them as well.
* If more composite context is desired, then additional domain specific `Composite` classes could extend `Composite` or delegate to it, as we’ll see in the Use Case example.
* The `Client` does not know nor care whether its `Component` reference is a single `Leaf` object or a `Composite` object at the root of a tree with thousands of objects within it.
* There is no `Configurer` in the GoF’s design. This is probably my sole issue, since I feel this is a near perfect design pattern. Most `Leaf` classes have limited agency. They will provide a snippet functionality, but probably not too much. An individual Lego Brick has limited agency. A logic gate limited agency. But when configured appropriately, form and function emerge, such as the Lego Millennium Falcon or a fully functioning computer. The `Composite` may be the heart of the design, but the `Configurer` is the brain.

<img src="/assets/CompositeGoF.png" alt="GoF Composite Design Pattern"  width = "90%" align="center" style="padding-right: 35px;">
 
One more item. Where should “add” reside in the design? From the GoF:
> _Declaring the child management operations._ Although the Composite class implements the Add and Remove operations for managing children, an important issue in the Composite pattern is which classes declare these operations in the Composite class hierarchy. Should we declare these operations in the Component and make them meaningful for Leaf classes, or should we declare and define them only in Composite and its subclasses?

They argue that structural management placement is a tradeoff of transparency versus safety. Placing it in `Component` provides uniformity, but at the safety expense that someone may try to add component to a `Leaf` object.

Placing structural management in the `Composite` is safer, but now the structure management feature is missing from the main `Component` interface.

I feel that safety is more important than transparency, which is why my diagram places `add(Component component)` in the `Composite`. Here are my reasons:
* This is separation of concerns as described in [Computation and Coordination](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html#computation-and-coordination). They are two different responsibilities. The `Client` is interested in computation, which it achieves via its reference to `component.execute()`. The `Configurer` is interested in coordination, which it achieves via its references to `composite.add(component)`. The `Client` should not know nor care about how the tree is constructed. The `Configurer` already knows all configuration specifics, such as were to find `add`.
* Declaring `add(Composite composite)` in `Component` is stating that all classes that implement `Component` must provide an implementation for `add` and that includes the `Leaf` classes for which `add` is not a valid behavior. This is a violation of the [Liskov Substitution Principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle). What should `Leaf` do in its `add` implementation? Nothing? Throw an exception? There is no behavior that won’t cause confusion when attempting to add a `Component` to a `Leaf`. Therefore, don’t allow it in the first place.

Here are some examples of what the Composite object tree might look like. This first version is the simplest. The tree consists of one `leaf` node:

<img src="/assets/CompositeGoFObjectsA.png" alt="GoF Composite Design Pattern Objects One Node"  width = "30%" align="center" style="padding-right: 35px;">

Here’s a more complex tree with several `composite` and `leaf` nodes:

<img src="/assets/CompositeGoFObjectsB.png" alt="GoF Composite Design Pattern Objects Tree"  width = "60%" align="center" style="padding-right: 35px;">

In both examples, the `client` only has a reference to the root of the tree.

# Use Case
The Use Case is too large to include in this blog. It will be posted in the next blog.

# Composite Pros and Cons
The relative pros and cons of Composite are like those with most of the Composable design patterns.

## Pros
A small implementation can support many composable configurations. The objects in the composition can be as few or many as needed for the desired behavior. Each composable configuration features its own specific behavior.

Constructing the object composition is relatively easy. While the Configurer is mostly independent from the design pattern in its traditional form, the Configurer is still the brains that's responsible for the correct composition when using this design pattern. There are at least three types of Configurers and possibly more:
* __Developer Configurer__. This would be a developer with domain expertise as well as knowledge of the system and its architecture, design and implementation. The Developer Configurer may have also written the Composite implementation as well. Logic gate composites will probably be composed by a Developer Configurer.
* __Customer Consultant__. This would be a trained customer support consultant in the company who has knowledge of the product and works with customers. The customer consultant will configure customer desired behaviors on behalf of the customer. The customer consultant may work with the development team to better understand how to compose the Composite so that it meets the customer's needs. This person will most likely not be a developer; therefore, a UI/UX wrapper in front of the Composite may be necessary.
* __Customer/User__. This would be the customer or user who will need a UI/UX wrapper in front of the Composite. This provides self-service for the customer or user, which allows the customer/user to configurer the system to achieve their goals without having to depend upon a customer consultant or development team.

## Cons
I almost want to write that there’s no cons for Composite. I don’t think there are any cons in the implementation, but there’s a potential con with its intent.

Composite is all structure. Behavior derives from its composition, which technically resides in the Configurer, which resides outside of the design. Composite will support any structurally consistent composition, even those that don’t make logical sense. 

Anyone can grab random Lego blocks and snap them together. Logic gates can be wired together in any number of ways without providing any useful behaviors.

Behavior is in the eye of the Configurer, and the Configurer may need glasses. The Configurer could be a self-servicing customer/user who configures something that they think is correct, but it is not correct. We have all written code with software bugs. Configurers can just as easily configure composites with logical bugs in their structure. These Configurers will first accuse your implementation of containing a bug before admitting that the error resides in their configuration. _The fault, dear Brutus, lies not in our stars, but in ourselves._

Integration and acceptance testing won’t address these configuration issues either since they're a creation of the user.

# Summary
Composite allows us to compose snippets of behavior into a tree structure, such that behavior emerges from the shape and organization of the tree more than from any individual node.

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* [Wikipedia Composite Design Pattern](https://en.wikipedia.org/wiki/Composite_pattern)
* [Source Making Composite Design Pattern](https://sourcemaking.com/design_patterns/composite)
* [Refactoring Guru Composite Design Pattern](https://refactoring.guru/design-patterns/composite)
* [Project Management Institute Composite Design Pattern](https://www.pmi.org/disciplined-agile/the-design-patterns-repository/the-composite-pattern)
* [Baeldung Composite Design Pattern](https://www.baeldung.com/java-composite-pattern)
* [OODesign Composite Design Pattern](https://www.oodesign.com/composite-pattern)
* [Learn CS Design Composite Design Pattern](https://www.learncsdesign.com/learn-the-composite-design-pattern/)
* [DoFactory Composite Design Pattern](https://www.dofactory.com/net/composite-design-pattern)
* [Reactive Programming Composite Design Pattern](https://reactiveprogramming.io/blog/en/design-patterns/composite)
* and for more, Google: [Composite Design Pattern](https://www.google.com/search?q=Composite+Design+Pattern)

Here are some resources that can be purchased or are included in a subscription service:
* Gang of Four Composite Design Pattern, page 163 ([O'Reilly](https://learning.oreilly.com/library/view/design-patterns-elements/0201633612/ch04.html#page_163) and [Amazon](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612))
* Agile Principles, Patterns, and Practices in C#, Chapter 31 ([O'Reilly](https://learning.oreilly.com/library/view/agile-principles-patterns/0131857258/ch31.xhtml) and [Amazon](https://www.amazon.com/Agile-Principles-Patterns-Practices-C/dp/0131857258))
* Head First Design Patterns, Chapter 9 ([O'Reilly](https://learning.oreilly.com/library/view/head-first-design/9781492077992/ch09.html#idm46060277796400) and [Amazon](https://www.amazon.com/Head-First-Design-Patterns-Object-Oriented-ebook/dp/B08P3X99QP))
