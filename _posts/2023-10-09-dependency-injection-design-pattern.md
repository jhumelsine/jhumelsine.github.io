---
title: Dependency Injection
description: Dependency Injection Demystified - Crafting Clean Code with Ease
unlisted: true
---

<img src="https://ae01.alicdn.com/kf/S7fdd40fce12042769a255dce921cb97dg/Stainless-Steel-Spice-Syringe-Marinade-Injector-Flavor-Syringe-Cooking-Meat-Poultry-Turkey-Chicken-Kitchen-BBQ-Tool.jpg" alt="Meat Injector" width = "55%" align="center" style="padding-right: 20px;">

 
# Introduction
I’m finally done kicking the object resolution can down the road. This wraps up the [Essential Design Patterns](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html).

About a dozen years ago, I worked on a small team with a tightly knit group of developers. I wrote some of the best code of my career while on that team. It just wasn’t the team that was tight. Our code was tightly coupled as well. It was modular, but it was still tightly coupled.

Often when we needed to test new behavior, we’d compile the entire product on our desktop machines, copy the executable to a thumb drive, carry it into our QA lab, which had about 3 servers on it, and hope that at least one of them was available. We’d copy our latest executable from the thumb drive to a server and then try to observe the new or updated functionality through the user interface.

If the behavior was broad, then we could probably see it. If the behavior was more subtle, then we probably couldn’t see it. Edge cases were very difficult to confirm.

We had [CppUnit](https://en.wikipedia.org/wiki/CppUnit), which is the C++ version of [xUnit](https://en.wikipedia.org/wiki/XUnit). We could write tests in CppUnit, but we didn’t really understand unit testing. I tried writing CppUnit tests several times, but after a few weeks, they would start breaking, because they were brittle. It was another several years and another company before I figured out unit testing.

Some of my code was decoupled enough that I could test it on my desktop. I could test the sunny day scenarios, but I was having a real problem testing the rainy-day scenarios. For example, I wanted to confirm that my code handled an Exception correctly, but I couldn’t make the code throw an Exception easily.

I had been using Design Patterns for about 7 years by this point. What was I missing? Here’s an extremely high-level sketch of my basic design. Keep in mind that I want to confirm the expected behavior in `ClientApplication` when it calls `myInterface.doThis()` and an Exception is thrown.

<img src="/assets/DependencyInjectionSetUp.png" alt="Dependency Injection Set Up" width = "55%" align="center" style="padding-right: 20px;">
 
Why am I having this problem? I’m following good Design Pattern Principles:
* I’m [programming to an interface, not an implementation](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html#program-to-an-implementation-not-an-interface).
* I’m encapsulating the implementation, `MyClass`, from `ClientApplication` by acquiring it via a [factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html).

I figured out a way to force an Exception, but it was nasty:
* I created a new class, `MyTestDouble`, which threw the Exception when `doThis()` was called.
* I would change the implementation in `MyInterfaceFactory` to return an instance of `MyTestDouble`.
* I would confirm the behavior via a manual test.
* I would change `MyInterfaceFactory` back to its original implementation to return `MyClass`.

<img src="/assets/DependencyInjectionForcedException.png" alt="Dependency Injection Forced Exception" width = "55%" align="center" style="padding-right: 20px;">
 
This worked, but it was painful. A test took a long time to set up and it required a change to the Factory. What if I ever forgot to revert the Factory back to its proper implementation?

# Dependency Inversion Principle
What was I doing wrong?

I was violating the [Dependency Inversion Principle](https://en.wikipedia.org/wiki/Dependency_inversion_principle) (DIP), even though I had not heard of it before. 
If an application is delegating to an interface, then the application should not be responsible to resolve that interface even if doing so indirectly.

`ClientApplication` as still deterministically resolving `MyInterface` as `MyClass` even if it didn’t know `MyClass`. `ClientApplication` was delegating to `MyInterfaceFactory`, which was creating `MyClass`.

Each of the lines in the diagram above has an arrowhead, which indicates knowledge and dependency, even if that knowledge and dependency is indirect. `ClientApplication` indirectly depends upon `MyClass`. Even my hack with `MyTestDouble` has the same problem.

# Dependency Injection
<img src="https://m.media-amazon.com/images/I/A1d1xag7ZeL._SL1500_.jpg" alt="Meat Injector" width = "20%" align="right" style="padding-right: 20px;">
 
At about that same time, I was reading [Martin Reddy](https://www.martinreddy.net/)’s [API Design for C++](https://www.amazon.com/API-Design-C-Martin-Reddy/dp/0123850037)
I think this was one of the first times I had heard of Dependency Injection (DI). Here is some of what he wrote:

>Dependency injection is a technique where an object is passed into a class (injected) instead of having the class create and store the object itself. Martin Fowler coined the term in 2004 as a more specific form of the Inversion of Control concept.

I didn’t understand Dependency Injection at first, and I definitely didn’t realize it could be the design solution to my problem.

DI is a technique we’ve all used before. We inject data into objects all the time. Consider the following code where we inject a name into `Person` via a constructor argument:
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

We can do the exact same thing with dependencies. The only distinction is that dependencies usually aren’t basic types.
```java
class ClientApplication {
    private MyInterface myInterface;
    
    public ClientApplication(MyInterface myInterface) {
        This.myInterface = myInterface;
    }
    …
    myInterface.doThis();
}
…
ClientApplication clientApplication = new ClientApplication(new MyClass());
```

# Dependency Injection Helps Support Dependency Inverse Principle
Let’s see how DI would have helped solve my DIP issue:

<img src="/assets/DependencyInjection.png" alt="Dependency Injection" width = "85%" align="center" style="padding-right: 20px;">
 
And here’s how I could test the Exception case. I would not have needed to change any existing code. I only needed to introduce `MyTestDouble` and `TestConfigurer`, which doesn’t affect the previous design.

<img src="/assets/DependencyInjectionTesting.png" alt="Dependency Injection Testing" width = "85%" align="center" style="padding-right: 20px;">
 
There’s a lot in both diagrams. I need to clarify some details.
## `MyInterface` and `ClientApplication`
`MyInterface` is the same in all these diagrams in the blog.

`ClientApplication` is almost the same. The only difference is that the object reference to `MyInterface` is injected into it via a constructor argument/parameter. This breaks the dependency that `ClientApplication` had upon `MyClass` and `MyTestDouble` in the previous diagrams. Now, it only depends upon `MyInterface`. `ClientApplication` will rarely change in subsequent diagrams.
## Configurer?
`Configurer`? It’s not a real word. I made it up. I prefer Orchestrator but it’s too close to Orchestration, which has other meanings. I considered Dependency Injector or Injector, but that’s an implementation technique. I want to convey design intent.

I have finally settled on a term I used many years ago when presenting Design Patterns Lunch & Learn seminars. The Configurer appears in many of my Design Pattern diagrams. It rarely appears in the Gang of Four diagrams. This is another sin of omission in their book in my humble opinion.

## Red Lines
The red lines are design/architectural boundaries, and they convey the same intent as seen in [Factories: Abstract Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html#abstract-factory). They constrain the flow of knowledge in a design.

I could go deep into the implications of knowledge boundaries and arrows, and I may go into more detail a future blog, but I’ll try to highlight what’s important now:
* `ClientApplication` and `MyInterface` are isolated in the upper right quadrant of this diagram. All class relationships cross the red boundary pointing inward. These entities have no knowledge or dependency upon other elements of the design. The rest of the design is invisible to them.
* `Configurer` and `TestConfigurer` are on the far left of the diagram. All class relationships cross the red boundary pointing outwardly from these classes. These elements have knowledge and dependency of the entire design, but they restrain themselves. They only use this knowledge to create and configure the objects in the rest of the design. They don’t contain any business logic. Not only are Configurers invisible to the rest of the design. They are invisible to each other as well. The design supports as many Configurers as needed.
* `MyClass` and `MyTestDouble` are in the lower right quadrant of this diagram. They have knowledge of and depend upon `MyInterface`. Only the `Configurer` knows about them, so they can be swapped out easily as well.

But what a second! `Configurer` is instantiating `MyClass` via `new()`. What happened to those [Factories Design Patterns](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) from the previous blog post?

Dependency Injection is a Creational Design Pattern, even if it wasn’t included by the GoF. I don’t think this was a sin of omission as much as it was a case of DI not being too well known when they were writing their book.

I think it’s okay to resolve object references via `new` via DI, but we’ll soon see that we can use Factories with DI as well.

I think there’s another reason why `new()` is okay in Configurers. Bob Martin talks about these boundaries in his _Clean Architecture_ designs:
* The upper right quadrant is cocooned business logic. It has no concrete dependencies.
* The lower right quadrant contains the concrete dependencies. They tend to be small implementations, especially when they are [Adapters](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html).
* The Configurers are in the “lowest” section of your code. They reside in the bootstrap portion of the feature, possibly `main()`. It’s so “low” in the code that you can use `new()` and not get yourself into too much trouble.

# Dependency Inject and Factories
While we can call `new()` using DI, we can still use Factories with them as well.
Here’s one example:

<img src="/assets/DependencyInjectionFactory.png" alt="Dependency Injection Factory" width = "85%" align="center" style="padding-right: 20px;">

As for the Exception set up, it would be identical to the Exception Test UML class diagram shown above. There’s no need for a Factory in that test.

And here’s how DI can work with Abstract Factory:

<img src="/assets/DependencyInjectionAbstractFactory.png" alt="Dependency Injection Abstract Factory" width = "85%" align="center" style="padding-right: 20px;">
 
Here’s the Exception Test diagram:

<img src="/assets/DependencyInjectionAbstractFactoryTest.png" alt="Dependency Injection Factory Test" width = "85%" align="center" style="padding-right: 20px;">

 
# Spring
There are Dependency Injection frameworks as well, with Spring probably being the most well-known for Java. It’s very similar to what I’ve shown previously, except that there are a few annotations:

<img src="/assets/DependencyInjectionSpring.png" alt="Dependency Injection Spring" width = "85%" align="center" style="padding-right: 20px;">
 
# Dependency Injection vs Dependency Inversion Principle vs Inversion of Control
These are three concepts with similar names and similar concepts, but still different:
* Dependency Injection is a design pattern/technique that injects dependencies into a class, usually as a constructor argument.
* Dependency Inversion Principle is not allowing a class that depends upon an interface to instantiate the reference to that interface.
* Inversion of Control is how frameworks work. The inversion is that the framework calls your code.

# Summary
Dependency Injection feels a little strange at first glance, but it’s a useful way to resolve dependencies while maintaining flexibility in the design.
This wraps up the [Essential Design Patterns](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html).

# References
Here are some free resources:
* https://en.wikipedia.org/wiki/Dependency_injection
* https://www.smashingmagazine.com/2020/12/practical-introduction-dependency-injection/
* https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring
* https://www.digitalocean.com/community/tutorials/java-dependency-injection-design-pattern-example-tutorial
* https://martinfowler.com/articles/injection.html
* https://en.wikipedia.org/wiki/Dependency_inversion_principle
* https://www.baeldung.com/cs/dip
* https://en.wikipedia.org/wiki/Inversion_of_control
* https://martinfowler.com/articles/injection.html
* https://martinfowler.com/bliki/InversionOfControl.html
* https://www.baeldung.com/spring-autowire

Here are some resources that can be purchased or are included in a subscription service:
* https://learning.oreilly.com/library/view/api-design-for/9780123850034/xhtml/chp003.xhtml
* https://learning.oreilly.com/library/view/clean-architecture-a/9780134494272/
