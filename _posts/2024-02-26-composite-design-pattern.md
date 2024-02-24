---
title: Composite Design Pattern
description: Configure behavior emerging from a group of snippet behavior objects organized in a tree structure.
unlisted: true
---
<img src="https://live.staticflickr.com/7154/6838743675_d5c4b4e6cd_b.jpg" alt="Jet Engine Cutaway" title="Image Source: [https://www.flickr.com/photos/davegray/6463738329](https://www.flickr.com/photos/multiplyleadership/6838743675
)" width = "60%" align="center" style="padding-right: 20px;">

# Introduction
The Composite Design Pattern is the next of the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html)  series.

The Composite Design Pattern embodies the concept of composable design patterns more completely than the previous patterns we’ve seen. Several concepts first introduced [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html)   will be revisited here as well.

Let’s review the composable patterns we’ve seen so far:
* [Proxy](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html), which featured a wrapper class in front of a core feature class.
* [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html), which featured a list of decorator objects that adorned a core feature with additional behavior. It’s a dynamically composable alternative to inheritance.
* [Chain of Responsibility](https://jhumelsine.github.io/2024/02/20/chain-of-responsibility-design-pattern.html), which featured a list of handlers, each of which has the potential to complete a task, such that the first handler in the list which can complete task is the one that does complete the task. It’s a dynamically composable alternative to `switch`/`if/else-if/else` statements.

The previous composable design patterns are based upon a linear structure. They use single delegation or a list as their data structure mechanism. Each tends to focus upon one core feature class, which is enhanced by other supporting classes composed around it. One could argue that Chain of Responsibility is a list of core feature classes, since only one is chosen to do work. But for CoR, the composition is about the choosing of the core feature class.

Composite is all about composition. There is no single core feature class. Composite features several classes each providing a snippet of behavior. Objects implementing those snippets of behavior are composed into _a core_ behavior that’s comprised of an aggregation of those object snippets organized via Composite. There could be many behaviors depending upon the organization of the object snippets.

Composite breaks the shackles of a linear structure. Composite’s underlying data structure is a self-referential tree. A single code base can accommodate a Composite tree of objects that’s as wide or as deep as needed for the given situation. The Composite tree could be one object or thousands of objects.

Composition is not a foreign concept in OO. It’s the HAS-A relationship, as in:
* A Car HAS-A(n) Engine.
* A Chordate HAS-A Spine.
* A Public Company HAS-A set of C Suite Officers.

HAS-A relationships are often rendered in OO programming language as a private field attribute class or a collection of classes.
The other primary OO relationship is IS-A, which is traditionally rendered with inheritance.

Just as Decorator is a dynamically composable alternative to inheritance, Composite is a dynamically composable alternative to traditional rendering of HAS-A relationships in OO design.

Traditional HAS-A implementation tends to be statically locked into place at compile time. The HAS-A relationships are hardcoded as field attributes. There is nothing wrong with this technique, especially with a domain for which these relationships do not change. Cars will always have engines. Chordates will always have spines. I often used this technique.

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

I would probably model a jet engine more traditionally with the concept that: JetEngine HAS-A Compressor, JetEngine HAS-A Turbine and even more decomposition with Turbine HAS-A set of Blades, etc. rather than Composite, since the composition of a Jet Engine is not likely to change frequently.

The purpose of my example is to highlight the compositional HAS-A nature of a Jet Engine regardless of how one might model it using a traditional OO approach or Composite. The Compressor and Turbine really do not exist as a single entity. Heck, even the Jet Engine does not really exist. They are the compositional sum of their components in a specific configuration. While we often refer to this relationship as HAS-A, it’s closer to IS-COMPRISED-OF.

## Logic Gates
<img src="https://upload.wikimedia.org/wikipedia/commons/1/16/Four_bit_adder_with_carry_lookahead.svg" alt="Full Adder Logic Gates" title="Image Source: https://www.wikiwand.com/en/Logic_gate#Media/File:Four_bit_adder_with_carry_lookahead.svg" width = "30%" align="right" style="padding-right: 35px;">

The [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) blog featured an image of logic gates for a four-bit adder.

A Multi-bit Adder is a composite of Full Adder components, which are each a composite of AND, OR and NOT logic gates. The Multi-bit Adder and the Full Adders are composite configurations of these three simple logic gates. This applies to almost all logical components of a computer. They only have meaning as a concept because we choose to assign them meaning based upon the behaviors that emerge from their configurations.

## Lego Brick Sets
Lego has been a popular toy for decades. The bricks were mostly generic when I was a child. Since then, Lego has expanded to specific themed sets often tied in with marketing and merchandizing of other brands, such as Star Wars themed Lego sets.

There are hybrid sets too. My son had a Police Boat set which featured a specific configuration on the front cover of the box, with step-by-step instructions on how to build the featured Police Boat. But the back cover provided photos of alternative boats that could be assembled from the same bricks. Other than the photos, these alternative boats were left up to the child’s imagination.

No individual Lego Brick has much agency. Form doesn’t emerge until the bricks are snapped together to create a cohesive structure. 

<img src="https://live.staticflickr.com/7057/7053151615_0e25fcf31d_o.jpg" alt="Lego Millennium Falcon" title="Image Source: https://www.flickr.com/photos/stickkim/7053151615" width = "40%" align="left" style="padding-right: 35px;">

You embark upon a new Lego project. You start with thousands of individual bricks which will eventually become the Millennium Falcon. They are assembled in stages. Individual bricks are components assembled into composite ship parts. These composite parts are assembled into even larger composites. Eventually the composites become identifiable parts of the ship, such as the cockpit, landing gear, engines, etc. After hours of assembly, the final parts are snapped together, and you have the Millennium Falcon.

<img src="https://live.staticflickr.com/4764/39195349224_39bf74764a_o.png" alt="Lego Patent" title="Image Source: https://www.flickr.com/photos/brickset/39195349224" width = "30%" align="right" style="padding-right: 35px;">

Even though the individual bricks have different shapes and sizes, they follow the consistent Lego snapping mechanism, which has been their common interface since the beginning. All bricks, regardless of their sets, can snap together with other bricks. There are even [apps](https://petapixel.com/2021/07/01/brickits-ai-camera-scans-your-lego-to-suggest-things-you-can-build/) that will use AI to create new projects with instructions from a photo of random bricks.

These properties aren’t unique to Lego Bricks. They apply to many construction toys including:
* [Tinkertoys](https://en.wikipedia.org/wiki/Tinkertoy)
* [Lincoln Logs](https://en.wikipedia.org/wiki/Lincoln_Logs)
* [Erector Sets](https://en.wikipedia.org/wiki/Erector_Set)

## Cooking
One of first blogs featured cooking – [Knock Knock … Who’s There?]( https://jhumelsine.github.io/2023/08/21/knock-knock-whos-there.html).

Meals are a composite of dishes, which are a composite of ingredients, which are a composite of molecules, which are ….

How far do we take breakdown components into composites? Where do we stop?

“If you wish to make an apple pie from scratch, you must first invent the universe.” – [Carl Sagan]( https://www.youtube.com/watch?v=5_vVGPy4-rc).

With all deference to Carl Sagan, we don’t have to go that far, which I’ll address later.

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
But why stop with atoms above? Atoms are composites of protons, neutrons and electrons. Protons are composites of quarks. Who knows? Maybe quarks and other subatomic particles are composites of other components not yet identified.

Composite’s self-referential tree structure means that we can easily ride these composite turtles a long way down _ad absurdum_.

<img src="https://live.staticflickr.com/4031/4288661567_dceef21cfb_o.jpg" alt="Sagan Apple Pie Recipe" title="Image Source: https://www.flickr.com/photos/neven/4288661567" width = "40%" align="right" style="padding-right: 35px;">

One could argue that since all the previous composite examples are based upon materials in the real world, they all could be reduced to subatomic particles. But unless we’re writing software for the particle collider, we’re probably not concerned with subatomic particles in our models.

We stop when we've reached behavior that makes sense as a single behavior within our domain.

While Carl Sagan may be technically correct, the first step of an Apple Pie recipe doesn’t necessarily need to be: __Trigger the Big Bang__.

# Design Structure
<img src="/assets/ComposableDesignPatternsIntroduction.png" alt="Composable Design Patterns Template"  width = "30%" align="right" style="padding-right: 35px;">

Composite’s structure is the foundation for the other Composable Design Patterns.

When I presented an image for Composable patterns in [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html), I had to be careful not to present Composite itself.
 
Most of the properties in the general composable example will be seen here once more with Composite. Here is the Gang of Four’s (GoF) Composite design:
* An interface, `Component`, resides at the top of the design which is consistent with the other composable design patterns.
* `Leaf` is a `Component`. I suspect that the GoF called it a _Leaf_, since all trees in this pattern terminate with a leaf node. _Terminal_ could have been another term, which the GoF will use for the same concept in a future pattern. If I weren’t using the GoF’s nomenclature, I might have considered calling it _Atomic_ in that these leaf/non-terminal nodes contain a single nugget of behavior that cannot be reduced to smaller parts. They are our Lego Bricks. They are our logic gates. They are the atoms of our periodic table.
* `Leaf` is a placeholder. In most designs, there will be many _Leaf_ classes. Each Lego Brick could be its own class. Each AND, OR and NOT gate could be its own class. Each atom in the periodic table could be its own class.
* `Composite` is the heart of the design. `Composite` is a `Component` that propagates to a list of `Component`s. It’s self-referential definition ensures that propagation traverses throughout the entire Composite object tree.
* The number of `components` within the `Composite` is not fixed, so the tree structure can expand as deep or as wide as needed. I only showed one mechanism for adding `components` to the `Composite`. There could be others. I also didn’t show additional mechanisms to read, update or delete `components`. A production quality design may want them as well.
* If more composite context is desired, then additional domain specific `Composite` classes could extend `Composite` or delegate to it, as we’ll see in the Use Case example.
* The overall design looks like a version of [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) except for the propagating reference from `Composite` back up to `Component`.
* The `Client` does not know nor care whether its `Component` reference is a single `Leaf` object or a `Composite` object at the root of a tree with thousands of objects within it.
* There is no `Configurer` in the GoF’s design. This is probably my sole issue, since I feel this is a near perfect design pattern. Most `Leaf` classes have limited agency. They will provide a snippet functionality, but probably not too much. An individual Lego Brick has limited agency. A logic gate limited agency. But when configured appropriately form and function emerge, such as the Millennium Falcon or a fully functioning computer. The `Composite` may be the heart of the design, but the `Configurer` is the brain.

<img src="/assets/CompositeGoF.png" alt="GoF Composite Design Pattern"  width = "80%" align="center" style="padding-right: 35px;">
 
One more item. Where should “add” reside in the design? From the GoF:
> Declaring the child management operations. Although the Composite class implements the Add and Remove operations for managing children, an important issue in the Composite pattern is which classes declare these operations in the Composite class hierarchy. Should we declare these operations in the Component and make them meaningful for Leaf classes, or should we declare and define them only in Composite and its subclasses?

They argue that structural management placement is a tradeoff of transparency versus safety. Placing it in `Component` provides uniformity, but at the safety expense that someone may try to add component to a `Leaf` object.

Placing structural management in the `Composite` is safer, but now this feature is missing from the main `Component` interface.

I feel that safety is more important than transparency, which is why my diagram places `add(Component component)` in the `Composite`. Here are my reasons:
* This is separation of concerns, with respect to [Computation and Coordination](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html#computation-and-coordination). They are two different responsibilities. The `Client` is interested in computation, which it achieves via its reference to `component.execute()`. The `Configurer` is interested in coordination, which it achieves via its references to `composite.add(component)`.
* Declaring `add(Composite composite)` in `Component` is stating that all classes that implement `Component` must provide an implementation for `add` and that includes the `Leaf` classes for which `add` is not a valid function. This is a violation of the [Liskov Substitution Principle](https://en.wikipedia.org/wiki/Liskov_substitution_principle). What should `Leaf` do in its `add` implementation? Nothing? Throw an exception? There is no behavior that won’t cause confusion when attempting to add a `Component` to a `Leaf`. Therefore, don’t allow it in the first place.

Here are some examples of what the Composite object tree might look like. This first version is the simplest. The tree consists of one `leaf` node:

<img src="/assets/CompositeGoFObjectsA.png" alt="GoF Composite Design Pattern Objects One Node"  width = "30%" align="center" style="padding-right: 35px;">

Here’s a more complex tree with several `composite` and `leaf` nodes:

<img src="/assets/CompositeGoFObjectsB.png" alt="GoF Composite Design Pattern Objects Tree"  width = "80%" align="center" style="padding-right: 35px;">

In both examples, the `client` has a reference to the root of the tree.

# Use Case – Can I get fries with that?
This Chain of Responsibility Use Case example is inspired by fast-food burger chains.

## Two all-beef patties …
McDonald's featured a popular Big Mac [jingle](https://www.youtube.com/watch?v=jSmAibfvCeU) in its commercials in my youth:
> Two all-beef patties, special sauce, lettuce, cheese, pickles, onions on a sesame seed bun.

It’s such a simple campaign listing the ingredients of a Big Mac. My generation could sing then and now too.

McDonald's was known for its consistency. A Big Mac was a Big Mac everywhere. Consistency was even in their advertising. It may not be the best burger in the world, but you know what you are getting when you order one.

But this didn’t work so well for my family. My father and I were both picky eaters. Onions? Yuck! Pickles? Yuck! Don’t get me started with the special sauce that McDonald’s won’t even identify. My father would order a plain burger with cheese. I’d order one with cheese and ketchup. Then we waited while customer after customer would order and be served. Fast food was no necessarily fast for us.

## Hold the pickle, hold the lettuce …
Then Burger King came to town. They had a different [jingle](https://www.youtube.com/watch?v=KJXzkUH72cY):
> Hold the pickle, hold the lettuce,
> 
> Special orders don’t upset us.
> 
> All we ask is that you let us serve it your way.

Our special orders were always provided quickly. There was no waiting. Needless to say, my family always chose Burger King over McDonald’s when we had the choice. 

## Can you keep a secret?
In-N-Out Burger offers typical fast-food burger fare, but they also have a _not so_ [secret menu](https://www.allrecipes.com/in-n-out-secret-menu-items-7558019). These are combinations you can request, which won’t be displayed on the menu including:
<img src="https://upload.wikimedia.org/wikipedia/commons/c/c4/In-N-Out_Burger_20-patty_cheeseburger.jpg" alt="20x20 at In-N-Out Burger" title="Image Source: https://commons.wikimedia.org/wiki/File:In-N-Out_Burger_20-patty_cheeseburger.jpg
" width = "50%" align="right" style="padding-right: 35px;">
* 3x3: a triple cheeseburger
* 4x4: a quadruple cheeseburger
* Flying Dutchman: bun-less double cheeseburger
* Protein Style: lettuce wrapped burger
* Animal Fries: fries topped with burger condiments
* Cheese Fries: self-explanatory
* Roadkill Fries: Animal Fries topped with a Flying Dutchman
* And more …
 
And you can even customize to some degree, such as a 20x20.

## Fast-food and Software Design
These three burger joints are metaphors for several software design considerations:
* McDonald’s is like a traditional OO design. A Big Mac is well defined. It’s well implemented. But if you want something different, prepare to wait. It’s like the Project Manager asking a simple feature and being told that it will take 6 months.
* Burger King is like the [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) design pattern. The core feature is a burger on a bun. The condiments are the decorators. Any configuration of burger and condiments is as easy to produce as any other.
* In-N-Out Burger is something completely different. There is no core feature of a burger on a bun. Decorator isn’t quite a good fit. Some secret menu items omit the burger, and some omit the bun. And customers can customize something that’s not on any menu. In-N-Out Burger is like the Composite design pattern.

## Composite ala In-N-Out Burger
We could use Composite to address several features for In-N-Out Burger, but I’d like to focus upon calorie count. How many calories are in each food item? The interface at the top of the design only needs to declare this:
```java
interface FoodItem {
    int getCalories();
}
```
This won’t be complex design, but it will take some space. I’m going split the design into multiple diagrams.

### Leaf Food Items
Let’s start with the _Leaf_ food items, and I don’t just mean lettuce. These will be the component parts available at In-N-Out Burger from which all possible combinations of foods can be assembled. Each will return how many calories are in each individually. This is not a complete list of ingredients, but it should be sufficient to convey what’s needed:

<img src="/assets/CompositeBurgerIngredients.png" alt="In-N-Out Burger Basic Ingredients"  width = "80%" align="center" style="padding-right: 35px;">
 
### Food Composites
This diagram adds `FoodComposite` to the design. It’s a list of `FoodItems` and it obtains the total calorie count by summing the calories in each `FoodItem`. `Burger` and `Pickle` remain on this diagram for context to highlight how the _Leaf_ `FoodItems` and `FoodComposite` reside in the same design, but they have no dependency upon one another:

<img src="/assets/CompositeBurgerComposite.png" alt="In-N-Out Burger Composite"  width = "80%" align="center" style="padding-right: 35px;">
 
### Cheeseburger
The Cheeseburger gets interesting. I could have made it a more traditional design where the `Cheeseburger` class had field attributes for `Burger`, `Bun`, etc. I decided to leverage `FoodComposite` instead.

I could have also had `Cheeseburger` extend `FoodComposite`, but I decided not to for two reasons:
* This approach is more in line with favoring object composition over inheritance.
* If `Cheeseburger` where a `FoodComposite` then more `FoodItem`s could be added to it. For now, I want `Cheeseburger` to remain a well-defined `Cheeseburger` that cannot be further modified by adding more `FoodItem`s to it. If we wanted to be able to enhance a `Cheeseburger`, we have several design options, including:
    * Define it to extend `FoodComposite`.
    * Instantiate a new `FoodComposite`, add a `Cheeseburger` to it, and then add any other `FoodItem` objects desired.
    * Declare a new `ModifiableCheeseburger` class, which basically does what I described in the second bullet, but now it’s encapsulated in a class.

`Cheeseburger` is an interesting class:
* It is a leaf `FoodItem`.
* It is not technically a `FoodComposite` but it contains and delegates to a `FoodComposite`.
* It’s a `Configurer`.
* `Cheeseburger` has no behavior specific implementation. It works by creating and assembling `FoodItem`s, add them to a `FoodComposite`, and finally implement its behavior by delegating to the `FoodComposite`.

<img src="/assets/CompositeCheeseburger.png" alt="In-N-Out Burger Cheeseburger"  width = "80%" align="center" style="padding-right: 35px;">
 
Its composite tree would be:

<img src="/assets/CompositeCheeseburgerObjects.png" alt="In-N-Out Burger Cheeseburger Objects"  width = "80%" align="center" style="padding-right: 35px;">
 
Each line represents the a call to `getCalories()`:
* `Cheeseburger` delegates to the `cheeseburger:FoodComposite` to get its calorie count.
* `cheeseburger:FoodComposite`’s calorie count is the sum of its `FoodItem`s.
* Each leaf `FoodItem` returns its calorie value.
* The final `Cheeseburger` calorie value will be the sum of the leaf node calories.

This same propagation strategy replicates in the subsequent object trees, so I won’t repeat the verbiage.

### Animal Fries
Animal Fries follows the same Cheeseburger design:

<img src="/assets/CompositeAnimalFries.png" alt="In-N-Out Burger Animal Fries"  width = "80%" align="center" style="padding-right: 35px;">
 
Its composite tree would be:

<img src="/assets/CompositeAnimalFriesObjects.png" alt="In-N-Out Burger Animal Fries Objects"  width = "80%" align="center" style="padding-right: 35px;">
 
### Flying Dutchman
Flying Dutchman will do likewise:

<img src="/assets/CompositeFlyingDutchman.png" alt="In-N-Out Burger Flying Dutchman"  width = "80%" align="center" style="padding-right: 35px;">
 
Its composite tree would be:

<img src="/assets/CompositeFlyingDutchmanObjects.png" alt="In-N-Out Burger Flying Dutchman Objects"  width = "80%" align="center" style="padding-right: 35px;">
 
### Roadkill Fries
There is a method to my madness. Roadkill Fries are Animal Fries with a Flying Dutchman on top. We can build Roadkill Fries from the composites we already have:
* `AnimalFries` and `FlyingDutchman` are `FoodItem`s, and that’s all we need to know to design `RoadkillFries`.
* Each concrete class implements `FoodItem` and delegates to `FoodComposite`.

<img src="/assets/CompositeRoadkillFries.png" alt="In-N-Out Burger Roadkill Fries"  width = "80%" align="center" style="padding-right: 35px;">
 
Its composite tree would be:

<img src="/assets/CompositeRoadkillFriesObjects.png" alt="In-N-Out Burger Roadkill Fries Objects"  width = "80%" align="center" style="padding-right: 35px;">
 
The calorie count for `RoadkillFries` will be the sum of the calories for each of the non-terminal leaf nodes shown above.

### Learn the rules, and then break the rules
I’m not really breaking the rules here, but I’m really pushing the boundary. I just couldn’t help myself.

The previous In-N-Out Burger design examples are hardcoded. Each secret menu item requires its own class. It’s not a difficult class to implement, but it’s not necessarily a flexible design either.

Let’s assume that In-N-Out Burger Corporate wants a more dynamic secret menu. They want the ability add, update or delete secret menu items at any time.

Corporate manages a Secret Menu Specification. It will be of the form:
* `3x3 => Burger|Burger|Burger|Cheese|Cheese|Cheese|Bun|Lettuce|Tomato|Onions|Pickle`
* `4x4 => Burger|Burger|Burger|Burger|Cheese|Cheese|Cheese|Cheese|Bun|Lettuce|Tomato|Onions|Pickle`
* `FlyingDutchman => Burger|Burger|Cheese|Cheese`
* `ProteinStyle => Burger|Lettuce`
* `AnimalFries => Fries|Mustard|Onions|Cheese`
* `CheeseFries => Fries|Cheese`
* `RoadkillFries => AnimalFries|FlyingDutchman`

The software at the In-N-Out Burger locations should support these and only these menu items.

The hardcoded designs previously shown won’t be flexible enough to accommodate this.

There’s a flexible design with explanation following:

<img src="/assets/CompositeBurgerFactory.png" alt="In-N-Out Burger Roadkill Fries"  width = "80%" align="center" style="padding-right: 35px;">
 
This design is still Composite at its core. The individual `FoodItem` classes for `Burger` et al. remain in the design. The specific hardcoded secret menu items are no longer present.

Two new classes have been added `FoodItemFactory` and `FoodItemBuilder`.

#### Simple Request
Let’s start with the most basic request with a request for a Burger. It would look like this in code:
```java
FoodItem foodItem = FoodItemFactory.acquire(“Burger”);
```

`FoodItemFactory`’s switch statement would match on “Burger” and return a new `Burger` instance.

#### More Complex Request
Let’s consider this:
```java
FoodItem foodItem = FoodItemFactory.acquire(“ProteinStyle”);
```

Unlike the previous example, `FoodItemFactory` won’t find a match for “ProteinStyle”. Its default behavior will return a `FoodItem` from the `FoodItemBuilder` for “ProteinStyle”. The `FoodItemBuilder` is a generic version of the hardcoded designs above such as `AnimalFries` and `FlyingDutchman`. They have the following in common:
* They all declare a `FoodComposite` field attribute.
* They all implement `getCalories()` by returning the `FoodComposite` field attribute’s `getCalories()`.
* They all populate `FoodComposite` with other `FoodItem` instances. The difference is that the previous design hardcoded the component construction, whereas this design retrieves the specification in the form of the list of `FoodItem`s, which it resolves via the `FoodItemFactory`.
* I didn’t have enough room to show the `FoodItemSpec` class. Its method will use the mapping examples shown above to return the list of `FoodItem`s for any secret menu item.
Since ProteinStyle is defined as:
* `ProteinStyle => Burger|Lettuce`
`FoodItemBuilder` will iterate the ingredients with “Burger” and “Lettuce”. It will obtain a `Burger` and `Lettuce` instance from `FoodItemFactory` and add them to the `FoodComposite`.

#### Hey, Watch This
Now consider this:
```java
FoodItem foodItem = FoodItemFactory.acquire(“RoadkillFries”);
```

RoadkillFries are defines as:
* `RoadkillFries => AnimalFries|FlyingDutchman`

When `FoodItemBuilder` acquires a `FoodItem` for each of these two from the `FoodItemFactory`, it will encounter the default case for both, which will build two new `FoodItemBuilder`, but this time for “AnimalFries” and “FlyingDutchman” separately. They are defined as:
* `AnimalFries => Fries|Mustard|Onions|Cheese`
* `FlyingDutchman => Burger|Burger|Cheese|Cheese`

Each new `FoodItemBuilder` will acquire the specification and iterate each of the elements each of which can now be found in `FoodItemFactory` switch statement, and the recursion resolves.

#### Caveats
I didn’t have enough room for edge cases. Production quality design would require exceptions when Strings are not found.
Beware of bugs in the above design; I have only proved it correct, not tried or tested it.

#### Flexibility
This design is quite flexible. In-N-Out Burger Corporate could easily add this to their secret menu:
* `BigMac => Burger|Burger|SpecialSauce|Lettuce|Cheese|Pickles|Onions|SesameBun`

# Composite Pros and Cons
The relative pros and cons of Composite are like those with most of the Composable design patterns.

## Pros
A small implementation can support many composable options. The In-N-Out Burger example kept growing with menu options but take another look. There’s only one `for` loop implemented in the entire design residing in `FoodComposite`.

The design is not limited to individual menu items. A `FoodComposite` can be created for each `FoodItem` in the order, such as: `Burger`, `Fries`, `Shake`, etc. And the design will return the calorie count of the entire order.

## Cons
I almost want to write that there’s no cons for Composite. I don’t think there are any cons in the implementation, but there’s a potential con with its intent.

Composite is all structure. Behavior derives from its composition, which technically resides in the `Configurer`, which is usually outside of the design. Composite will support any composition, even those that don’t make any sense. There’s nothing to prevent someone from ordering 100 `Bun`s.
Behavior is in the eye of the configurer, and the configurer may need glasses. The configurer could be an external user who configures something that’s inconsistent with what they wanted. They will pass judgement that the problem resides in your implementation before admitting that it resides in their configuration. Integration and acceptance testing won’t address this, since it’s a creation of the user.

# Summary
Composite is …

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* TBD

Here are some resources that can be purchased or are included in a subscription service:
* TBD
