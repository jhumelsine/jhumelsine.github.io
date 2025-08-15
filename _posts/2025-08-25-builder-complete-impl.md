---
title: DRAFT – Builder Design Pattern Continued
description: Progressing to the Gang of Four’s complete Builder design
unlisted: true
---
__Add new diagram here.__ 

# Introduction
__TBD__

This blog entry is a continuation of the previous blog entry [Builder Design Pattern - Basic Implementation]( https://jhumelsine.github.io/2025/08/13/builder-basic-impl.html). In that blog, I showed the progression of creating a complex object from a constructor to a basic version of the Builder Design Pattern.

The basic Builder in the previous blog did not feature several elements of the Gang of Four’s (GoF) Builder example. Specifically, it did not have __Director__ nor did it feature constructing the complex object defined in a specification.

This blog will continue where the previous blog left off by progressing step-by-step until it reaches design more akin to the GoF’s diagram:

<img src="/assets/Builder1-1.png" alt="RTF Builder by GoF" width = "90%" align="center" style="padding-right: 35px;">
 

These step-by-step transitions will pull elements of the design apart along their seams. There will be a little messiness as I progress. It’s like reorganizing a room. You need to pull everything apart before you can organize the elements to put it back together.

This blog entry will focus upon the design elements and implementation snippets in transition toward the GoF’s Builder design.

# Addressing Tight Coupling
The previous blog concluded with the [Pizza.Builder]( https://jhumelsine.github.io/2025/08/13/builder-basic-impl.html#the-pizzabuilder) design, which is indeed a Builder. But `Pizza.Builder` was declared as an inner class of `Pizza`, which tightly coupled the two classes. For some designs this may make perfect sense. However, this tight coupling won’t support the remaining GoF Builder behaviors that I want to convey.

Here is an intermediate redesign moving toward the GoF’s Builder. This would not be my final design. I must make some _accommodations_ in this design that I don’t particularly like, but they will make it easier to transition to the full GoF Builder design. I am mostly including this design since it is a transient transitional phase in moving toward the final design.

Here is the updated UML diagram with separate `PizzaBuilder` and `Pizza` class definitions. `Pizza` no longer has knowledge of nor depends upon `PizzaBuilder`. I also added a `Client` class to show how it uses `PizzaBuilder` to build a `Pizza` instance. The `Client` is the [Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer) in this design and it will be the Configure throughout these design phases.

__Add new diagram here.__ 
 

Here is the code:
```java
import java.util.*;

public class PizzaBuilder1 {
    public static void main(String[] args) {
        Pizza pizza1 = new PizzaBuilder(PizzaSize.LARGE)
            .addPepperoni()
            .addOnions()
            .build();
        System.out.println(pizza1);

        Pizza pizza2 = new PizzaBuilder(PizzaSize.MEDIUM)
            .addPepperoni()
            .addPeppers()
            .addOnions()
            .addBlackOlives()
            .build();
        System.out.println(pizza2);

        Pizza pizza3 = new PizzaBuilder(PizzaSize.SMALL).build();
        System.out.println(pizza3);
    }

}

enum PizzaSize {
    SMALL, MEDIUM, LARGE
}

public class PizzaBuilder {
    private PizzaSize size;
    private boolean pepperoni = false;
    private boolean peppers = false;
    private boolean onions = false;
    private boolean blackOlives = false;

    public PizzaBuilder(PizzaSize size) {
        this.size = size;
    }

    public PizzaBuilder addPepperoni() {
        pepperoni = true;
        return this;
    }

    public PizzaBuilder addPeppers() {
        peppers = true;
        return this;
    }

    public PizzaBuilder addOnions() {
        onions = true;
        return this;
    }

    public PizzaBuilder addBlackOlives() {
        blackOlives = true;
        return this;
    }

    public Pizza build() {
        return new Pizza(size, pepperoni, peppers, onions, blackOlives);
    }
}

class Pizza {
    private final PizzaSize size;
    private final boolean pepperoni;
    private final boolean peppers;
    private final boolean onions;
    private final boolean blackOlives;

    Pizza(PizzaSize size, boolean pepperoni, boolean peppers, boolean onions, boolean blackOlives) {
        this.size = size;
        this.pepperoni = pepperoni;
        this.peppers = peppers;
        this.onions = onions;
        this.blackOlives = blackOlives;
    }

    @Override
    public String toString() {
        StringBuilder description = new StringBuilder();
        description.append("Pizza Size: ").append(size);

        description.append(" with toppings:");
        boolean hasToppings = false;

        if (pepperoni) {
            description.append(" Pepperoni");
            hasToppings = true;
        }
        if (peppers) {
            if (hasToppings) description.append(",");
            description.append(" Peppers");
            hasToppings = true;
        }
        if (onions) {
            if (hasToppings) description.append(",");
            description.append(" Onions");
            hasToppings = true;
        }
        if (blackOlives) {
            if (hasToppings) description.append(",");
            description.append(" Black Olives");
            hasToppings = true;
        }

        if (!hasToppings) {
            description.append(" None");
        }

        return description.toString();
    }
}
```

# Extracting a PizzaBuilder Interface
The GoF’s Builder example declares a __Builder__ interface with several concrete __Builder__ classes. In this phase, I’m going to convert `PizzaBuilder` into an interface and move its implementation to `StandardPizzaBuilder`. Most of the rest of the design remains the same.

`Client` must still access the concrete builder, now named `StandardPizzaBuilder`. That’s because `build()` is not declared in the `PizzaBuilder` interface. The interface only defines methods that define pizza toppings. `PizzaSize` is still declared within the `StandardPizzaBuilder’s` constructor. I feel that should belong in `PizzaBuilder` as well, which I’ll introduce in the subsequent transition phase.

Each concrete class that implements `PizzaBuilder` can build its own type of product, which will become more obvious when I add a second concrete class. Concrete `PizzaBuilder` classes are not obligated to create a `Pizza` as their product.

Since the `Client` wants to create a `Pizza`, it will still reference the `StandardPizzaBuilder` class so that it has access to its `Pizza build()` method.

__Add new diagram here.__ 
 
Here are the updated Java snippets. The entire implementation for each design phase is provided at [Complete Demo Code](#CompleteDemoCode).
```java
interface PizzaBuilder {
    void addPepperoni();

    void addPeppers();

    void addOnions();

    void addBlackOlives();
}

public class StandardPizzaBuilder implements PizzaBuilder {
    private PizzaSize size;
    private boolean pepperoni = false;
    private boolean peppers = false;
    private boolean onions = false;
    private boolean blackOlives = false;

    public StandardPizzaBuilder(PizzaSize size) {
        this.size = size;
    }

    @Override
    public void addPepperoni() {
        pepperoni = true;
    }

    @Override
    public void addPeppers() {
        peppers = true;
    }

    @Override
    public void addOnions() {
        onions = true;
    }

    @Override
    public void addBlackOlives() {
        blackOlives = true;
    }

    public Pizza build() {
        return new Pizza(size, pepperoni, peppers, onions, blackOlives);
    }
}
```

# Adding the Director
Every __Builder__ example I have provided thus far has featured a hardcoded construction. If the `Client` wishes to build a different `Pizza`, then that will require a code change. I want more flexibility in the design. I want a customer to be able to customize their Pizza. I want them to be able to put any combinations of toppings on it without having to update the implementation.

The GoF’s Builder design includes a __Director__ that will accommodate customized complex object construction. The __Director__ implements a method that the GoF called _parseRTF()_ in their domain specific example and _Construct()_ in their more generalized example. This method parses a specification, identifies construction details and delegates them to an `Builder` interface, whose reference has been provided as a method parameter. That is, `Construct()` does not know nor depend upon a specific concrete `Builder`. It only depends upon the `Builder` interface contract.

This first diagram features `PizzaDirector`. Its `construct()` method declares two parameters:
* `List<String> specification` which represents a specification that consists of multiple lines of strings. The specification origin may have been a text file or the selections on an app by the customer.
* `PizzaBuilder` which is the interface to which the `PizzaDirector` will direct its construction.

`setSize(PizzaSize size)` has also been added to `PizzaBuilder` thereby placing it in the `PizzaBuilder` interface contract, which makes more sense, at least to me.

`construct(specification, pizzaBuilder)` iterates each specification line and based upon its value, delegates to the appropriate `PizzaBuilder` method. `construct(specification, pizzaBuilder)` only constructs the `PizzaBuilder` by telling it the pizza size and toppings. It does not build the pizza itself.
__Add new diagram here.__ 
 
The previous diagram is only part of the design. `construct(…)` required too much space to fit the entire design on one diagram.

Here’s the complete design where the `construct()` details have been removed. A few items of note:
* `Client` no longer hardcodes the pizza. The pizza order resides within the specification. The other parts of `Client` are mostly the same as with the previous design.
* Except for moving `setSize(PizzaSize`, `PizzaBuilder` and `StandardPizzaBuilder` have not changed.
* `Pizza` has never changed. Since it has no dependencies upon the rest of the design.

__Add new diagram here.__ 
 
Here is the code for the updated elements in this design:
```java
StandardPizzaBuilder pizzaBuilder1 = new StandardPizzaBuilder();
PizzaDirector.construct(getLargePizzaSpecification(), pizzaBuilder1);
Pizza pizza1 = pizzaBuilder1.build();
System.out.println(pizza1);

private static List<String> getLargePizzaSpecification() {
    List<String> specification = new LinkedList<>();

    specification.add("Large");
    specification.add("pepperoni");
    specification.add("onions");

    return specification;
}

class PizzaDirector {

    public static void construct(List<String> specification, PizzaBuilder pizzaBuilder) throws Exception {
        for (String spec : specification) {
            switch (spec.toUpperCase()) {
                case "LARGE": pizzaBuilder.setSize(PizzaSize.LARGE); break;
                case "MEDIUM": pizzaBuilder.setSize(PizzaSize.MEDIUM); break;
                case "SMALL": pizzaBuilder.setSize(PizzaSize.SMALL); break;
                case "PEPPERONI": pizzaBuilder.addPepperoni(); break;
                case "PEPPERS": pizzaBuilder.addPeppers(); break;
                case "ONIONS": pizzaBuilder.addOnions(); break;
                case "BLACK OLIVES": pizzaBuilder.addBlackOlives(); break;
                default: throw new Exception("Unknown spec=" + spec);
            }
        }
    }
}

public class StandardPizzaBuilder implements PizzaBuilder {
    private PizzaSize size = null;
    private boolean pepperoni = false;
    private boolean peppers = false;
    private boolean onions = false;
    private boolean blackOlives = false;

    public void setSize(PizzaSize size) {
        this.size = size;
    }

    public void addPepperoni() {
        pepperoni = true;
    }

    public void addPeppers() {
        peppers = true;
    }

    public void addOnions() {
        onions = true;
    }

    public void addBlackOlives() {
        blackOlives = true;
    }

    public Pizza build() {
        return new Pizza(size, pepperoni, peppers, onions, blackOlives);
    }
}
```

# A New Concrete PizzaBuilder
The GoF also featured several concrete Builders. In this final design I’ll add `CaloriePizzaBuilder`, which is a `PizzaBuilder`. This design shows how simple it is to add a new concrete `PizzaBuilder` once the design infrastructure has matured.
`CaloriePizzaBuilder’s` product is the number of calories in the pizza, which is returned as an `int`.

Here’s the design that supports `CaloriePizzaBuilder`. Here are a few items of note:
* `CaloriePizzaBuiler` has been added.
* The `Client` has been updated, but barely. It creates a `CaloriePizzaBuilder` rather than a `StandardPizzaBuilder`. `calories` is a basic type rather than an object.
* Nothing else in the design changes.

__Add new diagram here.__ 
 
Here are snippets of the updated code. The `PizzaSize` will act as a scalar to increase the number of calories reflecting the fact that larger pizzas will have more calories.
```java
StandardPizzaBuilder pizzaBuilder1 = new StandardPizzaBuilder();
PizzaDirector.construct(getLargePizzaSpecification(), pizzaBuilder1);
Pizza pizza1 = pizzaBuilder1.build();
System.out.println(pizza1);

public class CaloriePizzaBuilder implements PizzaBuilder {
    private PizzaSize size = null;
    private int calories = 0;

    public void setSize(PizzaSize size) {
        this.size = size;
        calories += getSizeRatio(size) * 1000.0;
    }

    private double getSizeRatio(PizzaSize size) {
        switch (size) {
            case SMALL: return 1.0;
            case MEDIUM: return 1.5;
            case LARGE: return 2.0;
            default: return 0.0;
        }
    }

    public void addPepperoni() {
        calories += getSizeRatio(size) * 250;
    }

    public void addPeppers() {
        calories += getSizeRatio(size) * 30;
    }

    public void addOnions() {
        calories += getSizeRatio(size) * 25;
    }

    public void addBlackOlives() {
        calories += getSizeRatio(size) * 100;
    }

    public int getCalories() {
        return calories;
    }
}
```

# The Complete Design
This final diagram shows all elements in the design. Its structure mirrors the GoF’s example at the top of this blog and also available [here](https://jhumelsine.github.io/2025/08/08/builder-introduction.html#why-use-it).
Once the entire design assembled, it becomes obvious that the `Strategy Design Pattern](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) is a major design element in this design. For example, this design can easily accommodate more concrete `PizzaBuilders` such as `PricePizzaBuilder`, for which its implementation would look very similar to `CaloriePizzaBuilder`, but rather than calculating calories, it would calculate the cost of the built pizza.
 
# But Wait, There’s More
There’s one more aspect of Builder that’s useful. I think it’s the most important aspect of Builder, but the GoF don’t really mention it. I’ll cover that in the next and final Builder blog entry.

# Summary
__TBD__

# References
See: [Previous Blog References](https://jhumelsine.github.io/2025/08/08/builder-introduction.html#references)

# Complete Demo Code
Here’s the entire implementation up to this point as one file. Copy and paste it into a Java environment and execute it. If you don’t have Java, try this [Online Java Environment](https://www.programiz.com/java-programming/online-compiler/). Add more tests. Play with the implementation. Refactor some of the code.

## Addressing Tight Coupling
```java
```

## Extracting a PizzaBuilder Interface
```java
```

## Adding the Director
```java
```

## A New Concrete PizzaBuilder
```java
```



