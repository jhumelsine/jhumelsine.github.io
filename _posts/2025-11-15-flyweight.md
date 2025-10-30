---
title: WORK IN PROGRESS – Flyweight/Multiton Design Pattern
description: TBD
unlisted: true
---

<img src="https://live.staticflickr.com/7295/14000278151_141357835b_b.jpg" alt="Flyweight Boxers" title="Image Source: https://www.flickr.com/photos/worldseriesboxing/14000278151/" width = "75%" style="padding-right: 20px;">

# Introduction
The Gang of Four (GoF) categorize __Flyweight__ as a [Structural Pattern](http://refactoring.guru/design-patterns/structural-patterns), whereas I view it as a [Creational Pattern](https://refactoring.guru/design-patterns/creational-patterns), which is why I've included it in my [Creational Pattern Series](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html).

Flyweight allows the client to create multiple instances of a Singleton class. I know this sounds like an oxymoron, but hang in there for a bit. Singleton only allows one instance for a class. Flyweight allow multiple attribute specified instances of a class, but with the constraint that when there can only be one instance for each set of unique attributes.

Though not included by the GoF, there is a creational pattern for this behavior: __Multiton__, which is a portmanteau of _Multiple_ and _Singleton_. Multiton describes __what__ it does. Flyweight describes __how__ it does it. I view them as two patterns for the same concept approached from two different directions: __What__ vs __How__. As I alluded to in my opening sentence, I think the GoF should have focused upon its creational aspects and possibly Multiton would have been a better name. But since Flyweight is the name they chose, I'll stick with it for the rest of this blog.

I view Flyweight is an extension of the [Singleton Design Pattern](https://jhumelsine.github.io/2025/10/31/singleton.html), or maybe it might be more accurate to view Sington as a special case of Flyweight. While I'm not a fan of Singleton, I do tend to appreciate Flyweight, even if it's a more general case of Singleton.

Regardless of its categorization, I've not much liked the name Flyweight either. I think it's based upon [weight class](https://en.wikipedia.org/wiki/Weight_class_(boxing)) in boxing, where [flyweight](https://en.wikipedia.org/wiki/Flyweight) is the lightest weight class. The pattern focuses upon keeping memory acquisition lean and clean.

# DVR Example
The _Multiple Singleton_ description can be confusing. Let me try to describe it with an example.

The harddrive for my DVR is stored in the cloud. I suspected this since response with the remote is horrendous and I suspected network latency. However, it was confirmed when we refinished the hardwood floors in our house this summer. The DVR was disconnected for several days during the work, and when I reattached it, one of my scheduled programs had been recorded when the DVR had been disconnected and unplugged.

What does __my__ harddrive look like in the cloud? Its 1080 Full HD 100 hour capacity requires somewhere between 500GB and 1TB. Does my cable company need to retain this much harddrive memory for each customer? Probably not.

The most viewed television program in the United States each year is the Superbowl. Many viewers will set their DVR to record the game even if only to rewatch the halftime show. The cable company doesn't need an individual for each customer. They only need one recording, which all customers can share. When the last customer deletes the game, then the program can be removed from their records. I suspect that there would be additional recordings for faster distribution, redundency, etc., but I'm going to ignore them for now.

This applies for all programs, not just popular ones. The cable company only needs to persist one instance per program regardless of how many customers have recorded it. Each instance would maintain the program's name, length among other attributes. Each customer would have their own program instance to, which would maintain their playback location in the program and possibly playback speed, closed-captions, etc., but these may be attributes associated with the customer and not the individual program.

Each program is a type of _multiple singleton_. The cable company only needs to persist one copy of a program, but there may be many individual programs that they have to persist. The primary attribute that distinguishes the programs would be the program's name.

# GoF Flyweight Design

Flyweight is very similar to [Singleton](https://jhumelsine.github.io/2025/10/31/singleton.html) except that instead of one `singleton` static instance, there is a static Map of instances.

This diagram represents the basics of the GoF's Flyweight design. In this design `flyweightA1` and `flyweightA2` will contain references to the same instance. `flyweightB` will be a separate instance.

<img src="/assets/Flyweight1.png" alt="GoF Flyweight Design" width = "80%" align="center" style="padding-right: 20px;">

# My Flyweight Design

Here is my variation on the GoF Flyweight pattern. I've combined the classes into one class so that this design is closer to the Singleton design.

Both are sufficient. Using one of the other is a matter of personal style.

<img src="/assets/Flyweight2.png" alt="My Flyweight Design" width = "80%" align="center" style="padding-right: 20px;">

# Issues
Singleton had several issues mostly with concurrency and memory leaks. Flyweight has the same issues, but possibly to an even greater degree.

## Concurrency
[Singleton's concurrency issue](https://jhumelsine.github.io/2025/10/31/singleton.html#singleton-implementation) is infrequent, since it is mostly only a concern when the `singleton` instance is first being initialized. Flyweight is different. A new instance can be instantated at any time when a new key value is presented.

Any concurrency mechanism that addresses this will be sufficient. Here is the thread safe update to the code snippets above using `synchronized` within Java.

```java
private static Map<String, Flyweight> flyweights = new HashMap<>();

public static synchronized int acquire(String key) {
    if (!flyweights.containsKey(key)) {
        flyweights.put(key, new Flyweight(key));
    }
    return flyweights.get(key);
}
```

## Memory Leaks
[Singleton's Memory Leak issue](https://jhumelsine.github.io/2025/10/31/singleton.html#memory-leaks) is minor. However, it could become a major issue with Flyweight, since there is an unlimited number of keys that could be requested. This sort of ironic, since the point of Flyweight is to reduce memory consumption.

The GoF don't really address the concern. If anything, they state that there is no concern. Here's exactly what they state about memory reclaimation:
>Sharability also implies some form of reference counting or garbage collection to reclaim a flyweight’s storage when it’s no longer needed. However, neither is necessary if the number of flyweights is fixed and small (e.g., flyweights for the ASCII character set). In that case, the flyweights are worth keeping around permanently.

Unless you know for sure that you'll never have a large number of flyweight instances, I'd prefer some sort of flyweight clean up. Instances can be released when there are no more references to them. For some languages, such as C++, this responsibility is mostly upon the developer. For other languages, such as Java, the environment will manage this and perform garbage collection when needed.

The GoF didn't provide any suggestions, as noted above. Java has a nice way to address this with its [WeakHashMap](https://docs.oracle.com/javase/8/docs/api/java/util/WeakHashMap.html) class. A __WeakHashMap__ doesn't include the map in a stored instance's reference count. If the weak map is the only reference to an instance, then it is eligible for garbage collection.

```java
private static Map<String, Flyweight> flyweights = new WeakHashMap<>();

public static int acquire(String key) {
    if (!flyweights.containsKey(key)) {
        flyweights.put(key, new Flyweight(key));
    }
    return flyweights.get(key);
}
```

## Combining Concurrency and Memory Leak Solutions
Here's a Java implementation that's ensures that the implementation is thread safe and releases memory by adding a [Collections.synchronizedMap](https://docs.oracle.com/javase/8/docs/api/java/util/Collections.html#synchronizedMap-java.util.Map-):
```java
private static Map<String, Flyweight> flyweights = Collections.synchronizedMap(new WeakHashMap<>());

public static int acquire(String key) {
    if (!flyweights.containsKey(key)) {
        flyweights.put(key, new Flyweight(key));
    }
    return flyweights.get(key);
}
```

### Singleton Memory Leak
I ended the [Singleton Memory Leaks](https://jhumelsine.github.io/2025/10/31/singleton.html#memory-leaks) section with:
>There is a way to address this at least in Java; however, I won’t present it until the next blog entry, which will feature the Flyweight Design Pattern.

To the best of my knowledge there is no ___Weak___ static variable concept in Java, but we can adjust the previous implementation so that the Map contains only one mapping:
```java
private static Map<String, Singleton> singleton = Collections.synchronizedMap(new WeakHashMap<>());

public static int acquire() {
    if (!singleton.containsKey("singleton")) {
        singleton.put("singleton", new Singleton());
    }
    return singleton.get("singleton");
}
```

## Shared State

# DVR Details

## My DVR Design

## My DVR Implementation

# Summary
TBD

# References
* [Wikipedia Flyweight Design Pattern](https://en.wikipedia.org/wiki/Flyweight_pattern)
* [Source Making Flyweight Design Pattern](https://sourcemaking.com/design_patterns/flyweight) - I'm including this resource, since I always include this resource; however, their diagram doesn't feel right to me.
* [Refactoring Guru Flyweight Design Pattern](https://refactoring.guru/design-patterns/flyweight)
* [Flyweight Pattern in Java](https://www.baeldung.com/java-flyweight) - By Baeldung
* [Flyweight Design Pattern](https://www.patterns.dev/vanilla/flyweight-pattern/) - by Patterns.dev
* [Flyweight Design Pattern in Java](https://www.digitalocean.com/community/tutorials/flyweight-design-pattern-java) - by DigitalOcean
* [Understanding the Flyweight Design Pattern](https://www.carloscaballero.io/understanding-the-flyweight-design-pattern/) - Blog by Carlos Caballero
* [Flyweight Design Pattern and Immutability: A Perfect Match](https://www.sitepoint.com/flyweight-design-pattern-immutability-perfect-match/) - Blog by Andrew Carter
* [Flyweight](https://www.cs.unc.edu/~stotts/GOF/hires/pat4ffso.htm) - University of North Carolina Computer Science Department. Looks like a direct copy from the GoF book.
* [Flyweight Design Pattern](https://www.youtube.com/watch?v=0vV-R2926ss) - Video by Derek Banas
* [Flyweight Pattern - DESIGN PATTERNS (C#/.NET)](https://www.youtube.com/watch?v=RDV0ioVTF4g) - Video by SingletonSean
* [Wikipedia Multiton Design Pattern](https://en.wikipedia.org/wiki/Multiton_pattern)
* [Design Pattern Series: Singleton and Multiton Pattern](https://www.codeproject.com/articles/Singleton-and-Multiton-Pattern#comments-section) - Blog by CodeProject
* [Multiton Pattern in Java: Mastering Advanced Singleton Variants](https://java-design-patterns.com/patterns/multiton/) - Blog by Java Pattern Designs
* and for more, Google: [Flyweight Design Pattern](https://www.google.com/search?q=flyweight+design+pattern) and [Multiton Design Pattern](https://www.google.com/search?q=multiton+design+pattern)
