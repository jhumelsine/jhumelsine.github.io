---
title: Getting the Right Abstraction is Hard
description: Sometimes you don’t know the right abstraction until you see a few examples.
---
![Abstraction](https://qph.cf2.quoracdn.net/main-qimg-a5f531f31c67bb181a343b318a34e943-lq)
# Abstraction
_The purpose of abstraction is not to be vague, but to create a new semantic level in which one can be absolutely precise._ - Edsger W. Dijkstra

Abstraction is a reoccurring theme in many software design patterns. But we must balance this against:
* _All problems in computer science can be solved with another layer of abstraction, except of course for the problem of too many layers of abstraction._ This is often listed with _indirection_ rather than _abstraction_ but it's the same theme. It's been attributed to several people.
* _Premature abstraction is the root of all evil._ I can't find a specific source, but it's a derivation of Knuth's _Premature Optimization_ quote.

**The only thing worse than no abstraction is the wrong abstraction. Getting the right abstraction can be tricky.**

# Fiber Optic Switch
I worked at a start up in the early 2000s. We built a fiber optic switch. It was a beast of a machine. It had the capacity for over 500 circuit packs. It consisted of several bays each about 6 feet (about 2 meters) tall and in total it was about 10 to 12 feet (about 3 to 4 meters) wide. It looked a bit like a set of bookshelves in a legal library, except instead of law books, the shelves held circuit packs. The image below from [Northern Ohio University - Pettit College of Law](https://law.onu.edu/academics) is about the same size as our switch.

<img src="https://law.onu.edu/sites/default/files/2022-09/law04.jpg" alt="Law Library" width = "60%" style="padding-right: 20px;">

 
# Circuit Packs
I was on the team that managed circuit pack (CP) higher level features, such as their state machines. There were several different types of CPs. The ones that switched the signals were called Transceivers (TRs). They came in three different sizes to handle different [Optical Carrier](https://en.wikipedia.org/wiki/Optical_Carrier_transmission_rates) (OC) transmission rates for OC-48, OC-96 and OC-192 rates. The CPs sizes were proportionally larger as they handled larger transmission rates.

<img src="/assets/CircuitPack.webp" alt="CircuitPack" align="center" width = "50%" style="padding-right: 20px;">

I took over ownership of some code when the original developer moved to another department. He had designed a `CircuitBack` class with extended classes for `TR48`, `TR96` and `TR192`.

<img src="/assets/CircuitPack.png" alt="CircuitPack UML Class Diagram" align="center" width = "40%" style="padding-right: 20px;">

# Time to Think
<img src="https://i.stack.imgur.com/G4tJM.jpg" alt="Green Bar Printer Paper" align="right" width = "35%" style="padding-right: 20px;">

This was my code now, and I was responsible. This was before "Agile" practices had swept the software engineering industry. The Agile Industrial Complex had not yet squeezed time to think out of our schedules for their ceremonies.

I printed the code on glorious printer paper sheets the size of placemats, and I hunkered down to think. I spread the printouts out on my large cubical workspace, we had reasonable desks then too, and I scribbled notes in the margins using ink and highlighter pens.

# This Doesn’t Seem Right
`CircuitPack` had several methods with the equivalent of this:
```java
class CircuitPack {
    
   public final void doCircuitPackProcess() {
      ...
      if (this instanceof TR48) {
         doTR48Process();
      } else if (this instanceof TR96) {
         doTR96Process();
      } else {
         doTR192Process();
      }
      ...
   }
}
```

I was still figuring out Object-Oriented principles myself, and this was well before I knew any of the Gang of Four Design Patterns. But I knew that this implementation had some issues:
* I generally don’t like down casting, especially casting like this. It feels like a code smell or design flaw to me. While `instanceof` isn’t technically down casting, they hang around in the same neighborhood.
* The `CircuitPack` base class had knowledge of its derived classes.
* Behavior for specific TRs was in the `CircuitPack` base class.
* What happens when we added a fourth TR, such as `TR768`?
    * `doTR768Process()` would have to be added to `CircuitPack`.
    * Each cascading if/else-if/else statement would have to be updated. 
    * Miss one, and there would be no compile error or warning. The `doTR192Process()` would be executed for the `TR768` case by default, and we'd be lucky if we found an obvious runtime issue during QA.

I changed the above `instanceof` code in `CircuitPack` to look more like this:
```java
abstract class CircuitPack {
    
   public final void doCircuitPackProcess() {
      ...
      doProcess();
      ...
   }
 
   protected abstract void doProcess();
}
```

Each TR class, which extended `CircuitPack`, had a new compile error until `doProcess()` was implemented. I was pushing TR specific implementation details into the concrete TR classes where they belonged. And the `CircuitPack` base class could still access them through polymorphism. The cascading if/else-if/else statements were gone.

The updated `TR48` class would look something like this:
```java
class TR48 extends CircuitPack {
   ...
   protected void doProcess() {
      doTR48Process();
   }
   ...
}
```

I made similar changes to the `TR96` and `TR192` class implementations as well. I searched for all places in `CircuitPack` where `instanceof` was being used in a similar way, and I made the same modifications.

# Don’t Repeat Yourself
I examined the TR class implementations. I found duplicate code across the TR classes.

Some of the methods were identical in all three TR classes. I suspect that the original developer started with the `TR48` implementation. When he found that he needed the same behavior in the `TR96` and then later the `TR192`, he used copy-and-paste to get it working.

This was a [Don’t Repeat Yourself](https://en.wikipedia.org/wiki/Copy-and-paste_programming) code smell – and once more this was before I knew the term. I pulled the identical method up into the `CircuitPack` as one method.

I was [refactoring](https://refactoring.guru/refactoring) code before I knew what refactoring was. I was changing structure without changing behavior. I also didn’t have any unit tests, so I was flying without a net! 

# Close But No Cigar
The TR classes still had similar methods, but these methods had internal differences. They weren’t character-for-character identical copies of one another. I couldn’t just pull the method up to `CircuitPack` to remove the duplication.

`TR48` had a field attribute `Component` while the other TR classes had a collection of `Component`s such as `Collection<Component>`. Other than one instance versus a collection of instances, the `Component` methods were similar across all three TR classes.

Here's an example of the distinction:
```java
class TR48 extends CircuitPack {
   private Component component;
   ...
   public void process() {
      ...
      component.doThis();
      ...
   }
   ...
}
 
class TR96 extends CircuitPack { // Duplicated in TR192 too
   private Collection<Component> components;
   ...
   public void process() {
      ...
      for (Component component : components)
         component.doThis();
      ...
   }
   ...
}
```

I changed `TR48` to be a collection of `Component`s as well even if that collection would only ever contain one instance. Code that used the collection would be a loop, even if it looped only once.

Now all three TR classes had the same implementation, and they were pulled up as one method into `CircuitPack` as well:
```java
abstract class CircuitPack {
   private Collection<Component> components;
   ...
   public void process() {
      ...
      for (Component component : components)
         component.doThis();
      ...
   }
   ...
}
```
 
# But Wait, There’s More
I was still left with several methods that were very similar across the TR classes. The differences were a few lines of TR specific code within otherwise identical methods across the TR classes. For example:
```java
class TR48 implements CircuitPack {
   ...
   public void process() {
      doA();
      doTR48();
      doB();
   ...
}
 
class TR96 implements CircuitPack {
   ...
   public void process() {
      doA();
      doTR96();
      doB();
   ...
}
 
class TR192 implements CircuitPack {
   ...
   public void process() {
      doA();
      doTR192();
      doB();
   ...
}
```

I used the same technique that I had used in the `instanceof` refactoring. But instead of pushing TR details down from the `CircuitPack` into each TR class, I pulled the common code from the TR classes up into the `CircuitPack` and left the TR details in the TR classes where they belonged.
```java
abstract class CircuitPack {
   ...
   public void process() {
      doA();
      doTR();
      doB();
   ...
   abstract protected void doTR();
}
 
class TR48 implements CircuitPack {
   ...
   protected void doTR() {
      doTR48();
   }
   ...
}
 
class TR96 implements CircuitPack {
   ...
   protected void doTR() {
      doTR96();
   }
   ...
}
 
class TR192 implements CircuitPack {
   ...
   protected void doTR() {
      doTR192();
   }
   ...
}
```

This technique is the Template Method Design Pattern. I had applied it a few years before I had learned the design patterns. I’ll provide a more formal description of the Template Method design pattern in my next blog entry.

# Refactored Code
Once I had refactored the code, it made much more sense. The `CircuitPack` class made sense as an abstraction. Most of the implementation was in the `CircuitPack`, which made sense since most of the behavior was the same regardless of OC rates. The differences were mostly minor, and they resided in the specific TR classes to support the `CircuitPack`. The separation of concerns between the `CircuitPack` and TR abstraction layers became more obvious.

I also felt confident that if a `TR768` were ever developed, all I would have needed to do would be to derive a `TR768` class from `CircuitPack` and provide the implementations for the abstract protected methods, which would be easily identified as a compile time errors until implemented. This would still involve work, but it would be isolated to the new `TR768` class. There would probably be no need to modify the other classes as well.

# Hindsight is 20/20
I do not blame the developer who originally created these classes. He was under tremendous pressure to deliver anything within extremely tight schedules. I suspect he started with just the `TR48`, and he later determined that a `CircuitPack` base class would be needed. He took his best guess as to what belonged in the `CircuitPack` base class and what belonged in the derived `TR48` class. When he added the `TR98` and `TR196` he may have realized that he had problems, and he did the best he could to get it working in his current design given his time constraints.

I had the luxury of being able to take a little time and think about the design. I also had the luxury of seeing three completed TR classes. By this point the layers of abstraction had emerged, even if they were in the wrong places.

It's difficult to get abstraction correct with just one class, such as having only the `TR48` as one example. Don't try to pull common code into a base class prematurely. Once you get a second similar class, such as the `TR96`, the common abstractions will emerge, and you can pull them up into a common base class. It might take 3 to 5 concrete classes until all the abstractions emerge and find their natural layer.

This is the [Rule of Three](https://en.wikipedia.org/wiki/Rule_of_three_(computer_programming)). Wait for three examples before looking for abstractions to refactor. Three seems like a sweet spot. One or two examples may not be enough. More than four or five and you’ll have a lot of refactoring work to do. Three is just enough to see the abstractions but not quite so much that the refactoring is too daunting.
