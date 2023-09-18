---
title: Command Design Pattern
description: Turn a function into a first-class citizen object so that it can do much more than before.
unlisted: true
---

![Picard Make It So](https://i.stack.imgur.com/MNeE7.jpg)

# Command, Meh

I originally didn’t consider Command an [Essential Design Pattern](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html). 
I wasn’t even sure why it was listed as a design pattern originally. 
It just seemed like polymorphism. 

This is the essence of the Command Design Pattern:
```
interface Command {
     void execute();
}
```

# That’s it… but what is it?

Bob Martin changed my mind in the Command portion of his **_Agile Principles, Patterns, and Practices in C#_** book, Chapter 21, ([O'Reilly](https://learning.oreilly.com/library/view/agile-principles-patterns/0131857258/) and [Amazon](https://www.amazon.com/Agile-Principles-Patterns-Practices-C/dp/0131857258)) and **_Clean Code: Design Patterns_** video. Episode 25 ([Clean Coders](https://cleancoders.com/episode/clean-code-episode-25) and [O'Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_03_25_00/)).

Objects are first-class citizens in Object-Oriented languages. There’s a popular saying in Java: *Everything is an object in Java*.

Functions are declared in classes as methods. They are not objects. They can be called directly, but that’s about it. They cannot be passed as arguments, added to data structures, etc. Other than calling them, there’s not much else we can do with them.

 The Command Design Pattern allows us to wrap a function within a class, instantiate it as an object and then execute it through an Interface via polymorphism. By “objectifying” the function, it becomes a first-class citizen. **A function wrapped within an object can do anything an object can do.**

There is nothing difficult or sophisticated in the implementation of Command. It’s about intent, context and mindset. Once a function is an object, we can do much more with it.
Here’s a simple example of a Command implementation:
```
class HelloWorld implements Command {
    void execute() {
        System.out.println(“Hello World!”);
    }
}

...

Command command = new HelloWorld();
command.execute();
```

Command can support parameters by passing them as constructor arguments. Here's a more generalized version of the HelloWorld above, but this version prints any String:
```
class Printer implements Command {
    private final String text;

    public Printer(String text) {
        this.text = text;
    }

    public void execute() {
        System.out.println(text);
    }
}

...

Command printer = new Printer("Hello There World!");
printer.execute();
```
# Command Decouples What from Whom
The examples above created instances of `HelloWorld` and `Printer` directly by invoking the `new()` operation. This violates the first design principle: [Program to an interface rather than an implementation](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html#program-to-an-interface-not-an-implementation).
We can use another [Essential Design Pattern](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html), [Factory Method](https://refactoring.guru/design-patterns/factory-method), and acquire the Command this way:
```
Command command = Commands.acquire(“Hello World”);
Command.execute();
```
`Commands.acquire(String commandName)` is a static method that encapsulates the specific class type from the code that uses it. Not only is the class type hidden, but the mechanism for choosing the class type  is hidden.
This decouples what is being executed from the code that is executing it.

A controller that could look something like this:
```
void processAction(Action action) {
    Command actionCommand = Commands.acquire(action);
    actionCommand.execute();
}
```
In the above, `processAction(Action action)` can handle any number of Actions as long as there’s a means to acquire a Command for a specific Action.

A feature can support many different actions. Each would have their own Action specific Command. Each class would be self-contained. Each could be more easily testable.

For more details, see [The Evolution of Command Pattern (I): How Command Pattern has evolved overtime](https://www.lvguowei.me/post/the-evolution-of-command-pattern/) by Guowei Lv.
# Command Decouples What from When
What if `processEvent(Action action)` is executed on the user thread that requires fast response time? It’s possible for a Command to take a significant amount of time before it returns. We can’t block a user thread while waiting for a long running process.

Instead, we can schedule the Command to execute asynchronously on another thread by doing something like this:
```
void processCommand(Action action) {
    Command actionCommand = Commands.acquire(action);
    processQueue.add(actionCommand);
}
```
This code adds the Command onto a queue which unblocks the `processEvent(Action action)`. Another thread process will remove the Command from the queue and execute it.

# Command Supports Undo and Redo
Command has a trick up its sleeve that functions do not have. Since Command is an object, it can have state, as was demonstrated in `Printer` implementation above with its `text` field attribute. A Command object can retain state about what it executed. It has the potential to undo what it executed. NOTE: Some functions cannot be undone. You can’t unring a bell.

Text processors have an undo option. If there’s a Command to make text bold, then there’s an undo to return it to its previous font. And if we’re allowing undo, why not redo as well?
Here is generally how execute/undo/redo would work:
* When a Command is executed, it is pushed onto `done`, which is a stack of Commands. This maintains a history of all Commands that have been executed.
* When an undo is requested, the `done` stack will pop its top Command and the popped Command’s `undo()` method will be invoked. The undone Command will then be pushed onto `undone`, another stack of Commands. This maintains a history of all Commands that have been undone.
* When a redo is requested, the `undone` stack will pop its top Command and its `execute()` method will be executed. It will be pushed onto the `done` stack once more.
* If a new Command is executed, then it will clear the `undone` stack, since the new Command invalidates the previous undone history.

More details can be found in [The Evolution of Command Pattern (II): How Command Pattern has evolved overtime](https://www.lvguowei.me/post/the-evolution-of-command-pattern-2/) also by Guowei Lv. I think there’s a typo in the implementation. The `RedoCommand.undoable()` returns `false`, when I think it should return `true`. He also does not clear the `undoneStack` when a new Command is executed. I don't know if this is his oversight or whether I'm incorrect in my thinking that it should be cleared.
# Command Processor Design Pattern
Most of what I’ve written about scheduling, undo, redo isn’t about Command directly. It’s more about how to leverage Command via the [Command Processor Design Pattern](https://www.dre.vanderbilt.edu/~schmidt/cs282/PDFs/CommandProcessor.pdf), which is not in the Gang of Four catalog. It's part of the [Pattern-Oriented Software Architecture](https://en.wikipedia.org/wiki/Pattern-Oriented_Software_Architecture) series, also known as POSA.

The Command Design Pattern provides a foundation from which the Command Processor Design Pattern can extend it.

# Real-World Example with Java
Java’s [Runnable](https://docs.oracle.com/javase/8/docs/api/java/lang/Runnable.html) interface is an example of Command only with different nomenclature. The structure is identical, but instead of `execute()`, it declares `void run()`. This is part of `run`'s [description](https://docs.oracle.com/javase/8/docs/api/java/lang/Runnable.html#run--):
> The general contract of the method run is that it may take any action whatsoever.

I love that definition. So unobstructed. Runnable allows developers the freedom to objectify "any action whatsoever."

`Runnable` is often associated with Java’s `Thread` class. “Thread” appears in the Runnable documentation, which I disagree with. `Runnable` has no knowledge of or dependency upon `Thread`. It’s `Thread` that has knowledge of and a dependency upon `Runnable`.

A `Runnable` can be declared as a separate class, injected as an anonymous class or injected as a Lambda function. Anonymous classes and Lambda functions can be convenient, but unless they are extremely simple and obvious, I prefer a separate class so that the `Runnable` class can be unit tested.

The [Thread](https://docs.oracle.com/javase/8/docs/api/java/lang/Thread.html) class provides the framework within which to execute a `Runnable` in the JVM. `Thread` provides some control operations, but for the most part, `Runnable`’s execution within a Thread is encapsulated.
Java has extended Thread management via these Executor interfaces:
* [Executor](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executor.html) – Executes a Runnable command
* [ExecutorService](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ExecutorService.html) – Extends service management to Executor
* [ScheduledExecutorService](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ScheduledExecutorService.html) – Extends scheduling management to ExecutorService

The [Executors](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/Executors.html) class provides factory methods to acquire various ExecutorServices.
Baeldung provides a brief tutoral for ExecutorServices: [A Guide to the Java ExecutorService](https://www.baeldung.com/java-executor-service-tutorial).

`Runnable`, `Thread` and `ExecutorService` can be a little daunting, but with knowledge of the Command and Command Processor Design Patterns they may be a little less daunting.

# Summary
The Command Design Pattern objectifies a function. It allows us to decouple what a function does from who decides what is done and when it is done. It supports a more modular design. And the idea of a function as an object will be extended in more design patterns to come.

# Additional References
I haven’t provided any detailed diagrams or implementations. There are many online resources with diagrams and implementations in different programming languages.
Here are some free resources:
* [Wikipedia Command Design Pattern](https://en.wikipedia.org/wiki/Command_pattern)
* [Source Making Command Design Pattern](https://sourcemaking.com/design_patterns/command)
* [Refactoring Guru Command Design pattern](https://refactoring.guru/design-patterns/command)
* The Evolution of Command Pattern [Part I](https://www.lvguowei.me/post/the-evolution-of-command-pattern/) and [Part II](https://www.lvguowei.me/post/the-evolution-of-command-pattern-2/)
* [The Command Processor Design Pattern](https://www.dre.vanderbilt.edu/~schmidt/cs282/PDFs/CommandProcessor.pdf)
* and for more Google "Command Design Pattern"

Here are some resources that can be purchased or are included in a subscription service:
* [Gang of Four Command Design Pattern](https://learning.oreilly.com/library/view/design-patterns-elements/0201633612/ch05.html#page_233)
* Agile Principles, Patterns, and Practices in C#, Chapter 21, ([O'Reilly](https://learning.oreilly.com/library/view/agile-principles-patterns/0131857258/) and [Amazon](https://www.amazon.com/Agile-Principles-Patterns-Practices-C/dp/0131857258))
* Clean Code: Design Patterns, Episode 25 video ([Clean Coders](https://cleancoders.com/episode/clean-code-episode-25) and [O'Reilly](https://learning.oreilly.com/videos/clean-code-fundamentals/9780134661742/9780134661742-code_03_25_00/))
* Head First Design Patterns ([O'Reilly](https://learning.oreilly.com/library/view/head-first-design/9781492077992/ch06.html#home_automation_or_bust) and [Amazon](https://www.amazon.com/Head-First-Design-Patterns-Object-Oriented-ebook/dp/B08P3X99QP))
