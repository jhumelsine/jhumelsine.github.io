---
title: DRAFT – Builder Design Pattern and Composable Patterns
description: Should Builder’s Director have been its own design pattern?
unlisted: true
---

# Introduction
__TBD__

# Multiple Objects
The previous Builder post entries demonstrated how the design can be used to build complex objects, even dynamically via a specification. But rather than building one complex object via a specification, it can be used to build a set of simple objects assembled in different combinations from which different behaviors emerge.

Previous blogs have covered design patterns that support this flexibility with the [Composable Design Pattern](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html), but I didn’t go into great detail about how to configure these multiple object designs.

I’ll wrap up the Builder design pattern with this blog by describing how it can be used to assemble objects that are part of a Composable design.

Don’t look for this use case in the Builder section of Gang of Four (GoF) book. They didn’t describe it in detail. They omitted one of Builders most powerful use cases. Their Builder builds a single complex object; whereas, this use of Builder returns the access object to the data structure that’s used with a Composable design, such as the head of a linked list or the root of a tree.

While the GoF didn’t cover this use case, other Builder references have mentioned the omitted use case, even if they didn’t provide much detail.

## [Source Making Builder](https://sourcemaking.com/design_patterns/builder)
>Builder often builds a [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html).

## [Refactoring Guru Builder](https://refactoring.guru/design-patterns/builder)
>Use the Builder to construct [Composite](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) trees or other complex objects.
>
>The Builder … comes in handy when you need to build an object tree.

# The Director
The GoF’s Builder description features two elements: __Builder__ and __Director__.

They focused their description upon the __Builder__. They acknowledge the __Director__, but they don’t treat it like a first-class citizen. Their __Director__ feels more like an supporting character in __Builder’s__ story rather than a character deserving of its own story. The more I think about the __Builder__ pattern as I’m working on this blog entry, the more I’m thinking that __Director__ should have probably deserved its own place in the GoF catalog as a design pattern.

The GoF included a __Director__ in their __Builder__ diagrams. They include a few high-level code snippet examples. And they also hinted at some of its additional uses at the end of __Builder__ in the __Known Uses__ section. __NOTE:__ I believe these first two references are associated with constructs in Smalltalk-80, for which they state, “Builder is a common pattern in Smalltalk-80”:
>The `Parser` class in the compiler subsystem is a __Director__ that takes a `ProgramNodeBuilder` object as an argument. A `Parser` object notifies its `ProgramNodeBuilder` object each time it recognizes a syntactic construct. When the parser is done, it asks the builder for the parse tree it built and returns it to the client.
>
>`ClassBuilder` is a builder that Classes use to create subclasses for themselves. In this case a Class is both the Director and the Product.

The next one is not associated with Smalltalk-80:
>The Service Configurator framework from the Adaptive Communications Environment uses a __Builder__ to construct network service components that are linked into a server at run-time. The components are described with a configuration language that’s parsed by an LALR(l) parser. The semantic actions of the parser perform operations on the builder that add information to the service component. In this case, the parser is the __Director__.

And their __Builder__ section ends with this final sentence in the closing __Related Patterns__ section:
>A [Composite]( https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) is what the builder often builds.

Technically, they do reference this use case, but they waited until their closing words before mentioning it, and even then, it’s mostly in passing without many details.

# The Configurer
While I didn’t go into great details describing how Composable design patterns are configured, I always mentioned [The Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer), which is more than the GoF mentioned in their descriptions.

Sometimes my Configurer design references were simply purple classes named _Configurer_, without any implementation details indicating how they orchestrated the objects in the composition. Sometimes my Configurer references contained an implementation, which was hardcoded. Sometimes I even included _Builder_ in the Configurer’s name. Maybe I should have used _Director_ instead.

I did not feature a specification-driven Configurer. I didn’t even feature a specification-driven Configurer for the [Specification Design Pattern](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html) or its [Smart Playlist Use Case](https://jhumelsine.github.io/2024/03/07/specification-design-pattern-use-case.html), both of which featured hardcoded Configurers.

# The Interpreter
I did not completely ignore specification-driven Configurers in the Composable Design Patterns. I featured them at great length with the [Interpreter Design Pattern - Introduction](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html). As stated by GoF above, Parsers are types of Builders and Directors.

Interpreter was its own miniseries where I presented Domain-Specific Languages (DSL), Grammar, and Parser Theory and Implementation. I will not repeat those details, since they can be found via the Interpreter link above.

A Configurer can be implemented via a Parser is a type of Builder/Director.

# The Decorator Use Case Example
__Don’t Panic!__

I will not present an example as complex as what I provided in the Interpreter examples. While all DSLs are defined by a grammar, not all grammars define a DSL. My grammar and parser in this example will be about as simple as possible.

I will present this final type of Builder by expanding upon the [Decorator Design Pattern Coffee Label Use Case](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html#use-case-coffee-labels). Here’s is a copy of the design from the Decorator blog:

<img src="/assets/DecoratorDrinks.png" alt="Starbuzz Labels via Decorator" width = "85%" align="center" style="padding-right: 20px;">
 
I have updated it slightly as. In this version, `DrinkOrder’s` behavior is more functional in that it calculates calories and cost. I also changed a few class names. I also expanded upon `DrinkOrderBuilder` from the previous diagram by providing more a bit more detail with the __Factories__.

<img src="/assets/Builder-4-1.png" alt="Director Design 1" width = "90%" align="center" style="padding: 35px;">
 
I started [TDD](https://jhumelsine.github.io/2024/07/15/tdd.html) as I was sketching out the implementation. I focused upon `Coffee`, `Tea`, `Sugar` and `Milk`. The tests were straightforward, and each class implementation was simple. But even so, I noticed the same code being copy-and-pasted in each concrete class. It was basically the same code with different values for calories and cost.

I started to refactor the code. I moved calorie and cost attributes into the `Drink` and `AddOn` abstract base classes and initialized them from the concrete class constructors. And still there was duplication.

It finally sunk through my thick skull that the `Drink` and `AddOn` abstract base classes didn’t need to be abstract. They can be concrete classes that declare immutable value objects with final attributes for `name`, `calories` and `cost`. The extraneous concrete classes disappeared from the design.

Sometimes you discover that your design isn’t quite right once you start test and implementation just as I did. Go ahead and change your design as you understand it better.

Here is the updated design with my new understanding.

<img src="/assets/Builder-4-2.png" alt="Director Design 2" width = "90%" align="center" style="padding: 35px;">
 
The `DrinkOrder` implementation matches the design snippet almost exactly:
```java
interface DrinkOrder {
    int getCost();

    int getCalories();
}
```

`Drink` doesn’t have any code snippets in the design, but it’s simply an immutable value object. I have tests for each class, but I won’t include them in these examples. They can be found in the [Complete Demo Code](#complete-demo-code).
```java
class Drink implements DrinkOrder {
    private final String name;
    private final int cost;
    private final int calories;

    public Drink(String name, int cost, int calories) {
        this.name = name;
        this.cost = cost;
        this.calories = calories;
    }

    @Override
    public final int getCost() {
        return cost;
    }

    @Override
    public final int getCalories() {
        return calories;
    }

    @Override
    public String toString () {
        return name;
    }
}
```

`AddOn` is a value object as well. In addition to `cost`, `calories` and `name`, it also has a reference to `drinkOrder`. `Drink` and `AddOn` still have a little duplication in their attributes, but I decided to allow it. While I could have consolidated `cost`, `calories` and `name` into one class, the additional complexity didn’t feel like it justified the consolidation.

`AddOn` is where the core __Decorator__ implementation resides as it calculates cost, calories and assembles `toString()` by delegating to `drinkOrder`, which is the next element in the linked list.

```java
 class AddOn implements DrinkOrder {
    private final int cost;
    private final int calories;
    private final String name;
    private final DrinkOrder drinkOrder;

    public AddOn(String name, int cost, int calories, DrinkOrder drinkOrder) throws NullPointerException {
        if (drinkOrder == null) {
            throw new NullPointerException("drinkOrder");
        }
        this.name = name;
        this.cost = cost;
        this.calories = calories;
        this.drinkOrder = drinkOrder;
    }

    @Override
    public final int getCost() {
        return cost + drinkOrder.getCost();
    }

    @Override
    public final int getCalories() {
        return calories + drinkOrder.getCalories();
    }

    @Override
    public final String toString() {
        return name + " " + drinkOrder.toString();
    }

}
```

The code snippets in the design for the __Factory__ hinted at their implementation with one example for each. Here are the implementations for both __Factories__. All calorie and cost values are contrived:
```java
class DrinkFactory {
    public static DrinkOrder acquire(String drink) throws IllegalArgumentException {
        drink = drink.trim();
        switch (drink) {
            case "Coffee": return new Drink(drink, 350, 5);
            case "Tea": return new Drink(drink, 250, 15);
            default: throw new IllegalArgumentException("Cannot create Drink for:" + drink);
        }
    }
}

class AddOnFactory {
    public static DrinkOrder acquire(String addOn, DrinkOrder drinkOrder) throws IllegalArgumentException {
        addOn = addOn.trim();
        switch (addOn) {
            case "Sugar": return new AddOn(addOn, 15, 200, drinkOrder);
            case "Milk": return new AddOn(addOn, 35, 100, drinkOrder);
            case "SoyMilk": return new AddOn(addOn, 35, 75, drinkOrder);
            case "EspressoShot": return new AddOn(addOn, 100, 400, drinkOrder);
            case "Lemon": return new AddOn(addOn, 15, 15, drinkOrder); 
            case "PumpkinSpice": return new AddOn(addOn, 75, 200, drinkOrder);
            default: throw new IllegalArgumentException("Cannot create AddOn for:" + addOn);
        }
    }
}
```

I’ll wrap most of the rest of the design with `DrinkOrderBuilder`. The grammar for the order is basically:
```
(AddOnID “:”)* DrinkID
```

It’s simply any number of colon separated AddOn identifiers terminating with a single Drink identifier. It’s so simple that it’s really a regular expression, which is still a context-free grammar.

I used recursion to implement the Builder/Director:
* If the colon delimiter is not found, then the order is a Drink identifier. It will acquire a and return `DrinkOrder` from the `DrinkFactory` based upon the Drink identifier.
* Otherwise, it will split the order into two substrings: the first AddOn Identifier and the rest of the order. It will acquired the `DrinkOrder` recursively from the rest of the order, which will be an argument for the `AddOnFactory` as the `drinkOrder` for the newly acquired AddOn, which will be returned.

The code is a bit less cumbersome than my description above:
```java
class DrinkOrderBuilder {
    public static DrinkOrder acquire(String order) {
        int delimiterIndex = order.indexOf(":");
        if (delimiterIndex < 0) return DrinkFactory.acquire(order.trim());
        return AddOnFactory.acquire(order.substring(0, delimiterIndex).trim(), acquire(order.substring(delimiterIndex+1).trim()));
    }
}
```

The final, and least interesting class, is `BaristaBot`. It acquires a `DrinkOrder` from `DrinkOrderBuilder` and prints the order’s label and calories.
```java
class BaristaBot {
    public void prepareOrder(String order) {
        DrinkOrder drinkOrder = DrinkOrderBuilder.acquire(order);

        System.out.format("Order Label:%s\n", drinkOrder.toString());
        int cost = drinkOrder.getCost();
        System.out.format("Price: $%d.%d\n", cost/100, cost % 100);
        System.out.format("Calories: %d\n", drinkOrder.getCalories());
    }
}
```

`main(…)` drives everything:
```java
    public static void main(String[] args) throws Exception {
        BaristaBot barista = new BaristaBot();

        System.out.println();
        barista.prepareOrder("SoyMilk:Sugar:Sugar:EspressoShot:PumpkinSpice:Coffee");
        
        System.out.println();
        barista.prepareOrder("Sugar:Lemon:Tea");
    }
```

And now we see how it all comes together. A text order is passed to the barista, which acquires a `DrinkOrder` linked list that’s composed of the objects that match the order. From there, the barista can obtain the label, cost and calories for the order using __Decorator__.

This implementation has similar behaviors to the __PizzaBuilder__ example in the previous blogs, where label, calories and eventually cost were calculated via different concrete Builders; whereas, this implementation uses one __Decorator__.

This demonstrates that different design patterns can solve similar problems. It’s a matter of design preference. This won’t be the last technique used to solve problems like this, but that’s the topic for a future blog TBD.

# Factory Revisit
I'm wrapping up with a bit of a side tangent, that's not specifically about __Builder__ or the __Director__. It's about the __Factories__.

The `DrinkFactory` and `AddOnFactory` are sufficient for now, but they aren't dynamic. If a new drink or add on is desired, then the code has to be updated. I revisited the implementation and updated it. Rather than a being a `switch` statement, the factories look up `DrinkConfiguration` information from a `Map`. `DrinkConfiguration` contains name, cost and calorie information. The `Map` is initialized via a `static` method in my example, but it could easily be initialized via a configuration file, so that it could be updated as needed.

There is the Java code for the updated Factories:
```java
class DrinkOrderConfiguration {
    private final String name;
    private final int cost;
    private final int calories;

    public DrinkOrderConfiguration(String name, int cost, int calories) {
        this.name = name;
        this.cost = cost;
        this.calories = calories;
    }

    public String getName() {
        return name;
    }

    public int getCost() {
        return cost;
    }

    public int getCalories() {
        return calories;
    }
}

class DrinkFactory {
    private static Map<String, DrinkOrderConfiguration> drinkOrderConfigurations = new HashMap<>();

    static {
        for (String drinkConfiguration : getDrinkOrderConfigurations()) {
            String[] tokens = drinkConfiguration.split(",");
            drinkOrderConfigurations.put(tokens[0].trim(), new DrinkOrderConfiguration(tokens[0].trim(), Integer.valueOf(tokens[1].trim()), Integer.valueOf(tokens[2].trim())));
        }
    }

    // Imagine these values were acquired fron an external source, such as a flat file.
    private static List<String> getDrinkOrderConfigurations() {
        List<String> drinkOrderConfigurations = new LinkedList<>();
        drinkOrderConfigurations.add("Coffee, 350, 5");
        drinkOrderConfigurations.add("Tea, 250, 15");
        return drinkOrderConfigurations;
    }

    public static DrinkOrder acquire(String drink) throws IllegalArgumentException {
        drink = drink.trim();
        if (!drinkOrderConfigurations.containsKey(drink)) throw new IllegalArgumentException("Cannot create Drink for:" + drink);
        DrinkOrderConfiguration drinkConfiguration = drinkOrderConfigurations.get(drink);
        return new Drink(drinkConfiguration.getName(), drinkConfiguration.getCost(), drinkConfiguration.getCalories());
    }
}

class AddOnFactory {
    private static Map<String, DrinkOrderConfiguration> drinkOrderConfigurations = new HashMap<>();

    static {
        for (String addOnConfiguration : getDrinkOrderConfigurations()) {
            String[] tokens = addOnConfiguration.split(",");
            drinkOrderConfigurations.put(tokens[0].trim(), new DrinkOrderConfiguration(tokens[0].trim(), Integer.valueOf(tokens[1].trim()), Integer.valueOf(tokens[2].trim())));
        }
    }

    // Imagine these values were acquired fron an external source, such as a flat file.
    private static List<String> getDrinkOrderConfigurations() {
        List<String> drinkOrderConfigurations = new LinkedList<>();
        drinkOrderConfigurations.add("Sugar, 15, 200");
        drinkOrderConfigurations.add("Milk, 35, 100");
        drinkOrderConfigurations.add("SoyMilk, 35, 75");
        drinkOrderConfigurations.add("EspressoShot, 100, 400");
        drinkOrderConfigurations.add("Lemon, 15, 15");
        drinkOrderConfigurations.add("PumpkinSpice, 75, 200");
        return drinkOrderConfigurations;
    }

    public static DrinkOrder acquire(String addOn, DrinkOrder drinkOrder) throws IllegalArgumentException {
        addOn = addOn.trim();
        if (!drinkOrderConfigurations.containsKey(addOn)) throw new IllegalArgumentException("Cannot create AddOn for:" + addOn);
        DrinkOrderConfiguration drinkOrderConfiguration = drinkOrderConfigurations.get(addOn);
        return new AddOn(drinkOrderConfiguration.getName(), drinkOrderConfiguration.getCost(), drinkOrderConfiguration.getCalories(), drinkOrder);
    }
}
```

# Summary
__TBD__

# References
See: [Previous Blog References](https://jhumelsine.github.io/2025/08/08/builder-introduction.html#references), which provides an extensive list of __Builder__ resources.

# Complete Demo Code
Here’s the entire implementation up to this point as one file. Copy and paste it into a Java environment and execute it. If you don’t have Java, try this [Online Java Environment](https://www.programiz.com/java-programming/online-compiler/). Add more tests. Play with the implementation. Refactor some of the code.
