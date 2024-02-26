---
title: Composite Design Pattern
description: Configure behavior emerging from a group of snippet behavior objects organized in a tree structure.
unlisted: true
---
<img src="https://upload.wikimedia.org/wikipedia/commons/c/c4/In-N-Out_Burger_20-patty_cheeseburger.jpg" alt="20x20 at In-N-Out Burger" title="Image Source: https://commons.wikimedia.org/wiki/File:In-N-Out_Burger_20-patty_cheeseburger.jpg" width = "60%" align="center" style="padding-right: 35px;">

# Introduction
[Composite Design Pattern](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) introduced the Composite Design Pattern, but it was getting a bit too long. This blog continues the story with a Use Case.

# Use Case – Can I get fries with that?
This Composite Use Case example is inspired by fast-food burger chains.

## Two all-beef patties …
McDonald's featured a popular Big Mac [jingle](https://www.youtube.com/watch?v=jSmAibfvCeU) in its commercials in my youth:
> Two all-beef patties, special sauce, lettuce, cheese, pickles, onions on a sesame seed bun.

It’s such a simple campaign that lists the ingredients of a Big Mac yet so memorable. My generation can still sing it.

McDonald's was known for its consistency. A Big Mac is a Big Mac everywhere. Consistency was even in their advertising by singing its ingredients. It may not be the best burger in the world, but you know what you are getting when you order one.

But this didn’t work so well for my family. My father and I were both picky eaters. Onions? Yuck! Pickles? Yuck! Don’t get me started with the special sauce that McDonald’s won’t even identify.

When we ate at McDonald's my father would order a plain burger with cheese. I’d order one with cheese and ketchup. My mother would order what they offered. Her order would be ready quickly, and then we waited as customer after customer would order and be served. Fast food was no necessarily fast for us.

## Hold the pickle, hold the lettuce …
Then Burger King came to town. They had a different [jingle](https://www.youtube.com/watch?v=KJXzkUH72cY):
> Hold the pickle, hold the lettuce,
> 
> Special orders don’t upset us.
> 
> All we ask is that you let us serve it your way.

Our special orders were always provided quickly. There was no waiting. Needless to say, my family always chose Burger King over McDonald’s when we had the choice. 

## Can you keep a secret?
In-N-Out Burger offers typical fast-food burger fare, but they also have a [secret menu](https://www.allrecipes.com/in-n-out-secret-menu-items-7558019). These are combinations you can request, which won’t be displayed on the menu including:
<img src="https://upload.wikimedia.org/wikipedia/commons/c/c4/In-N-Out_Burger_20-patty_cheeseburger.jpg" alt="20x20 at In-N-Out Burger" title="Image Source: https://commons.wikimedia.org/wiki/File:In-N-Out_Burger_20-patty_cheeseburger.jpg" width = "60%" align="right" style="padding-right: 35px;">
* 3x3: a triple cheeseburger
* 4x4: a quadruple cheeseburger
* Flying Dutchman: bun-less double cheeseburger
* Protein Style: lettuce wrapped burger
* Animal Fries: fries topped with burger condiments
* Cheese Fries: self-explanatory
* Roadkill Fries: Animal Fries topped with a Flying Dutchman
* And more …
 
And you can even customize to some degree, such as a 20x20 shown above.

## Fast-food and Software Design
These three burger joints are metaphors for several software design approaches:
* McDonald’s is like a traditional OO design. A Big Mac is well defined. It’s well implemented. But if you want something different, prepare to wait. It’s like the Project Manager asking a simple feature and being told that it will take 6 months.
* Burger King is like the [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) design pattern. The core feature is a burger on a bun. The condiments are the decorators. Any configuration of burger and condiments is as easy to produce as any other.
* In-N-Out Burger is something completely different. There is no core feature of a burger on a bun. Decorator isn’t quite the right fit. Some secret menu items omit the burger, and some omit the bun. And customers can customize something that’s not on any menu. In-N-Out Burger is like the Composite design pattern.

## Composite ala In-N-Out Burger
We could use Composite to address several features for In-N-Out Burger, but I’d like to focus upon a calorie count feature. How many calories are in each food item? The interface at the top of the design only needs to declare this:
```java
interface FoodItem {
    int getCalories();
}
```
This won’t be complex design, but it will take some space. I’m going split the design into multiple diagrams.

### Leaf Food Items
Let’s start with the _Leaf_ food items, and I don’t just mean lettuce. These will be the component parts available at In-N-Out Burger from which all possible combinations of foods can be assembled. Each will return how many calories are in them individually. This is not a complete list of ingredients, but it should be sufficient to convey what’s needed:

<img src="/assets/CompositeBurgerIngredients.png" alt="In-N-Out Burger Basic Ingredients"  width = "80%" align="center" style="padding-right: 35px;">
 
### Food Composites
This diagram adds `FoodComposite` to the design. It’s a list of `FoodItems`, and it obtains the total calorie count by summing the calories in each `FoodItem`. `Burger` and `Pickle` remain on this diagram as context to highlight how the _Leaf_ `FoodItems` and `FoodComposite` reside in the same design, but they have no dependency upon one another:

<img src="/assets/CompositeBurgerComposite.png" alt="In-N-Out Burger Composite"  width = "100%" align="center" style="padding-right: 35px;">
 
### Cheeseburger
The Cheeseburger gets interesting. I could have made it a more traditional design where the `Cheeseburger` class had field attributes for `Burger`, `Bun`, etc. I decided to leverage `FoodComposite` instead.

I could have also had `Cheeseburger` extend `FoodComposite`, but I decided not to for two reasons:
* The delegating approach is more in line with favoring object composition over inheritance.
* If `Cheeseburger` where a `FoodComposite` then more `FoodItem`s could be added to it. For now, I want `Cheeseburger` to remain a well-defined `Cheeseburger` that cannot be further modified by adding more `FoodItem`s to it. If we wanted to be able to enhance a `Cheeseburger`, we have several design options, including:
    * Define it to extend `FoodComposite`.
    * Instantiate a new `FoodComposite`, add a `Cheeseburger` to it, and then add any other `FoodItem` objects desired.
    * Declare a new `ModifiableCheeseburger` class, which basically does what I described in the second bullet, but now it’s encapsulated in a class.

`Cheeseburger` is an interesting class:
* It is a leaf `FoodItem`.
* It is not technically a `FoodComposite` but it contains and delegates to a `FoodComposite`.
* It’s a `Configurer`.
* `Cheeseburger` has no behavior specific implementation. It works by creating and assembling `FoodItem`s, adding them to a `FoodComposite`, and finally providing its behavior by delegating to the `FoodComposite`.

<img src="/assets/CompositeCheeseburger.png" alt="In-N-Out Burger Cheeseburger"  width = "90%" align="center" style="padding-right: 35px;">
 
Its composite tree would be:

<img src="/assets/CompositeCheeseburgerObjects.png" alt="In-N-Out Burger Cheeseburger Objects"  width = "75%" align="center" style="padding-right: 35px;">
 
Each line represents the a call to `getCalories()`:
* `Cheeseburger` delegates to the `cheeseburger:FoodComposite` to get its calorie count.
* `cheeseburger:FoodComposite`’s calorie count is the sum of its `FoodItem`s.
* Each leaf `FoodItem` returns its calorie value.
* The final `Cheeseburger` calorie value will be the sum of the leaf node calories.

This same propagation strategy replicates in the subsequent object trees, so I won’t repeat the verbiage.

### Animal Fries
Animal Fries follows the same Cheeseburger design:

<img src="/assets/CompositeAnimalFries.png" alt="In-N-Out Burger Animal Fries"  width = "90%" align="center" style="padding-right: 35px;">
 
Its composite tree would be:

<img src="/assets/CompositeAnimalFriesObjects.png" alt="In-N-Out Burger Animal Fries Objects"  width = "75%" align="center" style="padding-right: 35px;">
 
### Flying Dutchman
Flying Dutchman will do likewise:

<img src="/assets/CompositeFlyingDutchman.png" alt="In-N-Out Burger Flying Dutchman"  width = "90%" align="center" style="padding-right: 35px;">
 
Its composite tree would be:

<img src="/assets/CompositeFlyingDutchmanObjects.png" alt="In-N-Out Burger Flying Dutchman Objects"  width = "75%" align="center" style="padding-right: 35px;">
 
### Roadkill Fries
There is a method to my madness. Roadkill Fries are Animal Fries topped with a Flying Dutchman. We can build Roadkill Fries from the composites we already have:
* `AnimalFries` and `FlyingDutchman` are `FoodItem`s, and that’s all we need to know to design `RoadkillFries`.
* Each concrete class implements `FoodItem` and delegates to `FoodComposite`.

<img src="/assets/CompositeRoadkillFries.png" alt="In-N-Out Burger Roadkill Fries"  width = "90%" align="center" style="padding-right: 35px;">
 
Its composite tree would be:

<img src="/assets/CompositeRoadkillFriesObjects.png" alt="In-N-Out Burger Roadkill Fries Objects"  width = "100%" align="center" style="padding-right: 35px;">
 
The calorie count for `RoadkillFries` will be the sum of the calories for each of the non-terminal leaf nodes shown above.

### Learn the rules, and then break the rules
I’m not really breaking the rules here, but I’m really pushing the Composite boundary a bit.

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

The software at the In-N-Out Burger locations should support these and only these menu items, both regular and secret.

The hardcoded designs previously shown won’t be flexible enough to accommodate this.

There’s a flexible design with explanation following:

<img src="/assets/CompositeBurgerFactory.png" alt="In-N-Out Burger Roadkill Fries"  width = "100%" align="center" style="padding-right: 35px;">
 
This is still a Composite design at its core. The individual `FoodItem` classes for `Burger` et al. unchanged in this design from the previous ones. They are so simple that they apply as-is even when making a change such as this one. The specific hardcoded secret menu items are no longer present in this design.

Two new classes have been added to replace the previous hardcoded screte menu items: `FoodItemFactory` and `FoodItemBuilder`.

#### A Simple Request
Let’s start with the most basic request with a request for a Burger. It would look like this in code:
```java
FoodItem foodItem = FoodItemFactory.acquire(“Burger”);
```

`FoodItemFactory`’s switch statement would match on “Burger” and return a new `Burger` instance.

#### A More Complex Request
Let’s consider this:
```java
FoodItem foodItem = FoodItemFactory.acquire(“ProteinStyle”);
```

Unlike the previous example, `FoodItemFactory` won’t find a match for “ProteinStyle”. Its default behavior will return a `FoodItem` from the `FoodItemBuilder` for “ProteinStyle”. The `FoodItemBuilder` is a generic version of the hardcoded designs above such as `AnimalFries` and `FlyingDutchman`. They have the following in common:
* They all declare a `FoodComposite` field attribute.
* They all implement `getCalories()` by returning the `FoodComposite` field attribute’s `getCalories()`.
* They all populate `FoodComposite` with other `FoodItem` instances. The difference is that the previous design hardcoded the component construction, whereas this design retrieves the specification in the form of the list of `FoodItem`s, which it resolves via the `FoodItemFactory`.
* I didn’t have enough room to show the `FoodItemSpec` class. Its method will use the mapping examples shown above to return the list of `FoodItem`s for any secret menu item.

Since ProteinStyle is defined as `ProteinStyle => Burger|Lettuce`, `FoodItemBuilder` will iterate the ingredients with “Burger” and “Lettuce”. It will obtain a `Burger` and `Lettuce` instance from `FoodItemFactory` and add them to the `FoodComposite`.

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
A small implementation can support many composable options. The In-N-Out Burger example kept growing with menu options but take another look. There’s only one `for` loop implemented in the entire design residing in `FoodComposite`. Even the most complex design with `FoodItemFactory` and `FoodItemBuilder` is terse. Most of the implementation resides in a few lines of code.

The design is not limited to individual menu items. A `FoodComposite` can be created for each `FoodItem` in the order, such as: `Burger`, `Fries`, `Shake`, etc. And the design will return the calorie count of the entire order.

## Cons
I almost want to write that there’s no cons for Composite. I don’t think there are any cons in the implementation, but there’s a potential con with its intent.

Composite is all structure. Behavior derives from its composition, which technically resides in the `Configurer`, which is usually outside of the design. Composite will support any structurally consnstent composition, even those that don’t make any sense. There’s nothing to prevent someone from ordering 100 `Bun`s. Anyone can grab random Lego blocks and snap them together. Logic gates can be wired together in any number of ways without providing any useful behaviors

Behavior is in the eye of the configurer, and the configurer may need glasses. The configurer could be an external user who configures something that’s not exactly what they wanted. They will pass judgement upon your implementation insisting that there's a bug in it before admitting that it resides in their configuration. Integration and acceptance testing won’t address these issues either, since it’s a creation of the user.

# Summary
Composite allows us to compose snippets of behavior into a tree structure, such that behavior emerges from the shape and organization of the tree more than from any individual node.

# References
See: [Composite Design Pattern/References](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html#References).
