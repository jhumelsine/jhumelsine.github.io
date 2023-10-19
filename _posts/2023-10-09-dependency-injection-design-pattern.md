---
title: Dependency Injection
description: How Dependency Injection resolves object resolution cleanly
---

<img src="https://ae01.alicdn.com/kf/S7fdd40fce12042769a255dce921cb97dg/Stainless-Steel-Spice-Syringe-Marinade-Injector-Flavor-Syringe-Cooking-Meat-Poultry-Turkey-Chicken-Kitchen-BBQ-Tool.jpg" alt="Meat Injector" width = "55%" align="center" style="padding-right: 20px;">
 
# Introduction
I’m finally done kicking the object resolution can down the road. This wraps up the [Essential Design Patterns](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html).

About a dozen years ago, I worked on a tightly knit small team. I wrote some of the best code of my career while on that team. It just wasn’t the team that was tight. Our code was tightly coupled as well. It was modular, but it was still tightly coupled.

Often when we needed to test new behavior, we’d compile the entire product on our desktop machines, copy the executable to a thumb drive, walk it into our QA lab, copy the executable from the thumb drive to a server and then try to observe the new or updated functionality through the user interface.

If the behavior was broad, then we could probably observe it. If the behavior was more subtle, then we probably couldn’t observe it. Edge cases were very difficult to confirm. We'd ship our code and hope for the best. I don't recall any major issues with what we wrote.

We had [CppUnit](https://en.wikipedia.org/wiki/CppUnit), which is the C++ version of [xUnit](https://en.wikipedia.org/wiki/XUnit). We could write tests in CppUnit, but we didn’t really understand how to structure tests well. I tried writing CppUnit tests several times, but after a few weeks, they would start breaking, because they were brittle. It was several years before I figured out unit testing.

Some of my code was decoupled enough that I could test it on my desktop. I could test the sunny day scenarios, but I was having a real problem testing the rainy-day scenarios. For example, I wanted to confirm that my code handled an Exception correctly, but I couldn’t make the code throw an Exception easily.

I had been using Design Patterns for about 7 years by this point. What was I missing? Here’s an extremely high-level sketch of my basic design. Keep in mind that I want to confirm the expected behavior in `ClientApplication` when it calls `myInterface.doThis()` and an Exception is thrown by `MyClass`. But I couldn't figure out a way to force `MyClass` to throw the Exception.

<img src="/assets/DependencyInjectionSetUp.png" alt="Dependency Injection Set Up" width = "75%" align="center" style="padding-right: 20px;">
 
Why am I having this problem? I’m following good Design Pattern Principles:
* I’m [programming to an interface, not an implementation](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html#program-to-an-implementation-not-an-interface).
* I’m encapsulating the implementation, `MyClass`, from `ClientApplication` by acquiring it via a [Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html).

I figured out a way to force an Exception, but it was nasty:
* I created a new class, `MyTestDouble`, which threw the Exception when `doThis()` was called.
* I would change the implementation in `MyInterfaceFactory` to return an instance of `MyTestDouble`.
* I would confirm the behavior via a manual test.
* I would change `MyInterfaceFactory` back to its original implementation to return `MyClass`.

<img src="/assets/DependencyInjectionForcedException.png" alt="Dependency Injection Forced Exception" width = "75%" align="center" style="padding-right: 20px;">
 
This worked, but it was painful. A test took a long time to set up and it required a change to the Factory. What if I ever forgot to revert the Factory back to its proper implementation?

# Dependency Inversion Principle
What was I doing wrong?

I was violating the [Dependency Inversion Principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle) (DIP), even though I had not heard of it before.

When code instantiates an object directly via `new()`, it is tightly coupled to that class. If the coupled class does the same, then it's tightly coupled to its dependencies. This tight coupling can daisy chain through the system to the point that the entire system is tightly coupled from top to bottom. You can see how the arrows show dependency from left to right:

<img src="https://upload.wikimedia.org/wikipedia/commons/4/42/Traditional_Layers_Pattern.png" alt="Tight Coupling" width = "65%" align="center" style="padding-right: 20px;">

[Programming to an interface, not an implementation](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html#program-to-an-interface-not-an-implementation) helps break that dependency.
Placing an interface between classes acts as a buffer that inverts the dependency. Notice that the interface implementation points up to the Implementation. It flips the dependency flow as seen above. It inverts the dependency.

The classes only depend upon interfaces. They don't depend upon each other. They don't even know about each other.

<img src="https://upload.wikimedia.org/wikipedia/commons/8/8d/DIPLayersPattern.png" alt="Dependency Inversion Principle" width = "65%" align="center" style="padding-right: 20px;">

But this only part of the story. We inverted the dependencies in our project, mostly. I inverted the dependencies in my code, mostly. Yet the code was still coupled. Notice above that there are two potential paths from `ClientApplication` to `MyClass`. 

<img src="/assets/DependencyInjectionSetUp.png" alt="Dependency Injection Set Up" width = "75%" align="center" style="padding-right: 20px;">

One path is across the top and then down. That path is inverted, and not the problem. The second path is down and across the bottom. That one is the problem. Follow the arrows. There's a dependency chain from `ClientApplication` to `MyInterfaceFactory` to `MyClass` in the object resolution. `ClientApplication` still depends upon `MyClass`, but it's just not quite as obvious. I had not inverted all the dependencies.

# Dependency Injection
<img src="https://m.media-amazon.com/images/I/A1d1xag7ZeL._SL1500_.jpg" alt="Meat Injector" width = "20%" align="right" style="padding-right: 20px;">
 
At about that same time, I was reading [Martin Reddy](https://www.martinreddy.net/)’s [API Design for C++](https://www.amazon.com/API-Design-C-Martin-Reddy/dp/0123850037)
I think this was one of the first times I had heard of [Dependency Injection](https://en.wikipedia.org/wiki/Dependency_injection) (DI). Here is some of what he wrote:

>Dependency injection is a technique where an object is passed into a class (injected) instead of having the class create and store the object itself. Martin Fowler coined the term in 2004 as a more specific form of the Inversion of Control concept.

I didn’t understand Dependency Injection at first, and I didn’t realize it could be the design solution to my problem.

DI is a technique we’ve all used before. We inject data into objects all the time. Consider the following code that injects a name into `Person` via a constructor argument:
```java
class Person {
    private String name;

    public Person(String name) {
        this.name = name;
    }
}
…
Person person = new Person(“Tommy”);
```

The `Person` class does not depend upon the name being “Tommy” or “Joe” or “Jane”. It can handle any of these names.

We can do the exact same thing with Dependency Injection. The only distinction is that dependencies usually aren’t basic types.
```java
class ClientApplication {
    private MyInterface myInterface;
    
    public ClientApplication(MyInterface myInterface) {
        this.myInterface = myInterface;
    }
    …
    myInterface.doThis();
}
…
ClientApplication clientApplication = new ClientApplication(new MyClass());
```

# Dependency Injection Helps Support Dependency Inverse Principle
Let’s see how DI would have helped solve my DIP issue. There is no dependency path from `ClientApplication` to `MyClass`:

<img src="/assets/DependencyInjection.png" alt="Dependency Injection" width = "95%" align="center" style="padding-right: 20px;">
 
And here’s how I could test the Exception case. I would not have needed to change any existing code. I only needed to introduce `MyTestDouble` and `TestConfigurer`, which doesn’t affect the previous design. `ClientApplication` and `MyInterface` are untouched. `MyClass` does not appear in this design, but that's okay. I'm not interested in `MyClass` for this test. I'm only interested in how `ClientApplication` response when a call to `MyInterface` throws an exception.

<img src="/assets/DependencyInjectionTesting.png" alt="Dependency Injection Testing" width = "95%" align="center" style="padding-right: 20px;">
 
There’s a lot in both diagrams. I need to clarify some details.
## `MyInterface` and `ClientApplication`
`MyInterface` is the same in all these diagrams in this blog. It's an interface contract. It has no external knowledge of how it's used or how it's implemented.

`ClientApplication` is almost the same. The only difference is that the object reference to `MyInterface` is injected into it via a constructor argument/parameter. This breaks the dependency that `ClientApplication` had upon `MyClass` and `MyTestDouble` in the previous diagrams. Now, it only depends upon `MyInterface`. `ClientApplication` will rarely change in subsequent diagrams.

## Configurer?
`Configurer`? It’s not a real word. I made it up. I prefer _Orchestrator_ but it’s too close to _Orchestration_, which has other meanings. I considered _Dependency Injector_ or _Injector_, but that’s an implementation technique. I want to convey design intent. Martin Fowler used the term _Assembler_ in [Inversion of Control Containers and the Dependency Injection pattern](https://martinfowler.com/articles/injection.html), but that's a term that used elsewhere too.

I have finally settled on a term I used many years ago when presenting Design Patterns Lunch & Learn seminars at the office. _Configurer_ appeared in many of my Lunch & Learn Design Pattern diagrams, often as an external actor. I'm resurrecting it for these blogs too. The concept rarely appears in the Gang of Four diagrams. This is another sin of omission in their book in my humble opinion.

## Red Lines
The red lines are design/architectural boundaries, and they convey the same intent as seen in [Factories: Abstract Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html#abstract-factory). They constrain the flow of knowledge in a design.

I could go deep into the implications of knowledge boundaries and arrows, and I may go into more detail a future blog, but I’ll try to highlight what’s important now:
* `ClientApplication` and `MyInterface` are isolated in the upper right quadrant of this diagram. All class relationships cross the red boundary pointing inward. These entities have no knowledge or dependency upon other elements of the design. The rest of the design is invisible to them.
* `Configurer` and `TestConfigurer` are on the far left of the diagram. All class relationships cross the red boundary pointing outwardly from these classes. These elements have knowledge and dependency of the entire design, but they restrain themselves. They only use this knowledge to create and configure the objects in the rest of the design. They don’t contain any business logic. Not only are Configurers invisible to the rest of the design. They are invisible to each other as well. This design supports as many Configurers as needed.
* `MyClass` and `MyTestDouble` are in the lower right quadrant of this diagram. They have knowledge of and depend upon `MyInterface`. They tend to be small, and they are often [Adapters](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) or [Façades](https://jhumelsine.github.io/2023/10/03/facade-design-pattern.html). Only the `Configurer` knows about them, so they can be swapped out easily as needed.

# Turtles All the Way Down
<img src="https://upload.wikimedia.org/wikipedia/commons/4/47/River_terrapin.jpg" alt="Turtles All the Way Down" width = "35%" align="right" style="padding-right: 20px;">

But wait a second! `Configurer` is instantiating `MyClass` via `new()`. What happened to those [Factories Design Patterns](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) from the previous blog post?

I had two mental blocks with Dependency Injection when I first started to read about it:
* Was I still going to have to inject all dependencies for production and unit testing?
* Where does it end? We're not kicking the can down the road anymore so much as we're kicking it under the rug, which really isn't a very good metaphor.

## The Complete Set of Dependencies
Was I still going to have to inject all dependencies for production and unit testing?

Short answer: Yes and No.

Production configuration will require resolution for all layers of dependencies, even dependencies of dependencies. This might get a bit large and complex, but it's only creating instances and assembling them together. An understanding of the parts of the architecture is needed, but there is no business logic.

Here's an example where:
* `ClientApplication` delegate to `Interface1`
* `Implementation1` implements `Interface1`, and it delegates to `Interface2` and `Interface3`
* `Implementation2` and `Implementation3` implement `Interface2` and `Interface3` respectively.
* `ProductionConfigurer` creates and connects the dependencies as follows:

<img src="/assets/DependencyInjectionProductionConfigurer.png" alt="Dependency Injection Production Configurer" width = "75%" align="center" style="padding-right: 20px;">

We can see that `ProductionConfigurer` is creating an instance of `ClientApplication` by creating instances of its dependencies and injecting them as constructor arguments. The developer who implements `ProductionConfigurer` will most likely be an architect or team lead. It's more important to have knowledge of the overall design than it is to have specific knowledge about any of the individual classes.

`ProductionConfigurer` is not limited to calling `new` directly. Here are a few other options:
* `ProductionConfigurer` could create and inject using [Factories Design Patterns](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html)
* `ProductionConfigurer` could be based upon an inject framework, such as **Spring** described below
* If configuration is more dynamic or configurable, then `ProductionConfigurer` could leverage the **Builder Design Pattern** (See: [Source Making Builder](https://sourcemaking.com/design_patterns/builder) or [Refactoring Guru Builder](https://refactoring.guru/design-patterns/builder))

Unit test configuration will require resolution for the first layer of dependencies only. The interface is a contract. It is not an implementation. Implementation knowledge and dependencies are encapsulated behind it. It doesn't matter how simple or complex the actual implementation is. We only need to configure the bare minimum in our `Test Doubles`, and that may only be a single behavior for a single method too.

Here's how we could test `ClientApplication` in isolation with `Test Doubles`:

<img src="/assets/DependencyInjectionClientApplicationTestConfigurer.png" alt="Dependency Injection Client Application Test Configurer" width = "50%" align="center" style="padding-right: 20px;">

Notice how all the `Implementation` classes and their interfaces are not present, except for `Interface1`. And `Interface1` is only shown, because `ClientApplication` depends upon it. This is part of the elegance of [Program to an interface, not an implementation](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html#program-to-an-interface-not-an-implementation).
The dependency chain stops at `Interface1`. There is no dependency upon `Implementation1` even if `Implementation1` will implement `Interface1` in production as was shown in the previous diagram.

Even if the production configuration requires multiple layers of dependency, we only need to configure one layer at a time for testing.

Here's how we could test `Implementation1` in isolation with `Test Doubles`:

<img src="/assets/DependencyInjectionImplementation1TestConfigurer.png" alt="Dependency Injection Client Application Test Configurer" width = "50%" align="center" style="padding-right: 20px;">

And like the previous diagram, we only need to configure one layer to resolve the dependencies.

## Kicking the Can Under the Rug
Dependency Injection is a Creational Design Pattern, even if it wasn’t included by the Gang of Four. I don’t think this was a sin of omission as much as it was a case of DI not being too well known when they were writing their book.

I think it’s okay to resolve object references via `new` via DI, but we’ll soon see that we can use Factories with DI as well.

I think there’s another reason why `new()` is okay in Configurers. Bob Martin talks about these boundaries in [Clean Architecture](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164/):
* The upper right quadrant is cocooned business logic. It has no concrete external dependencies. All communication to external dependencies is via interfaces
* The lower right quadrant contains the concrete dependencies. They tend to be small implementations, especially when they are [Adapters](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) or [Façades](https://jhumelsine.github.io/2023/10/03/facade-design-pattern.html).
* We are kicking the can under the rug, but we will eventually reach the bottom. The Configurers are in the “lowest” section of your code. They reside in the bootstrap portion of the feature, possibly `main()`. It’s so “low” in the code that you can use `new()` and not get yourself into too much trouble.

# Dependency Inject and Factories
While we can call `new()` using DI, we can still use Factories with them as well.
Here’s one example:

<img src="/assets/DependencyInjectionFactory.png" alt="Dependency Injection Factory" width = "95%" align="center" style="padding-right: 20px;">

As for the Exception set up, it would be identical to the Exception Test UML class diagram shown above. There’s no need for a Factory in that test.

And here’s how DI can work with Abstract Factory:

<img src="/assets/DependencyInjectionAbstractFactory.png" alt="Dependency Injection Abstract Factory" width = "95%" align="center" style="padding-right: 20px;">
 
Here’s the Exception Test diagram:

<img src="/assets/DependencyInjectionAbstractFactoryTest.png" alt="Dependency Injection Factory Test" width = "95%" align="center" style="padding-right: 20px;">

# Spring
There are Dependency Injection frameworks as well, with Spring probably being the most well-known for Java. It’s very similar to what I’ve shown previously, except that there are a few annotations. As for that _Spring Magic_, that's part of the Spring framework. It will handle everything you need if what you need resides in the framework. You won't have direct access to the _Spring Magic_ itself. For the most part, you don't even see it.

<img src="/assets/DependencyInjectionSpring.png" alt="Dependency Injection Spring" width = "95%" align="center" style="padding-right: 20px;">

You have at least two options with testing, and they will be very similar to what we've seen above. I'll describe them without providing additional images:
* It's no different than the test designs shown before. Even with Spring annotations, they are still Java classes. A _Test Double_ can be created and injected when an object instance of `ClientApplication` as its constructor argument when created via `new()`. This can be done in the test code without depending upon the Spring framework.
* [JUnit](https://en.wikipedia.org/wiki/JUnit) can create _Test Doubles_ via [Mockito](https://en.wikipedia.org/wiki/Mockito) with Spring (and also without Spring), to inject what's needed as well.
 
# Dependency Injection vs Dependency Inversion Principle vs Inversion of Control
These are three concepts with similar names and similar concepts, but still different:
* **Dependency Injection** is a design pattern/technique that injects dependencies into a class, usually as a constructor argument.
* **Dependency Inversion Principle** is not allowing a class to have any knowledge or dependency upon a class that it delegates to.
* **Inversion of Control** is how frameworks work. The inversion is that the framework calls your code.

# Summary
Dependency Injection feels a little strange at first glance, but it’s a useful way to resolve dependencies while maintaining flexibility in the design.
This wraps up the [Essential Design Patterns](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html).

# References
Here are some free resources:
* Dependency Injection:
    * [Wikipedia](https://en.wikipedia.org/wiki/Dependency_injection)
    * [Inversion of Control Containers and the Dependency Injection pattern](https://martinfowler.com/articles/injection.html) by Martin Fowler
    * [A Practical Introduction To Dependency Injection](https://www.smashingmagazine.com/2020/12/practical-introduction-dependency-injection/) by Jamie Corkhill
    * [Java Dependency Injection - DI Design Pattern Example Tutorial](https://www.digitalocean.com/community/tutorials/java-dependency-injection-design-pattern-example-tutorial) by Pankaj
    * [Guide to Spring @Autowired](https://www.baeldung.com/spring-autowire) by Baeldung
* Dependency Inversion Principle:
    * [Wikipedia](https://en.wikipedia.org/wiki/Dependency_inversion_principle)
    * [System Design: Dependency Inversion Principle](https://www.baeldung.com/cs/dip) by Baeldung
* Inversion Of Control:
    * [Wikipedia](https://en.wikipedia.org/wiki/Inversion_of_control)
    * [Inversion of Control](https://martinfowler.com/bliki/InversionOfControl.html) by Martin Fowler
    * [Intro to Inversion of Control and Dependency Injection with Spring](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring) by Baeldung

Here are some resources that can be purchased or are included in a subscription service:
* **API Design in C++** Chapter 3 by Martin Reddy ([O'Reilly](https://learning.oreilly.com/library/view/api-design-for/9780123850034/xhtml/chp003.xhtml) and [Amazon](https://www.amazon.com/API-Design-C-Martin-Reddy/dp/0123850037/))
* **Clean Architecture** Chapter 11 by Robert Martin ([O'Reilly]([https://learning.oreilly.com/library/view/clean-architecture-a/9780134494272/](https://learning.oreilly.com/library/view/clean-architecture-a/9780134494272/ch11.xhtml)https://learning.oreilly.com/library/view/clean-architecture-a/9780134494272/ch11.xhtml) and [Amazon](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164/))
