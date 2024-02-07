---
title: Decorator Design Patterns
description: Layer additional behaviors upon core features.
unlisted: true
---
<img src="https://i5.walmartimages.com/seo/Potato-Head-Mr-Potato-Head-Classic-Toy-For-Kids-Ages-2-and-Up-Includes-13-Parts-and-Pieces-to-Create-Funny-Faces_0707e86c-0291-454b-a656-0cbfc7ca86b1.fedf9869656cdea9c0a454df4fccf3f4.jpeg" alt="Mr. Potato Head" title="Image Source: https://www.amazon.com/Mr-Potato-Head-Classic-Pieces/dp/B08X1RYZ1R" width = "50%" align="center" style="padding-right: 35px;">
 
# Introduction
The Decorator Design Pattern is the next of the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html)  series.

Inheritance enhances existing functionality often by adding new or modifying existing behaviors to existing classes, but this comes with some constraints:
* The extending classes have knowledge of and depend upon their parent classes.
* Behavior is static, and it cannot be adjusted once declared.
* For some languages, such as Java, there’s only one inheritance lineage. There is no multiple inheritance. This makes mixing and matching of behaviors more challenging. Multiple inheritance supporting languages, such as C++, have their own issues with the [diamond problem](https://en.wikipedia.org/wiki/Multiple_inheritance#The_diamond_problem).

I indicated in the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) blog that Composable Design Patterns could be an alternative for inheritance via delegation.

We can view an inheritance tree as a type of delegation, where behaviors are executed via inheritance from a child class up through its ancestor classes. Child class methods can override ancestor methods while still accessing the ancestor method, which can be done via an explicit parent call, such as with `super` in Java.

Delegation is one object calling the method of another object via an object reference. We saw this with the [Proxy Design Pattern](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html) where the `Proxy` objects delegated to the `CoreFeature` object.

Decorator expands delegation further than Proxy. Rather than one object delegating to another object, it’s structured as a linked list of objects delegating down the chain. A `coreFeature` object anchors the chain as the final object in the chain. The decorator objects adorn the `coreFeature` functionality.
This type of delegation is slightly differnt than object `a` calling a method of object `b`, and then `b` calls a different method of object `c`, etc.
In Decorator, the objects delegate down the chain calling the same method from start to finish.

Inheritance delegates up through ancestor classes statically. Decorator delegates through a list of objects dynamically.

We’ll see that Decorator addresses some of the issues with inheritance:
* The concrete classes in the Decorator Design Pattern have no knowledge of or dependency upon one another.
* Behavior is dynamic and can be configured and changed at runtime.
* The Decorator list can include as many or as few individual behaviors Decorator objects as needed to achieve the overall desired behavior.
  
# Real World Analogies to Decorator
Decorator is not difficult to implement. Its main challenge is comprehension. Here are a few real-world examples to ease one into the Decorator concept.

## Mr. Potato Head
<img src="https://m.media-amazon.com/images/I/8120J11F0pL._AC_SX569_.jpg" alt="Mr. Potato Head with Appendages" title="Image Source: https://www.amazon.com/Playskool-Potato-Head-Suitcase-Toddler/dp/B0050QJTBA" width = "35%" align="right" style="padding-right: 20px;">

[Mr. Potato Head](https://en.wikipedia.org/wiki/Mr._Potato_Head) is a favorite childhood toy and Disney Toy Story character. But he’s also a good metaphor for the Decorator Design Pattern.

Mr. Potato Head has been a favorite for many children including me in my youth. The plain potato core feature isn’t much of a toy. The fun came in decorating the potato with caricature appendages, such as eyes, mouth, nose, ears, feet, hands, and hats. Kids could create Mr. Potato Head with different expressions depending upon the appendages added. And inevitably, noses would be placed in the ear holes, hands in the hat holes, etc.

The original version of the toy only included the Decorator appendages. The child needed a real potato, probably provided by mom.
After too many rotting Mr. Potato Heads transformed into <span style="font-family:Chiller;">__Mr. Potato Zombie__</span>, Hasbro provided a plastic potato, which also doubled as convenient storage for the appendages.

Mr. Potato Head is a Decorator. The Core Feature is the potato. The Decorators are the caricature appendages added to it.

## Food Orders with Options
Lots of foods demonstrate Decorator.

### I Scream For Ice Cream
<img src="/assets/DecoratorIceCream.JPG" alt="Squealing with Delight at Ice Cream" width = "30%" align="right" style="padding-right: 20px;">

My family ate at a Disney World restaurants on a vacation years ago. My 4-year-old son ordered an ice cream sundae for dessert. He practically squealed with delight when our server placed it in front of him. It was a bowl of vanilla ice cream with half a dozen plastic cups of toppings ringed around it. Of course, he dumped all the toppings on the ice cream before devouring it.

The ice cream is the Core Feature, and the toppings are the Decorators.

### Pizza
Most pizzas begin with a crust, sauce, and cheese. Then customers can add toppings as desired, such as pepperoni, mushrooms, olives, and … pineapple.

The basic pizza is the Core Feature, and the toppings are the Decorators.

## Multiplane Camera
<img src="https://upload.wikimedia.org/wikipedia/en/e/ef/Sketch_of_a_Multiplane_camera_with_motorized_movements.gif?20200218220911" alt="Multiplane Camera" title="Image Source: https://en.wikipedia.org/wiki/File:Sketch_of_a_Multiplane_camera_with_motorized_movements.gif" width = "35%" align="right" style="padding-right: 20px;">

Most cartoon animations consist of a background image with the cartoon characters drawn on clear [celluloid sheets](https://en.wikipedia.org/wiki/Cel), i.e., cels. The cels are placed on top of the background image and photographed for one frame of the animation. The next frame only requires a new cel with small updated character movements. There's no need to redraw the background.

This animation technique is a version of Decorator, well  maybe closer to Proxy since there’s usually only one character cel, but the [Multiplane Camera](https://en.wikipedia.org/wiki/Multiplane_camera) expands this technique into Decorator territory. 

In the Multiplane Camera, there’s a background image. Layers of cels are suspended above with space between each layer. A camera, suspended above them all, captures a frame that’s a composition of the layered cels. The space space between layers provides a 3-D effect.

Walt Disney explains the Multiplane Camera in this [2-minute video](https://www.youtube.com/watch?v=kN-eCBAOw60) from 1957.

The Multiplane Camera is a Decorator. The Core Feature is the background image. The Decorators are the cel layers stacked on top of it.

## SLR Camera with Lenses and Filters
<img src="https://m.media-amazon.com/images/I/91eFHyuoSLL._AC_SX425_.jpg" alt="SLR Camera Kit" title="Image Source: https://www.amazon.com/Canon-EOS-4000D-420-800mm-Professional/dp/B0CPL216F3/ref=asc_df_B0CPL216F3" width = "25%" align="right" style="padding-right: 20px;">

An SLR Camera consists of a camera body with a lens attached to it. Most SLR Cameras come with a default lens, but the photographer isn’t restricted to just that one lens. Additional lenses can be purchased, such as telephoto, wide angle, etc. The lenses can be attached to the camera body based upon the desired shot. Additional filters can be added to for different effects with color and lighting.

An SLR Camera package is a Decorator. The Core Feature is the camera body. The Decorators are the lenses and filters. Unlike other Decorator examples, the camera body requires at least one Decorator lens to work.

## Photoshop Layers
Photoshop is the digital version of layering concept from the Multiplane Camera. A final image is composed of stacked virtual layers of image content. However, unlike the Multiplane Camera, all the layers are directly on top of one another. Here’s an [11-minute video](https://www.youtube.com/watch?v=3gxcIn8L__w) demonstrating the feature.

Photoshop Layers is a Decorator. The background image is the Core Feature. Additional layers are the Decorators on top of it.

# Design Structure
Now that we have a taste of Decorators in the real world, let’s see how the pattern is structured in software.

## Proxy transformation into Decorator
Decorator’s design structure is mostly an extension of the design structures we’ve already seen. 
My version of [Proxy](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html) comes very close to being Decorator. As I wrote:
> [Having Proxy delegate to Feature] creates more composition possibilities, which will be featured in future design patterns.
>
> ...
>
> But we have a minor issue. In its traditional presentation, there’s only one Proxy class. What if the Concrete Class has multiple administrative concerns? The next pattern, Decorator, will address this.

Proxy delegates directly to the CoreFeature in the Gang of Four’s (GoF) Proxy design. I modified this slightly, by having the Proxy delegate to the Feature interface. It may not be obvious at first, but this design defines a linked list of Proxy objects with a CoreFeature object as the anchor at the end.

Let’s see how this can be useful if we want two Proxies. The only difference in this diagram and the one in the [Proxy Design Pattern](https://jhumelsine.github.io/2024/02/01/proxy-design-pattern.html) blog is that there are two Proxies rather than one. `ProxyB`’s structure mirrors `ProxyA`’s structure.

<img src="/assets/DecoratorProxies.png" alt="Decorator via Proxy" width = "75%" align="center" style="padding-right: 20px;">

The diagram only presents potential. The composability resides in the `Configurer`. Here’s an example of the linked list of objects that `Configure` could create from this design:

<img src="/assets/DecoratorProxyObjects.png" alt="Decorator Objects via Proxy" width = "50%" align="center" style="padding-right: 20px;">
 
The design supports each of these and more. The list could be a `client` followed by seven `proxyA` objects with the `coreFeature` at the end. The only constraint is that the list must end with `coreFeature`.

Because Proxy is so much like Decorator with this minor change, I view Proxy as a special case of Decorator. Proxy is Decorator with one Proxy instance.

However, I have an issue with this design. There’s code duplication with the delegate management in both the Proxy classes. Plus, two lines are crossing each other in the design. While crossed lines cannot always be avoided in a design, it does start the slide down the slippery slope of a messy design. I’ve found that a messy design tends to have a messy implementation as well. While the above is not very messy, I like to avoid messy design elements, such as crossing lines, if I can avoid them.

## GoF Decorator
The GoF’s Decorator design removes the delegate duplicate code as well as the crossing lines. Here’s their Decorator:
* The reference to `Feature` has been pulled up into a common abstract _`Decorator`_ class. _`Decorator`_ cannot be instantiated as an object on its own.
* _`Decorator`_ implements `execute()` by delegating to `feature.execute()`. The `feature` reference is `private` which encapsulates the implementation details.
* Each concrete _`Decorator`_ class extends abstract `Decorator` and overrides `execute()` by running its own implementation as well as calling `Decorator`’s `execute()`.

<img src="/assets/DecoratorGoF.png" alt="Decorator via GoF" width = "75%" align="center" style="padding-right: 20px;">

Here’s an example of the linked list of objects that can be created for this design:

<img src="/assets/DecoratorGoFObjects.png" alt="Decorator Objects via GoF" width = "50%" align="center" style="padding-right: 20px;">
 
This design supports any number of _`Decorator`_ objects instandiated from concrete classes.

The GoF design is better than my multiple Proxies design, but one thing still bugs me about this design. It depends upon the concrete class developer calling `super.execute()`. I’m not convinced that developers will always call the parent’s `super.execute()` method, and there are no warnings or errors if they do not.

## Decorator with Template Method
I want to remove the need for developers to read documentation and pray that they follow instructions. I’d rather nudge (well, really force) them in the right direction.

This final design incorporates the [Template Method Design Pattern](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) into the design:
* The only modifications are in the abstract `Decorator` and the concrete `Decorator` classes that extend it.
* The GoF Decorator design is based upon running specialized pre-code, then delegating to `feature.execute()` and then running specialized post-code in the concrete `Decorator` classes.
* This Template Method infused version moves those three steps into the abstract _`Decorator`_, which ensures that those three steps are always executed in that order. The abstract _`Decorator`_ does not contain specialized pre- or post- code, but it can call them abstractly via its protected abstract methods, which must be implemented in the concrete _`Decorator`_ classes.
* I declare _`Decorator`_’s `execute()` method as `final`, so that the concrete `Decorator` classes cannot override it and break the delegation chain, even if unintentionally.
* The concrete `Decorator` classes only need to define what needs to be executed before and after the delegation. If they do not have any pre- or post- behavior, then these methods, which must be defined, can be empty.

<img src="/assets/DecoratorTemplateMethod.png" alt="Decorator using Template Method" width = "75%" align="center" style="padding-right: 20px;">
 
# Use Case – Coffee Labels
So far this has been very abstract. Let’s get into a use case that’s more concrete.

The __Heads First Design Patterns__ authors demonstrate Decorator via a fictious coffee shop called: Starbuzz Coffee. Here’s the gist of their example:
* Starbuzz offers several types of coffee: Dark Roast, House Blend, Espresso, Decaf, etc.
* Each coffee can be enhanced with Milk, Cream, Sugar, Whipped Cream, Mocha, Soy, etc. 
* They want to ensure that each combination is properly and consistently priced.
* They can’t design a class for each possible combination, because the number of combinations explodes exponentially.
In short, they decide to use the Decorator Design Pattern:
    * In their example, _`Feature`_ will be _`Beverage`_ and its function returns the _`cost()`_ of the Beverage.
    * The four types of coffee will be Core Features. While the previous diagrams only showed one Core Feature with Decorator, the design handles any number of them. Each coffee type has its own cost.
    * Each of the coffee enhancers will be a Decorator, and each will have their own cost.
    * A coffee order will be a coffee type with as many Decorator enhancements added as desired by the customer. The final cost will be calculated by traversing the list of decorators and adding their costs.
  
The Head First Design Pattern authors provide much more detail, but one has to purchase the book or subscribe to a service to view their example.
Another alternative is to visit the [Decorator Pattern With Real World Example In Java](https://www.codiwan.com/decorator-design-pattern-real-world-example-java/), which for the most part replicates the book’s Java code.

In my example, I’m going to focus upon a different feature, the Drink Order Label.

In my version of Starbuzz Coffee, the baristas don’t hand write the drink orders on the cups. The drink orders are printed on labels and attached to the cups. Drink orders can be created in at least three ways:
* The barista can type it in on an order pad as the customer places a drink order.
* The customer can place a drink order via an in-store kiosk.
* The customer can place a drink order via the Starbuzz mobile app.

Once the drink order label is attached to the cup, a member of the staff can fill it.

Here’s my Decorator/Template-Method design:
* I have two basic Core Features with `Coffee` and `Tea`. 
* _`Flavor`_ is the abstract Decorator. It’s using the Template Method design for `getLabel()`. It retrieves the delegate `drinkOrder`’s label, adds a comma separator and then adds its own abstracted flavor label.
* Each concrete _`Flavor`_ only needs to implement its `getFlavorLabel()` method.
* The `LablePrinter` prints a label for a `DrinkOrder` that’s been injected into it via the `DrinkOrderBuilder`.
* `DrinkOrderBuilder` is a Configurer, but I haven’t used that term here on purpose. Nor am I calling it a `Factory`. It’s more than that since each `DrinkOrder` will be individually constructed based upon the customer’s customized order. Its implementation is a bit too much to show in the diagram, but I'll provide a code sample below. The barista’s order pad, the in-store kiosk, and the Starbuzz mobile app each would delegate to `DrinkOrderBuilder`, which has not been shown due to space constraints as well.

<img src="/assets/DecoratorDrinks.png" alt="Starbuzz Labels via Decorator" width = "85%" align="center" style="padding-right: 20px;">
 
`DrinkOrderBuilder` can construct any order that’s desired by the customer. Here are some possible examples of a labelPrinter object and the list of drink order objects that have been constructed:

<img src="/assets/DecoratorDrinksObjects.png" alt="Starbuzz Labels via Decorator Objects" width = "50%" align="center" style="padding-right: 20px;">
 
The design will print labels for all of these from the simple “Coffee” to the longest one presented with “Coffee, Sugar, Sugar, Milk.” Notice that the label is constructed on the return calls, so that the last object in the list is printed on the label first and the first one is printed last.

A request for double sugars can be satisfied with two sugar decorators. The design also supports crazy combinations, such as adding lemon to coffee. However, we could protect the customer from this in the `DrinkOrderBuilder` by preventing lemon as a choice when coffee is the core drink.

`DrinkOrderBuilder` might be something like the code below. It iterates a list of ingredients into instantiates and composes a linked list of `DrinkOrder` objects.
```java
DrinkOrder buildDrinkOrder(String ingredients) {
    DrinkOrder drinkOrder = null;
    for (String ingredient : ingredients.split()) { // I know this is not working Java. Only suggesting iteration.
        drinkOrder = acquire(ingredient, drinkOrder);
    }
    return drinkOrder;
}

DrinkOrder acquire(String ingredient, DrinkOrder drinkOrder) throws Exception {
    switch(ingredient) {
        case “Coffee”: return new Coffee();
        case “Tea”: return new Tea();
        case “Sugar”: return new Sugar(drinkOrder);
        case “Milk”: return new Milk(drinkOrder);
        case “Lemon”: return new Lemon(drinkOrder);
        case “PumpkinSpice”: return new PumpkinSpice(drinkOrder);
        default: throw new Exception(“Unknown ingredient=” + ingredient);
    }
}
```

Consider how flexible this design is. `Coffee` could easily be replaced with `DarkRoast`, `HouseBlend`, `Espresso`, and `Decaf`. `Tea` could be replaced with `EarlGrey`, `BlackTea`, `Chai`, and `Herbal`. New _`Flavor`_’s can easily be added, such as `Honey`, `SoyMilk`, and `Cream`. Some _`Flavor`_’s may be seasonal, so they can be easily added or removed as desired, such as `PumpkinSpice` and `Peppermint`.

__This is an extremely contrived example.__ It’s definitely over an engineered design for this specific problem. It converts a String of ingredients into a label that’s almost identical to the ingredients. I could have just as easily split the ingredients and inserted a comma between them.

But this design can be the foundation for more. My `DrinkOrder` only printed a label. But the interface could have more features such as: `getCost()`, which is what the Head First authors showed, and `getCalories()`. Decorator could be the foundation for an entire suite of Drink Order functionality.

A Decorator based design might be a good mechanism for automation. For example, Starbuzz R&D might design an automated barista that makes drinks based upon instructions programmed in each concrete class.

# Decorator Pros and Cons
There is no perfect design pattern. There are always tradeoffs and considerations. Here are a few for Decorator.

## Cons
Decorator won’t replace all inheritance. It can layer additional behaviors to methods in an existing interface, but it can’t extend a class with new methods. A pizza is still a pizza regardless of how many toppings you put on it.

Composable behavior may be more difficult to diagnose with Decorator. This is the first major design pattern where behavior has been distributed across several classes. While it should be simple to understand concrete classes individually in a Decorator design, the responsibility to orchestrate observable behavior resides in the `Configurer` in composing the objects from those concrete classes correctly.

Unit tests may be sufficient to confirm behavior for individual concrete classes, but they are insufficient for overall behavior confirmation. Integration tests may be needed to confirm that desired behavior emerges when the objects are composed. However, since the number of possible configurations is infinite, it’s impossible to create tests for all possible scenarios.

## Pros
Decorator’s composable behavior is a double-edged sword. It's also a pro in addition to being a con. Behavior is not statically locked into place as is the case with inheritance. When new behavior is desired, it may be a relatively simple matter of a new composition of objects instantiated from the existing classes in the design. New behavior can be configured at runtime as well.

Sometimes new behavior is beyond the scope of a new composition. New behavior may require new core feature or decorator classes. It should be obvious where a new concrete class resides in the design. And as soon as the class is defined, the IDE or the compiler will scream which methods need to be implemented. Existing concrete classes can be references as implementation examples for new concrete class implementations.

The classes can be unit tested with ease. The classes in my Starbuzz design generally only need one test to confirm the label content that they produce. Even the abstract _`Flavor`_ class can be unit tested with a dependency upon a [test double](https://martinfowler.com/bliki/TestDouble.html) without depending upon another concrete class in the design.

There may be many concrete classes in a Decorator design, but the concrete classes tend to be independent. They don’t depend upon or know about each other. Therefore, new concrete classes can be added without fear of breaking existing classes. It’s also easier to identify and remove deprecated classes when they are no longer needed without impacting the rest of the design.

# Summary
Decorator is an alternative to inheritance, but it is not a complete replacement. When desired behavior includes multiple option combinations, possibly chosen by the customer or even the user, then it might just be the right tool for the job.

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* [Wikipedia Decorator Design Pattern](https://en.wikipedia.org/wiki/Decorator_pattern)
* [Source Making Decorator Design Pattern](https://sourcemaking.com/design_patterns/decorator)
* [Refactoring Guru Decorator Design Pattern](https://refactoring.guru/design-patterns/decorator)
* [Project Management Institute Decorator Design Pattern](https://www.pmi.org/disciplined-agile/the-design-patterns-repository/the-decorator-pattern)
* [Dofactory C# Decorator Design Pattern](https://www.dofactory.com/net/decorator-design-pattern)
* [Dofactory JavaScript Decorator Design Pattern](https://www.dofactory.com/javascript/design-patterns/decorator)
* [Baeldung Decorator Design Pattern](https://www.baeldung.com/java-decorator-pattern)
* [The Art of Decorating: Applying the Decorator Design Pattern in Real Life](https://www.pentalog.com/blog/design-patterns/decorator-design-pattern/)
* [OODesign Decorator Design Pattern](https://www.oodesign.com/decorator-pattern)
* and for more, Google: [Decorator Design Pattern](https://www.google.com/search?q=Decorator+Design+Pattern)

Here are some resources that can be purchased or are included in a subscription service:
* Gang of Four Proxy Design Pattern, page 175 ([O'Reilly](https://learning.oreilly.com/library/view/design-patterns-elements/0201633612/ch04.html#page_175) and [Amazon](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612))
* Agile Principles, Patterns, and Practices in C#, Chapter 35 ([O'Reilly](https://learning.oreilly.com/library/view/agile-principles-patterns/0131857258/ch35.xhtml) and [Amazon](https://www.amazon.com/Agile-Principles-Patterns-Practices-C/dp/0131857258))
* Clean Code: Design Patterns, Episode 32 video ([Clean Coders](https://cleancoders.com/episode/clean-code-episode-32) and [O'Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_03_32_00/))
* Head First Design Patterns, Chapter 3 ([O'Reilly](https://learning.oreilly.com/library/view/head-first-design/9781492077992/ch03.html) and [Amazon](https://www.amazon.com/Head-First-Design-Patterns-Object-Oriented-ebook/dp/B08P3X99QP))
