---
title: DRAFT – Abstract Factory Design Pattern
description: Ensuring consistency among dependencies and the ability to create multiple objects without knowing class types
unlisted: true
---

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/30/Piet_Mondrian%2C_1942_-_Broadway_Boogie_Woogie.jpg/800px-Piet_Mondrian%2C_1942_-_Broadway_Boogie_Woogie.jpg" alt="Broadway Boogie Woogie" title="Image Source: By Piet Mondrian - Transferred from en.wikipedia to Commons., Public Domain, https://commons.wikimedia.org/w/index.php?curid=37640791" width = "50%" align="center" style="padding: 35px;">

# Introduction
__TBD__

# Gang of Four’s Introduction to Abstract Factory
The Gang of Four (GoF) organized their book in four basic sections:
* The initial 80 pages in which they lay out the foundations for design patterns with an overview as well as present a use case for a little context.
* The [Creational Design Patterns](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html), which cataloged patterns useful in creating and assembling objects. They are the topic of this blog series.
* The [Structural Design Patterns](https://refactoring.guru/design-patterns/structural-patterns), which catalog the structure of objects working together in repeating patterns.
* The [Behavioral Design Patterns](), which catalog the behaviors that emerge from objects working together in repeating patterns.

Patterns were presented in alphabetical order within each category. Therefore, __Abstract Factory__ was the first design pattern presented in detail in their book. It’s a daunting first design pattern to encounter if reading the book from cover to cover.

Imagine seeing this as your first design pattern.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/9/9d/Abstract_factory_UML.svg/1016px-Abstract_factory_UML.svg.png" alt="Gang of Four Abstract Factory Design" title="Image Source: https://en.wikipedia.org/wiki/File:Design_by_contract.svg" width = "80%" align="center" style="padding: 35px;">
 
__Builder__ (reference TBD) is the second pattern cataloged in the book, and it’s just about as intimidating. It’s the next pattern in this creational blog series.

The first several times I attempted to read the GoF, I made it through the foundations and use case, but once I hit Abstract Factory and Builder, I was so flummoxed that I put the book back on the shelf. I describe my struggles with the GoF, and how I got past them in [It’s You Move](https://jhumelsine.github.io/2023/08/24/its-your-move.html).

# My Introduction to Abstract Factory
Dear Reader, I hope to be gentler with you as I present Abstract Factory than the GoF were with me.

Here is Abstract Factory in a Nutshell: __Abstract Factory is the [Strategy Design Pattern](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) where the behavior declared in the interface is a Factory method that acquires an instance to an abstract declaration.__

Unlike the GoF, Abstract Factory is not the first design pattern I have presented. I’m about halfway through their pattern catalog. I’ve covered [Design Pattern Foundations](https://jhumelsine.github.io/3000/01/01/preface.html#design-pattern-foundations), [Essential Design Patterns](https://jhumelsine.github.io/3000/01/01/preface.html#essential-design-patterns) and [Composable Design Patterns]( https://jhumelsine.github.io/3000/01/01/preface.html#composable-design-patterns). I illustrated how the Essential Design Patterns work together to create the [Hexagonal Architecture/Design](https://jhumelsine.github.io/3000/01/01/preface.html#hexagonal-architecture-aka-ports-and-adapters-design).

I have previously mentioned Abstract Factory:
* In the [Creational Design Pattern](https://jhumelsine.github.io/2025/07/18/creational-design-patterns.html) series introduction, with just a brief description.
* In the [Abstract Factory]( https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html#abstract-factory) section of the [Factory Design Patterns]( https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html) blog with another description.
* In the [Abstract Factory Design Pattern]( https://jhumelsine.github.io/2024/11/27/abstraction-cohesion.html#abstract-factory-design-pattern) section of the [What Is Cohesive Abstraction?](https://jhumelsine.github.io/2024/11/27/abstraction-cohesion.html) blog with an example.

The GoF dumped Abstract Factory, whole cloth, in its entirety in their book. I will present Abstract Factory step-by-step layering in its parts with context while building upon the content that I presented previously.

# Abstract Factory Intent
It took a long time before I understood the intent of Abstract Factory. I knew that Abstract Factory helps ensure that all dependencies for an application are consistent for a specific environment or scenario. But its importance still didn’t resonate with me.

In [A Cautionary Tale](https://jhumelsine.github.io/2024/11/27/abstraction-cohesion.html#a-cautionary-tale), I described work situation where an integration test was connected to our production database. When the test cleaned up upon completion, it deleted a table in our production database losing customer data. Had we configured our system with an Abstract Factory, it would have been much more difficult to have configured our test environment with a production dependency.

While thinking about this blog, I realized that I was missing another scenario. In most Creational Design Patterns, the caller depends upon the concrete class type of the object being created even if the class type is encapsulated and unknown to the caller. I described this situation in the beginning of the [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) (DI) blog. DI helps resolve this indirect dependency issue by creating the instance and injecting it into the application.

DI only resolves this issue when the application only depends upon one individual object. If repeated instances of the same object type are required by the application, then DI won’t work as it’s usually presented.

Abstract Factory will address both situations:
* Consistent Dependencies
* Multiple Object Instances

There may be other scenarios where it will be useful, but I won’t present them.

# Abstract Factory Use Case
I will return to the [Call of Duty](https://jhumelsine.github.io/2024/11/27/abstraction-cohesion.html#call-of-duty) example in (What Is Cohesive Abstraction?)[ https://jhumelsine.github.io/2024/11/27/abstraction-cohesion.html] with a few modifications.

This use case will feature a Warrior who uses a Launcher/Projectile weapon system, such as a Rifle/Bullet, Bazooka/Bazooka Shell or Bow/Arrow. The Warrior’s behavior consists of:
* Acquiring a Launcher.
* Acquiring a set of Projectiles and loading those Projectiles into the Launch up to the Launcher’s Projectile capacity.
* Aiming and launch each of the Projectiles loaded into the Launcher.

In more concrete terms, if the Warrior is a Rifleman, then he/she will load six bullets into his/her rifle, take aim and fire the six rounds.

I will construct the Warrior design in phases using the Abstract Factory in a test environment. Then I’ll show how it can accommodate Rifle/Bullet, Bazooka/BazookaShell and Bow/Arrow.

# Abstract Factory is Comprised of other Design Patterns
Abstract Factory is comprised of one [Design Pattern Principle](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html) and several [Essential Design Patterns](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html):

## [Program to an interface, not an implementation](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html#program-to-an-interface-not-an-implementation)
This principle applies to almost every design pattern, but it’s even more so with Abstract Factory. The Warrior has two interface dependencies: `Launcher` and `Projectile`.

<img src="/assets/AbstractFactory1.png" alt="Warrior delegates to Launcher and Projectile interfaces" width = "100%" align="center" style="padding-right: 35px;">

`Launcher` and `Projectile` need to remain consistent. For example, we can’t attempt to load an `Arrow` into a `Rifle`. We also need the ability to instantiate any number of new Projectile instances.

The Abstract Factory will resolve these dependencies, so that they are resolved in consistent pairs, they are consistent, allow for multiple instances.

## [Strategy](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html)
`Launcher` and `Projectile` are Strategies, since each will be realized via different sets of concrete classes.

However, the Strategy component of the pattern is the `LauncherSystem` Abstract Factory interface. When added to the design, we get:

<img src="/assets/AbstractFactory2.png" alt="Featuring Strategy with LauncherSystem" width = "100%" align="center" style="padding-right: 35px;">

`LauncherSystem` is a Strategy as well. It’s only an Abstract Factory in the sense that it’s a Strategy whose responsibility is to acquire other dependencies associated with the design. Once the `LaunchSystem` reference is resolved, which I haven’t shown yet, the `Warrior` can acquire a `Launcher` and `Projectiles`.

## [Factory](https://jhumelsine.github.io/2023/09/07/essential-design-patterns.html)
`LaunchSystem` is an abstract Strategy that declares the ability to acquire `Launcher` and `Projectile` virtually. Concrete instances of `LaunchSystem` realize that potential using a Factory.

Here’s the design with a `LauncherSystemTestDouble`, which creates `LauncherTestDouble` and `ProjectileTestDouble`. The dashed red line represents architecture boundaries especially to convey the flow of dependency and knowledge.

<img src="/assets/AbstractFactory3.png" alt="Adding test Doubles" width = "100%" align="center" style="padding-right: 35px;">

This is the entire Abstract Factory design. __Abstract Factory is simply a specific application of the Strategy design pattern where the behavior is acquiring object instances.__

## [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) and [Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer)

Dependency consistency resides in the concrete Factories, but we can’t enforce it. A developer could easily define a `LauncherSystem` where the `Launcher` is a Rifle and the `Projectile` is an Arrow. However, the `LauncherSystem` is not complex. The code snippet in the diagram is almost the entire implementation. Visual inspection should help ensure that the acquired `Launcher` and `Projectile` are consistent. It would not be difficult to design unit tests that confirm this as well.

As long as each `LauncherSystem` enforces dependency consistency, the rest of the design should not need to be concerned about it.

At this point, the GoF are done with the design, but I’m not We still don’t know how `Warrior` resolves the `LauncherSystem` reference. There may be several ways to resolve this, but I prefer Dependency Injection with a Configurer, which completes the design with:

<img src="/assets/AbstractFactory4.png" alt="Adding Testing Configurer" width = "100%" align="center" style="padding-right: 35px;">

`WarriorTest` is a test, but it’s also a Configurer. It creates an instance of `LauncherSystemTestDouble` and injects it into `Warrior`.

# Testing the Use Case
Here is an example of Java code for the above:
```java
interface LauncherSystem {
    Launcher acquireLauncher();

    Projectile acquireProjectile();
}

interface Launcher {
    int getProjectileCapacity();

    void load(Projectile projectile);

    void aim();

    void launch();
}

interface Projectile {
    String getType();
}

class Warrior {
    private final LauncherSystem launcherSystem;

    public Warrior(LauncherSystem launcherSystem) {
        this.launcherSystem = launcherSystem;
    }

    public void fire() {
        Launcher launcher = launcherSystem.acquireLauncher();

        for (int i = 0; i < launcher.getProjectileCapacity(); i++) {
            launcher.load(launcherSystem.acquireProjectile());
        }

        for (int i = 0; i < launcher.getProjectileCapacity(); i++) {
            launcher.aim();
            launcher.launch();
        }

    }

}

class LauncherSystemTestDouble implements LauncherSystem {
    private List<String> actions;

    public LauncherSystemTestDouble(List<String> actions) {
        this.actions = actions;
    }

    @Override
    public Launcher acquireLauncher() {
        return new LauncherTestDouble(actions);
    }

    @Override
    public Projectile acquireProjectile() {
        return new ProjectileTestDouble();
    }
}

class LauncherTestDouble implements Launcher {
    private List<String> actions;
    private Projectile projectile;

    public LauncherTestDouble(List<String> actions) {
        this.actions = actions;
    }

    @Override
    public int getProjectileCapacity() {
        return 2;
    }

    @Override
    public void load(Projectile projectile) {
        this.projectile = projectile;
        actions.add(String.format("Load: %s", projectile.getType()));
    }

    @Override
    public void aim() {
        actions.add("Aim LauncherTestDouble");
    }

    @Override
    public void launch() {
        actions.add(String.format("Launch %s", projectile.getType()));
    }
}

class ProjectileTestDouble implements Projectile {
    @Override
    public String getType() {
        return "ProjectileTestDouble";
    }
}

private static void testWarrior() throws Exception {
    // Given
    List<String> actions = new LinkedList<>();
    LauncherSystem launcherSystem = new LauncherSystemTestDouble(actions);
    Warrior warrior = new Warrior(launcherSystem);

    // When
    warrior.fire();

    // Then
    assertEquals("[Load: ProjectileTestDouble, Load: ProjectileTestDouble, Aim LauncherTestDouble, Launch ProjectileTestDouble, Aim LauncherTestDouble, Launch ProjectileTestDouble]", actions.toString());
}
```

# Additional Use Case Launcher Systems
Most of the work is in defining the interfaces, providing the test doubles and testing the `Warrior`. Adding more Launcher Systems is trivial once the foundations have been laid.

Since this pattern is about creating Launcher Systems, their implementations only contain print statements that describe what they would do rather than provide an actual implementation. In an actual application, each of these methods would have more sophisticated implementations. Print statements keep the example small, while still conveying different text-specified behaviors for each `LauncherSystem`.

Nothing in the design or implementation changes above the horizontal dashed red line. The Abstract portion of the design does not depend upon nor know about the concrete elements below it.

## Rifle/Bullet

<img src="/assets/AbstractFactory5.png" alt="Adding Rifle/Bullet Pair" width = "100%" align="center" style="padding-right: 35px;">
 
```java
class RifleWeaponSystem implements LauncherSystem {
    @Override
    public Launcher acquireLauncher() {
        return new Rifle();
    }

    @Override
    public Projectile acquireProjectile() {
        return new Bullet();
    }
}

class Rifle implements Launcher {
    private Projectile projectile;

    @Override
    public int getProjectileCapacity() {
        return 6;
    }

    @Override
    public void load(Projectile projectile) {
        this.projectile = projectile;
        System.out.format("Load %s into rifle\n", projectile.getType());
    }

    @Override
    public void aim() {
        System.out.println("Aim down rifle barrel");
    }

    @Override
    public void launch() {
        System.out.format("Squeeze rifle trigger to shoot %s\n", projectile.getType());
    } 
}

class Bullet implements Projectile {
    @Override
    public String getType() {
        return "Bullet";
    }
}

```

## Bazooka/BazookaShell
<img src="/assets/AbstractFactory6.png" alt="Adding Bazooka/Shell Pair" width = "100%" align="center" style="padding-right: 35px;">
 
```java
class BazookaWeaponSystem implements LauncherSystem {
    @Override
    public Launcher acquireLauncher() {
        return new Bazooka();
    }

    @Override
    public Projectile acquireProjectile() {
        return new BazookaShell();
    }
}

class Bazooka implements Launcher {
    private Projectile projectile;

    @Override
    public int getProjectileCapacity() {
        return 1;
    }

    @Override
    public void load(Projectile projectile) {
        this.projectile = projectile;
        System.out.format("Load %s into bazooka\n", projectile.getType());
    }

    @Override
    public void aim() {
        System.out.println("Aim down bazooka site");
    }

    @Override
    public void launch() {
        System.out.format("Squeeze bazooka trigger to launch %s\n", projectile.getType());
    } 
}

class BazookaShell implements Projectile {
    @Override
    public String getType() {
        return "BazookaShell";
    }
}
```

## Bow Arrow
<img src="/assets/AbstractFactory7.png" alt="Adding Bow/Arrow Pair" width = "100%" align="center" style="padding-right: 35px;">
 
```java
class ArcheryWeaponSystem implements LauncherSystem {
    @Override
    public Launcher acquireLauncher() {
        return new Bow();
    }

    @Override
    public Projectile acquireProjectile() {
        return new Arrow();
    }
}

class Bow implements Launcher {
    private Projectile projectile;

    @Override
    public int getProjectileCapacity() {
        return 1;
    }

    @Override
    public void load(Projectile projectile) {
        this.projectile = projectile;
        System.out.format("Notch %s into the bow string\n", projectile.getType());
    }

    @Override
    public void aim() {
        System.out.format("Pull back bow string and aim down %s\n", projectile.getType());
    }

    @Override
    public void launch() {
        System.out.format("Release bow string to launch %s\n", projectile.getType());
    } 
}

```

## The Dirty Main
Here’s `main()`, which executes the code above:
```java
public class AbstractFactory {
    public static void main(String[] args) throws Exception {
        Test.test();

        System.out.println();

        Warrior rifleman = new Warrior(new RifleWeaponSystem());
        rifleman.fire();

        System.out.println();

        Warrior antiTankSoldier = new Warrior(new BazookaWeaponSystem());
        antiTankSoldier.fire();

        System.out.println();

        Warrior archer = new Warrior(new ArcheryWeaponSystem());
        archer.fire();
    }
}
```

My class diagrams include Configurers, but the implementation doesn’t include specific Configurer classes. Configuration happens in `main()`. It instantiates several `Warrior` instances and has them fire.

When this code is executed, it produces the following output, which shows how the same implementation produces different results depending upon the concrete `LauncherSystem` that’s been injected into the `Warrier`:
```
End Tests

Load Bullet into rifle
Load Bullet into rifle
Load Bullet into rifle
Load Bullet into rifle
Load Bullet into rifle
Load Bullet into rifle
Aim down rifle barrel
Squeeze rifle trigger to shoot Bullet
Aim down rifle barrel
Squeeze rifle trigger to shoot Bullet
Aim down rifle barrel
Squeeze rifle trigger to shoot Bullet
Aim down rifle barrel
Squeeze rifle trigger to shoot Bullet
Aim down rifle barrel
Squeeze rifle trigger to shoot Bullet
Aim down rifle barrel
Squeeze rifle trigger to shoot Bullet

Load BazookaShell into bazooka       
Aim down bazooka site
Squeeze bazooka trigger to launch BazookaShell

Notch Arrow into the bow string
Pull back bow string and aim down Arrow
Release bow string to launch Arrow
```

Bob Martin presents configuration as being in the _lowest_ portion of the design. It may reside in `main()` as I’ve shown above, where it resides in the _dirtiest part of the program_ as Martin has called it.

I won’t say that `main()` is ___dirty___, but I’ve found that it’s often where dynamic configuration resides.

# The GoF Abstract Factory Mess
I feel that a messy diagram will lead to a messy design and messy implementation. The GoF’s Abstract Factory class diagrams are messy in my opinion. It’s possibly the messiest design in their catalog. There are many similar named elements, and the class diagram cannot be drawn without lines crossing each other. Abstract Factory may be their only design pattern diagram with crossing lines.

Any Abstract Factory class diagram that includes at least two concrete factories and two dependencies will have lines crossing each other, because the __2+ Factory/2+ Dependency Abstract Factory__ has a [complete 3-to-3 bipartite graph]( https://en.wikipedia.org/wiki/Complete_bipartite_graph), which I know cannot be planar due to [Kuratowski's Theorem](https://en.wikipedia.org/wiki/Kuratowski%27s_theorem) thanks to [Professor Johnson](https://en.wikipedia.org/wiki/Donald_B._Johnson), my graph theory professor in college.

My diagrams have been planar, since I limited them to one concrete Factory. Here’s my complete design with all three `LauncherSystems` added:

<img src="/assets/AbstractFactory8.png" alt="Non-Planar Diagram" width = "100%" align="center" style="padding-right: 35px;">

My complete diagram has so many crossing lines that it reminds me [Broadway Boogie Woogie](https://en.wikipedia.org/wiki/Broadway_Boogie_Woogie) by [Piet Mondrian]( https://en.wikipedia.org/wiki/Piet_Mondrian)

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/30/Piet_Mondrian%2C_1942_-_Broadway_Boogie_Woogie.jpg/800px-Piet_Mondrian%2C_1942_-_Broadway_Boogie_Woogie.jpg" alt="Broadway Boogie Woogie" title="Image Source: By Piet Mondrian - Transferred from en.wikipedia to Commons., Public Domain, https://commons.wikimedia.org/w/index.php?curid=37640791" width = "20%" align="right" style="padding: 35px;">

I color coded my diagram differently. Usually I use green for interfaces, blue for implementation, purple for configuration and red for externals. That’s still the case in the Abstract section, but the colors below the line take on one time meaning so that it’s a little easier to keep track of the relationships between cohesive classes, especially in the many crossing lines:
* Red is for the Rifle/Bullet classes.
* Green is for Bazooka/BazookaShell classes.
* Purple is for Bow/Arrows classes.

While the lines cross because this diagram is not a planar graph, notice that the three sets of cohesive colors never interact with one another. They have no dependencies or knowledge of one another. They peacefully coexist.

There’s another way to picture this. Don’t imagine this class diagram on one plane. Imagine it on multiple stacked planes. The Red, Green and Purple planes are stacked on top of each other where they each independently plug into the Abstract elements.

Each plane is planar within its own plane with the same structure:

<img src="/assets/AbstractFactory9.png" alt="Generic Abstract Factory" width = "100%" align="center" style="padding-right: 35px;">

Abstract Factory isn’t messy. It’s quite neat and orderly in three-dimensional space. It only becomes messy when the three-dimensional design is projected onto two-dimensional space.

# Summary
__TBD__

# References
__TBD__
