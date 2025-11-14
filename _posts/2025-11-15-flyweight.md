---
title: DRAFT OPEN FOR COMMENTS – Flyweight/Multiton Design Pattern
description: When you thought you only wanted a Singleton, but you just couldn't stop at one
unlisted: true
---

<img src="https://live.staticflickr.com/7295/14000278151_141357835b_b.jpg" alt="Flyweight Boxers" title="Image Source: https://www.flickr.com/photos/worldseriesboxing/14000278151/" width = "50%" style="padding-right: 20px;">

# Introduction
The Gang of Four (GoF) classify __Flyweight__ as a [Structural Pattern](http://refactoring.guru/design-patterns/structural-patterns), whereas I view it as a [Creational Pattern](https://refactoring.guru/design-patterns/creational-patterns), which is why I've included it in my [Creational Pattern Series](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html). Hopefully my reclassification won't cause too much confusion, since other sources classify and list Flyweight with the structural patterns.

Flyweight allows the client to create multiple instances of a Singleton-like class. I know that multiple instances of a Singleton sounds like an oxymoron, but hang in there for a bit. While Singleton only allows one instance for a class, Flyweight allows multiple attribute-keyed instances of a class, but with the constraint that there can only be one instance for each set of unique attributes.

I view Flyweight as an extension of the [Singleton Design Pattern](https://jhumelsine.github.io/2025/10/31/singleton.html), or maybe it might be more accurate to view Singleton as a special case of Flyweight. While I'm not a fan of Singleton, I do tend to appreciate Flyweight.

Though not included by the GoF in the catalog, there is a creational pattern that captures this behavior: __Multiton__, a portmanteau of _Multiple_ and _Singleton_. Multiton describes __what__ it does. Flyweight describes __how__ it does it. I view them as two patterns for the same concept with two different perspectives: __What__ vs __How__. Multiton explains what is produced: one instance per key. Flyweight explains how to ensure it remains lightweight and shared.

As I alluded to in my opening sentence, I think the GoF should have focused upon the creational aspect of this behavior and included it in the creational section as Multiton rather than their Flyweight structural classification. But since Flyweight is the name they chose, I'll stick with it for the rest of this blog.

Regardless of its categorization, I've not much liked the name _Flyweight_. I think its inspiration is based upon a [weight class](https://en.wikipedia.org/wiki/Weight_class_(boxing)) in boxing, where [flyweight](https://en.wikipedia.org/wiki/Flyweight) is the lightest weight class. I think this name was chosen because the pattern focuses upon keeping memory acquisition lean and clean. Even if this is the origin of the name, I still don't feel that a boxing metaphor is sufficiently descriptive.

# DVR Example
Since the _Multiple Singleton_ concept can be confusing, let me try to describe it with an example.

The hard drive for my DVR is stored in the cloud. I had long suspected this since response with the remote is horrendously slow. I had suspected network latency was the cause of its sluggishness. Its cloud nature was confirmed when we refinished the hardwood floors in our house this summer. The DVR was disconnected for several days while the floors were being sanded and refinished, and when I reconnected the DVR, one of my scheduled programs had been recorded while the DVR had been unplugged and disconnected.

What does __my__ hard drive look like in the cloud? Its 1080-Full-HD 100 hour capacity requires somewhere between 500GB and 1TB. Does my cable company need to retain this much hard drive storage for each customer? Probably not. Cloud space for programs could be shared.

The most viewed television program in the United States each year is the [Super Bowl](https://en.wikipedia.org/wiki/Super_Bowl). Many viewers will set their DVR to record the game even if only to rewatch the [halftime show](https://en.wikipedia.org/wiki/List_of_Super_Bowl_halftime_shows). The cable company doesn't need to retain an individual recording of the Super Bowl in the cloud for each customer. They only need one recording, which all customers can share. When the last customer deletes the program from their line up, then the program can be removed from the cloud. I'm sure they have additional recordings for redundancy and performance, but those needs are beyond the scope of Flyweight.

A single instance applies for all programs, not just popular ones. The cable company only needs to persist one instance per program regardless of how many customers have recorded it. Each instance would maintain the program's name and runtime length among other attributes. Each customer would have a list of their recorded programs, which would maintain local information, such as the playback location in a program for each customer.

Each program stored in the cloud is a type of _multiple singleton_. The cable company only needs to persist one copy of a program, but there may be many persisted programs. The key attribute that distinguishes the programs would be the program's name.

The key idea: customers share the same underlying recording instance; only per-user playback state is unique.

Although this example focuses on DVR recordings, Flyweight is widely used anywhere many objects share a large portion of their state. In graphical user interfaces, for instance, thousands of characters on a screen may share the same underlying font glyphs or rendering data, dramatically reducing memory requirements. Web browsers and document editors rely heavily on this concept to keep performance responsive.

Game engines provide another classic application. In gaming, extrinsic state (position, velocity, orientation, behavior scripts) tends to vary wildly, while intrinsic state (meshes, textures, animations) is expensive and shared. Projectiles, particles, trees, and [Non-Playing Characters](https://en.wikipedia.org/wiki/Non-player_character) (NPCs) often reuse shared meshes, textures, animations, or AI states, while only position and behavior differ per instance. Instead of thousands of identical copies consuming system resources, Flyweight allows the engine to maintain a single authoritative version of each asset and apply extrinsic state as needed.

The principle remains the same: when shared identity outweighs individual uniqueness, Flyweight can make high-scale systems feasible.

# GoF Flyweight Design

Flyweight is similar to [Singleton](https://jhumelsine.github.io/2025/10/31/singleton.html) except that instead of one `singleton` static instance, there is a static Map of key attributes(s) to instances.

This diagram represents the basics of the GoF's Flyweight design. In this design `flyweightA1` and `flyweightA2` will contain references to the same instance. `flyweightB` will be a separate instance.

<img src="/assets/Flyweight1.png" alt="GoF Flyweight Design" width = "80%" align="center" style="padding-right: 20px;">

# The Singleton extension for Flyweight Design

Here is a Flyweight pattern that's more akin to being an extension of Singleton. It's basically Singleton where the `static singleton` instance is replaced by a `static Map<Key, Flyweight>`.

<img src="/assets/Flyweight2.png" alt="Singleton Extension For Flyweight Design" width = "80%" align="center" style="padding-right: 20px;">

Both designs are sufficient. Using one instead of the other is a matter of personal style.

# Issues
Singleton had several issues mostly with concurrency, memory leaks and shared state. Flyweight has the same issues and possibly to an even greater degree.

## Concurrency
[Singleton's concurrency issue](https://jhumelsine.github.io/2025/10/31/singleton.html#singleton-implementation) is infrequent, since it is mostly only a concern when the `singleton` instance is first being initialized. Flyweight is different. A new instance can be instantated any time when a new key is requested.

Any concurrency mechanism that addresses this will be sufficient. Here is the thread safe update to the code snippets above using `synchronized` within Java.

```java
private static Map<String, Flyweight> flyweights = new HashMap<>();

public static synchronized Flyweight acquire(String key) {
    if (!flyweights.containsKey(key)) {
        flyweights.put(key, new Flyweight(key));
    }
    return flyweights.get(key);
}
```

Using synchronized works but can bottleneck under heavy access. A `ConcurrentHashMap.computeIfAbsent` or `compute` version, as shown later, provides better scalability.

## Memory Leaks
[Singleton's Memory Leak issue](https://jhumelsine.github.io/2025/10/31/singleton.html#memory-leaks) is minor. However, it could become a major issue with Flyweight, since there is an unlimited number of keys that could be requested. This is sort of ironic, since the point of Flyweight is to reduce memory consumption.

The GoF don't really address the concern. If anything, they state that there is no concern. Here's what they state about memory reclamation:
>Sharability also implies some form of reference counting or garbage collection to reclaim a flyweight’s storage when it’s no longer needed. However, neither is necessary if the number of flyweights is fixed and small (e.g., flyweights for the ASCII character set). In that case, the flyweights are worth keeping around permanently.

Unless you know for sure that the number of flyweight instances are fixed and small, I'd prefer some sort of flyweight clean up. Instances can be released when there are no more references to them. For some languages, such as C++, this responsibility is mostly upon the developer. For other languages, such as Java, the environment will manage this and perform garbage collection when needed.

The GoF didn't provide any Flyweight reclamation suggestions, since they didn't feel they were necessary as noted above. Even with garbage collection, we have an issue, since the Flyweight itself is a reference which would prevent garbage collection.

Java has a nice way to address this with its [WeakHashMap](https://docs.oracle.com/javase/8/docs/api/java/util/WeakHashMap.html) class. A `WeakHashMap` doesn't include references stored in the Map against the reference count. If the only reference remaining is the one from the Weak Map, then the instance is eligible for garbage collection.

```java
private static Map<String, Flyweight> flyweights = new WeakHashMap<>();

public static Flyweight acquire(String key) {
    if (!flyweights.containsKey(key)) {
        flyweights.put(key, new Flyweight(key));
    }
    return flyweights.get(key);
}
```

 __NOTE:__ A __Weak__ reference only makes the unreferenced flyweight element eligible for garbage collection. It does not ensure it. If garbage collection is not performed, then the flyweight element will be retained. This may not necessarily be bad. Retaining an unreferenced element until collected creates a window in which another client has the opportunity to acquire it while it's still instantated.
 
## Combining Concurrency and Memory Leak Solutions
Let's combine concurrency and memory leak solutions. Here's a Java implementation that's ensures that the implementation is thread safe and releases memory by adding a [WeakReference](https://docs.oracle.com/javase/8/docs/api/java/lang/ref/WeakReference.html).

__Full Disclosure:__ I have never implemented a `WeakReference`. ChatGPT generated the following code when I asked it to review my previous version, which had a few issues:

```java
private static final Map<String, WeakReference<Recording>> recordings = new ConcurrentHashMap<>();

public static Recording acquire(String name) {
    // Atomically compute a WeakReference<Recording> for this name
    WeakReference<Recording> ref =
        recordings.compute(name, (key, existingRef) -> {
            Recording recording = (existingRef == null ? null : existingRef.get());

            if (recording == null) {
                // Either first time or the previous Recording was GC’d
                recording = new Recording(key);
                return new WeakReference<>(recording);
            }

            return existingRef; // recording still alive
        });

    // Safe because compute(...) always returns a non-null WeakReference
    Recording result = ref.get();

    // Cleanup if the value was collected
    if (result == null) {
        recordings.remove(name, ref); // only removes if still mapped to this ref
        return acquire(name); // retry
    }

    return result;
}
```

Because `ConcurrentHashMap.compute` locks only the bucket for that key, multiple independent acquisitions can run concurrently without blocking each other.

### Singleton Memory Leak
I ended the [Singleton Memory Leaks](https://jhumelsine.github.io/2025/10/31/singleton.html#memory-leaks) section with:
>There is a way to address [singleton memory leaks] at least in Java; however, I won’t present it until the next blog entry, which will feature the Flyweight Design Pattern.

__Full Disclosure:__ As with the previous code, I asked ChatGPT to generate some code from scratch with the following prompt:
>Create a Java implementation for Singleton using a Weak Reference. Make sure that it's thread safe as well.

Here's what it produced:
```java
public final class MySingleton {
    // Volatile to ensure visibility across threads for double-checked locking.
    private static volatile WeakReference<MySingleton> instanceRef = new WeakReference<>(null);

    // Private constructor to prevent external instantiation.
    private MySingleton() {
        // heavy initialization here (if needed)
    }

    public static MySingleton getInstance() {
        MySingleton instance = instanceRef.get();
        if (instance == null) {
            synchronized (MySingleton.class) {
                instance = instanceRef.get();
                if (instance == null) {
                    instance = new MySingleton();
                    instanceRef = new WeakReference<>(instance);
                }
            }
        }
        return instance;
    }

    // Example method
    public void doWork() {
        System.out.println("Singleton instance: " + this);
    }
}
```

## Internal State
Flyweight has the same internal state concerns that Singleton does. The more formal terms for state are:
* ___Intrinsic state___ which is an object’s state that can be shared across many instances
* ___Extrinsic state___ which is context-specific, per-instance data passed in from outside

As I described in [Singleton Internal State](https://jhumelsine.github.io/2025/10/31/singleton.html#internal-state), we want to make sure that we take advantage of shared intrinsic state without allowing specific extrinsic state to slip within the intrinsic state realm.

**Rule of thumb:** Intrinsic state never changes once created; extrinsic state always belongs to whoever is using the singleton or the flyweight.

Flyweight can also leverage the same wrapper [Singleton State Injection](https://jhumelsine.github.io/2025/10/31/singleton.html#state-injection) solution; therefore, I won't repeat it here, but I will feature it in the [DVR Use Case](#dvr-use-case).

# DVR Use Case
I'll return to the [DVR Example](#dvr-example) to demonstrate a Flyweight design and implementation. This use case will be bare bones. I'll provide enough to demonstrate:
* A Flyweight implementation
* A wrapper class to store extrinsic state information that's passed to the shared Flyweight instance.

I cut a lot of corners in this design and implementation. It is by no means production quality. The classes are too tightly coupled to one another. I did not use [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html) techniques, and I haven't provided any unit tests. I _confirmed_ the code as I wrote it via manual testing. _Mea culpa. Mea culpa. Mea maxima culpa._

This is how [legacy code](https://jhumelsine.github.io/2025/03/24/legacy-code.html) accumulates, one shortcut at a time.

## DVR Design
The use case design consists of three classes:
* `DVR` - This is collection of Programs recorded by a user. The user can  retrieve a recorded Program by name.
* `Program` - This is the Program recorded by a user. Program mostly manages program state, which in this design is playback location within the program. Most methods allow the user to move about the program's viewing locations.
* `Recording` - This is the shared Flyweight instance for a Program. Only one Recording instance is needed regardless of how many users have recorded the program.

<img src="/assets/Flyweight3.png" alt="DVR Use Case Design" width = "100%" align="center" style="padding-right: 20px;">

## DVR Implementation
I'll present the class implementations from right to left as shown in the design.

### Recording
`Recording` demonstrates a Flyweight design and implementation.
In a real application, Recording would have more behavior, such as description, rating, warnings, etc.
It would have at least three states: Scheduled, Recording and Recorded.

None of these additional behaviors have been represented in this use case.

I wanted different values for the recording length, so I chose a bogus runtime of 15 minutes times the length of the title for some variety.

```java
class Recording {
    private static final Map<String, WeakReference<Recording>> recordings = new ConcurrentHashMap<>();

    private final String name;
    private final int runTime;
    
    public static Recording acquire(String name) {
        // Atomically compute a WeakReference<Recording> for this name
        WeakReference<Recording> ref =
            recordings.compute(name, (key, existingRef) -> {
                Recording recording = (existingRef == null ? null : existingRef.get());

                if (recording == null) {
                    // Either first time or the previous Recording was GC’d
                    recording = new Recording(key);
                    return new WeakReference<>(recording);
                }

                return existingRef; // recording still alive
            });

        // Safe because compute(...) always returns a non-null WeakReference
        Recording result = ref.get();

        // Cleanup if the value was collected
        if (result == null) {
            recordings.remove(name, ref); // only removes if still mapped to this ref
            return acquire(name); // retry
        }

        return result;
    }

    private Recording(String name) {
        this.name = name;
        this.runTime = name.length() * 15;

        System.out.format("Creating Recording name=%s, runtime=%d\n", name, runTime);
    }

    public String getRecordingName() {
        return name;
    }

    public int getRunTime() {
        return runTime;
    }

    public void playAt(int start, int duration) {
        System.out.format("Playing %s starting at minute %d for %d minute(s)\n", name, start, Math.min(duration, runTime - start));
    }
}
```

### Program
`Program` contains state, which in this example is `playbackLocation`. It keeps track of where the user has been watching the program. The most challenging part
of this implementation was making sure that `playbackLocation` was correct, especially when the `minutes` arguments could have resulted in a negative `playbackLocation` or one that was beyond the runtime of the `recording` if care was not taken.

Most behavior delegates to `recording`, which is a Flyweight entity shared by all DVR accounts that have recorded it.

```java
class Program {
    private final Recording recording;
    private int playbackLocation = 0;

    public Program(String name) {
        this.recording = Recording.acquire(name);
    }

    public String getProgramName() {
        return recording.getRecordingName();
    }

    public int getPlayTime() {
        return recording.getRunTime();
    }

    // Play resumes at the current recording location.
    public void play(int minutes) {
        recording.playAt(playbackLocation, minutes);
        playbackLocation = Math.min(playbackLocation + minutes, recording.getRunTime());
    }

    // Play restarts at the beginning.
    public void restart(int minutes) {
        playbackLocation = 0;
        play(minutes);
    }

    public int getRemainingPlayTime() {
        return recording.getRunTime() - playbackLocation;
    }

    public int getPlaybackLocation() {
        return playbackLocation;
    }

    public void fastForward(int minutes) {
        playbackLocation = Math.min(playbackLocation + minutes, recording.getRunTime());
    }

    public void reverse(int minutes) {
        playbackLocation = Math.max(playbackLocation - minutes, 0);
    }

    // Convenient means to print Program state.
    public String toString() {
        return String.format("name=%s, playTime=%d, playbackLocation=%d, remainingPlayTime=%d", getProgramName(), getPlayTime(), getPlaybackLocation(), getRemainingPlayTime());
    }
}
```

### DVR
`DVR` only provides context for the map of recorded Programs.

There is no method that returns the list of recorded programs, but if this were a real feature, I would add one.

`Program getProgram(String name)` should probably return an `Optional<Program>`, since a `Program` for the given `name` may not be on the DVR. If this were going to be a real feature, then I would have done that rather than return a `null` if the `Program` is not found.

`DVR` uses a simple `HashMap` because it’s only modeling per-user state; the shared concurrency concerns reside inside `Recording`.

```java
class DVR {
    // There's no method to return the programs map, but if this were a real feature, I'd add one.
    Map<String, Program> programs = new HashMap<>();

    public void recordProgram(String name) {
        programs.put(name, new Program(name));
    }

    public void deleteProgram(String name) {
        programs.remove(name);
    }

    // This should really return Optional<Program>, but that's not the primary concern of this demo.
    public Program getProgram(String name) {
        return programs.get(name);
    }
}
```

# Comparing Singleton, Multiton, and Flyweight Patterns

While these three patterns are closely related, each manages object uniqueness and shared state at a different scale.

| Aspect                      | **Singleton**                                   | **Multiton**                                                          | **Flyweight**                                                                                    |
| --------------------------- | ----------------------------------------------- | --------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------ |
| **Core Idea**               | Exactly one global instance                     | One instance per key                                                  | One shared instance per *intrinsic* state                                                        |
| **Lookup Structure**        | Single static reference                         | Map keyed by identifier                                               | Map keyed by intrinsic state (often composite)                                                   |
| **State Split**             | No split; all state is internal                 | No formal state split                                                 | **Intrinsic** state inside; **extrinsic** state passed in by caller                              |
| **Memory Behavior**         | Minimal memory savings; always held             | Saves memory when many keys reuse instances                           | Maximizes sharing of heavy intrinsic data; huge savings at scale                                 |
| **Thread Safety Concerns**  | Safe once instance is published                 | Must synchronize map access                                           | Must synchronize map access **and** handle extrinsic state correctly                             |
| **Identity / Uniqueness**   | One unique object                               | One per key                                                           | Many callers share the same intrinsic object; extrinsic varies per use                           |
| **Creation Logic**          | Construct on first access                       | Construct on first use of each key                                    | Construct on first use of each intrinsic state combination                                       |
| **GC Friendliness**         | Often poor (long-lived global)                  | Better; map can use WeakReferences                                    | Very good with WeakHashMap or WeakReference flyweights                                           |
| **When to Use**             | Global configuration, logging, connection pools | Keyed sets of shared instances (e.g., database connections by tenant) | Large numbers of lightweight objects sharing heavy representation (glyphs, sprites, trees, etc.) |
| **When *Not* to Use**       | When global state is harmful                    | When key explosion defeats sharing                                    | When extrinsic state dominates or identity must be unique                                        |
| **Typical Java Patterns**   | Private constructor + static instance           | Private constructor + `Map<K, T>` factory                             | Factory using `Map<IntrinsicKey, Flyweight>` with separate extrinsic parameter                   |
| **Conceptual Connection Between Patterns** | Multiton with one key                           | Superset of Singleton                                                 | Specialization of Multiton + intrinsic/extrinsic discipline                                      |


# Summary
Flyweight can be viewed as a generalization of Singleton and Multiton — enforcing “one instance per key” where the key represents intrinsic state. By separating shared and mutable attributes, we gain a memory-efficient architecture that still expresses our domain clearly.

However, this power comes with trade-offs: thread-safety becomes critical, cleanup must be intentional, and debugging shared state can be challenging. Understanding when the savings outweigh the complexity is essential.

Used wisely, Flyweight supports scalable object-rich systems in UI frameworks, game engines, streaming services, and more.

Flyweight is not a universal solution, but in domains where intrinsic state is heavy and reused, it can transform a memory-hungry design into a scalable, elegant architecture.

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

# Complete Demo Code
Here’s the entire implementation up to this point as one file. Copy and paste it into a Java environment and execute it. If you don’t have Java, try this [Online Java Environment](https://www.programiz.com/java-programming/online-compiler/). Play with the implementation.

```java
import java.util.*;
import java.util.concurrent.*;
import java.lang.ref.WeakReference;

public class FlyweightDemo {
    public static void main(String[] args) throws Exception {
        DVR dvr1 = new DVR();
        DVR dvr2 = new DVR();
        DVR dvr3 = new DVR();

        dvr1.recordProgram("Jaws");
        Program jaws1 = dvr1.getProgram("Jaws");
        System.out.println(jaws1.toString());
        jaws1.play(15);
        System.out.println(jaws1.toString());

        dvr2.recordProgram("Jaws");
        Program jaws2 = dvr2.getProgram("Jaws");
        System.out.println(jaws2.toString());
        jaws2.fastForward(30);
        jaws2.play(45);
        System.out.println(jaws2.toString());

        dvr3.recordProgram("Star Wars");
        Program starWars3 = dvr3.getProgram("Star Wars");
        System.out.println(starWars3.toString());

        dvr1.deleteProgram("Jaws");

        dvr3.recordProgram("Jaws");
    }
}

class DVR {
    // There's no method to return the programs map, but if this were a real feature, I'd add one.
    Map<String, Program> programs = new HashMap<>();

    public void recordProgram(String name) {
        programs.put(name, new Program(name));
    }

    public void deleteProgram(String name) {
        programs.remove(name);
    }

    // This should really return Optional<Program>, but that's not the primary concern of this demo.
    public Program getProgram(String name) {
        return programs.get(name);
    }
}


// Program contains user state, which in this example is playbackLocation, which keeps track of where the user has been watching the program.
// Most behavior is delegated to recording, which is a Flyweight entity shared by all DVR accounts that have recorded it.
class Program {
    private final Recording recording;
    private int playbackLocation = 0;

    public Program(String name) {
        this.recording = Recording.acquire(name);
    }

    public String getProgramName() {
        return recording.getRecordingName();
    }

    public int getPlayTime() {
        return recording.getRunTime();
    }

    // Play resumes at the current recording location.
    public void play(int minutes) {
        recording.playAt(playbackLocation, minutes);
        playbackLocation = Math.min(playbackLocation + minutes, recording.getRunTime());
    }

    // Play restarts at the beginning.
    public void restart(int minutes) {
        playbackLocation = 0;
        play(minutes);
    }

    public int getRemainingPlayTime() {
        return recording.getRunTime() - playbackLocation;
    }

    public int getPlaybackLocation() {
        return playbackLocation;
    }

    public void fastForward(int minutes) {
        playbackLocation = Math.min(playbackLocation + minutes, recording.getRunTime());
    }

    public void reverse(int minutes) {
        playbackLocation = Math.max(playbackLocation - minutes, 0);
    }

    // Convenient means to print Program state.
    public String toString() {
        return String.format("name=%s, playTime=%d, playbackLocation=%d, remainingPlayTime=%d", getProgramName(), getPlayTime(), getPlaybackLocation(), getRemainingPlayTime());
    }
}

// Recording demonstrates Flyweight.
// In a real application, Recording would have more behavior, such as description, rating, warnings, etc.
// It would have at least three states: Scheduled, Recording and Recorded.
// None of these additional behaviors have been represented in this demo.
// I wanted different values for the recording length, so I chose a bogus 15 minutes times the length of the title.
class Recording {
    private static final Map<String, WeakReference<Recording>> recordings = new ConcurrentHashMap<>();

    private final String name;
    private final int runTime;
    
    public static Recording acquire(String name) {
        // Atomically compute a WeakReference<Recording> for this name
        WeakReference<Recording> ref =
            recordings.compute(name, (key, existingRef) -> {
                Recording recording = (existingRef == null ? null : existingRef.get());

                if (recording == null) {
                    // Either first time or the previous Recording was GC’d
                    recording = new Recording(key);
                    return new WeakReference<>(recording);
                }

                return existingRef; // recording still alive
            });

        // Safe because compute(...) always returns a non-null WeakReference
        Recording result = ref.get();

        // Cleanup if the value was collected
        if (result == null) {
            recordings.remove(name, ref); // only removes if still mapped to this ref
            return acquire(name); // retry
        }

        return result;
    }

    private Recording(String name) {
        this.name = name;
        this.runTime = name.length() * 15;

        System.out.format("Creating Recording name=%s, runtime=%d\n", name, runTime);
    }

    public String getRecordingName() {
        return name;
    }

    public int getRunTime() {
        return runTime;
    }

    public void playAt(int start, int duration) {
        System.out.format("Playing %s starting at minute %d for %d minute(s)\n", name, start, Math.min(duration, runTime - start));
    }
}
```
