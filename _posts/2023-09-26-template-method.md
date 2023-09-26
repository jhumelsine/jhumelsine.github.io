---
title: Template Design Pattern
description: For when your algorithm is only a skeleton, and you need to put some meat on those bones.
unlisted: true
---

![MadLibs](https://miro.medium.com/v2/resize:fit:1400/1*8M2JfaTacGjI8YQlO9qF5A.jpeg)
 
# Strategy and Template Method are the Same, Except When They’re Not
The Template Method Design pattern continues the themes also found in the [Command](https://jhumelsine.github.io/2023/09/18/command-design-pattern.html) and [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) design patterns. **The primary Object-Oriented (OO) mechanism in all three patterns is polymorphism.** Of the three, Template Method and Strategy are the most closely related. 

Strategy declares a contract, often via an interface, which is wholly implemented within extending classes. Each extending class has complete freedom in how its implementation honors the contract.

Whereas, Template Method declares a contract, via an abstract base class, which retains some if not most of the implementation that honors the contract. However, it may not have enough specific information for its implementation to complete the contract. Specific information is implemented in extending classes within protected methods declared in the base class as abstract methods. The implementation in the abstract base class accesses the specific extending protected methods via polymorphism by delegating to their abstract protected methods. Different extending classes allow for different behaviors.

The structure is much like a [Mad Libs](https://www.madlibs.com/) game. Most of the story is declared in the Mad Libs template, but there are missing words identified by their parts of speech. See the _The Magic Computers_ image at the top of the page. Each player provides their own words so that each story is similar yet different.

# Structure
Template Method’s UML Class Diagram looks very similar to Strategy’s, except that Template Method’s contract is declared and partially implemented within an abstract base class; whereas Strategy’s contract is declared in an interface and wholly implemented within the extending classes.

The [Project Management Institute](https://www.pmi.org/disciplined-agile/the-design-patterns-repository/the-template-method-pattern) provides a good diagram. I like this diagram, since it includes the Client. Here’s a brief description of each element:
* `TargetAbstraction` – This is the abstract base class which declares the contract as it’s represented in the `templateMethod()`.
* `templateMethod()` – This is the contract. It’s a public method, and the diagram shows a bit of it. Not only does this contract specify what the contract is, but it also declares a little bit about how it will be implemented. In this example, `templateMethod()` is implemented as three steps, which declared as protected abstract methods. Not all methods must be protected abstract methods. It’s common for private methods to be part of the template method’s definition too.
* `ImplementationA` and `ImplementationB` - These are classes that extend `TargetAbstraction`. They must provide implementations for each of the steps, and each class determines the behavior that’s appropriate for it.

![Template Method UML Class Diagram](https://www.pmi.org/-/media/pmi/microsites/disciplined-agile/the-design-patterns/template_method_0.jpg
)
 
The diagram doesn’t indicate what’s in the `Client`. It will probably be something along the lines of:
```java
TargetAbstraction target = TargetAbstractions.acquire(“A”);
target.templateMethod();
```
In this example, `templateMethod()` is the only method that the `Client` can access. The `step()` methods are not accessible. They are implementation details encapsulated from the `Client`.
# Coffee, Tea or Me
A common example of Template Method is a Coffee Shop, and it’s often named Starbuzz.
Here’s an example from [CodeProject](https://www.codeproject.com/Articles/21221/Template-Method-Design-Pattern-vs-Functional-Progr).

In this example:
* `HotDrink` contains the contract `MakeRecipe`. While not show in the diagram the recipe consists of the following in this order:
    * `BoilWater` - Never Changes
    * `Brew` - Changes depending upon Coffee or Tea
    * `PourInCup` - Never Changes
    * `AddCondiments` - Changes depending upon Coffee or Tea

![HotDrink UML Class Diagram](https://www.codeproject.com/KB/architecture/TMDPvsLambda/image002.jpg)
 
Imagine how this would be updated to make `HotCocoa`. Assume that the Cocoa powder does not require heated milk. What would be the condiments?

# Situations Where Template Method Could Be Useful
<img src="https://i.pinimg.com/originals/ae/20/73/ae207372837cec0d56db1db481893176.jpg" alt="Spider Man Great Power Great Responsibility" align="right" width = "15%" style="padding-right: 20px;">
Template Method houses more implementation than the Command and Strategy design patterns. There are some behaviors that the client application cannot avoid. For example, in the `HotDrink` example above there are exactly four steps, and the extended classes are unable to affect them.
<P>

This is a great power that the Template Method wields over the rest of the classes in the pattern. **With great power comes great responsibility.** Make sure that Template Method is being used appropriately.

## Consolidate Similar Methods/Behaviors
As was seen in my [CircuitPack example](https://jhumelsine.github.io/2023/09/22/RightAbstractionIsHard.html), Template Method can be used to implement abstractions once they’re discovered. It’s useful when the classes are similar, but still a little different. The similarities can be moved into an abstract base class and the differences can remain in the extended classes.

## Don’t Abdicate Responsibility
One of my pet peeves with some OO practices, and I’m talking to you Java, is that it’s too easy to override methods. By default, all methods in Java can be overridden. Developers can prevent this by making a method `final`, but I’d prefer a paradigm where the method is final unless declared as overridable. Of course, the horse is out of the barn on this one with Java.

With an overridable default, a class developer is basically conveying in the contract, “I think this method will satisfy your needs. But it may not. If it doesn’t then override it. Oh, and if you do decide to override it, be sure to honor the intent of the contract.” I’m not sure all developers honor contracts when overriding methods.

What if the method being overridden performs authentication and authorization rejecting an invalid client request? How do we know that the extending class will also perform the authentication and authorization? We don’t.

But we can use the Template Method to get the best of both worlds. Authentication and Authorization cannot be overridden, and the extending class can still override the basic behavior.
```java
class Behavior {
    public final void doBehavior(User user) throws NotAuthenticatedException, NotAuthorizedException {
        if (!isAuthenticated(user)) throw new NotAuthenticatedException(user);
        if (!isAuthorized(user)) throw new NotAuthorizedException(user);
        behavior(user);
    }

    protected void behavior(User user) {
        // Default behavior. This can be overridden.
    }
}

…
class SpecialBehavior extends Behavior {
    @Override
    protected void behavior(User user) {
        // Overriding Special Behavior
    }
}
```
The above is not a classical Template Method. `Behavior` is not an abstract class and `behavior` is not an abstract method. However, it’s the same principle.
If we make `behavior()` abstract, then there is not default implementation, and it is Template Method.

## Temporal Coupling
Sometimes things must be executed in a specific order. For example, with our `HotDrink` pouring the water into the cup before boiling it wouldn’t work so well.

Template Method provides a means to ensure order. `HotDrink` will always perform the same operations in the same order, even if some of them are delegated to extending classes.

## Frameworks and Hollywood Principle


There are two basic techniques for code reuse:
* Libraries, where the application code delegates to methods in other classes. Utility libraries are like this.
* Frameworks, where the application code implements methods declared by the framework and called by the framework.

<img src="https://lh3.googleusercontent.com/60NpYzzxYr4v60JZTR9c__o-7bABBoYI1ff9ancTw42FYlS6MKsWjjyXpMPlQ8g1LzTJ0uVKQMcKvRioCuswfkKMKFoWUKQg23zCQTZJ0-ruGQ1uMJNKDPv-gxV-W5ufHNTIG4ii" alt="Don't Call Us, We'll Call You" align="right" width = "25%" style="padding-right: 20px;">

Frameworks are also known by the [Hollywood Principle](https://dzone.com/articles/the-hollywood-principle) – _Don’t call us, we’ll call you._
 
While Frameworks may use different implementations, the underlying concepts are still what we see in the Template Method design pattern. All of the previous examples provided are examples of frameworks, even if very small. `HotDrink` is a framework for making a hot drink.

The Android [`Activity`](https://developer.android.com/reference/android/app/Activity) class is part of the Android framework. For the most part, each Android window has an `Activity` that manages its state depending upon whether it’s actively running, partially covered, newly visible, etc.

Several states in the state machine call the protected methods, and if the application developer has implemented them, they will be executed. One can think of these protected methods as a means for the application developer to manage resources used by the Activity. For example, if the Activity is not visible, then a battery draining resource, such as the GSP, could be deactivated in `onPause()` and reactivated in `onRestart()`.

Keep in mind that the developer never calls these methods directly. It’s the Activity framework that calls what the developer has provided.
![Activity State Machine Lifecycle](https://developer.android.com/images/activity_lifecycle.png)
 
# Template Method and Strategy Comparison
Template Method and Strategy solve the same problem but in different ways.

If we start with Strategy, we can move the Client Application method into the Template Method abstract class and what had been the interface methods will become the abstract protected methods.

If we start with the Template Method, we can move the method in the Template Method into the Client Application and what had been the abstract protected methods will become the interface methods.

I’ve started with Template Method in mind only to move to Strategy. And I’ve started with Strategy in mind only to move to Template Method.

As for which to use, I’ll defer to [When to use template method Vs. Strategy?](https://stackoverflow.com/questions/672083/when-to-use-template-method-vs-strategy) on Stack Overflow.

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* [Wikipedia Template Method Design Pattern](https://en.wikipedia.org/wiki/Template_method_pattern)
* [Source Making Template Method Design Pattern](https://sourcemaking.com/design_patterns/strategy)
* [Refactoring Guru Template Method Design Pattern](https://refactoring.guru/design-patterns/template-method)
* [Project Management Institute Template Method Design Pattern](https://www.pmi.org/disciplined-agile/the-design-patterns-repository/the-template-method-pattern)
* and for more Google "Template Method Design Pattern”

Here are some resources that can be purchased or are included in a subscription service:
* [Gang of Four Template Method Design Pattern](https://learning.oreilly.com/library/view/design-patterns-elements/0201633612/ch05.html#page_325)
* Agile Principles, Patterns, and Practices in C#, Chapter 22 ([O'Reilly](https://learning.oreilly.com/library/view/agile-principles-patterns/0131857258/) and [Amazon](https://www.amazon.com/Agile-Principles-Patterns-Practices-C/dp/0131857258))
* Clean Code: Design Patterns, Episode 27 video ([Clean Coders](https://cleancoders.com/episode/clean-code-episode-27) and [O'Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_03_27_00/))
* Head First Design Patterns, Chapter 8 ([O'Reilly](https://learning.oreilly.com/library/view/head-first-design/9781492077992/ch08.html) and [Amazon](https://www.amazon.com/Head-First-Design-Patterns-Object-Oriented-ebook/dp/B08P3X99QP))
