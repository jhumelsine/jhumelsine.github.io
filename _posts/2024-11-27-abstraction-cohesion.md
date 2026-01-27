---
title: What Is Cohesive Abstraction?
description: How to manage cohesion among abstractions consistently
---

# Introduction
This blog is a continuation of the previous [Abstract](https://jhumelsine.github.io/2024/10/30/abstraction.html) blog where I follow up with _Cohesive Abstractions_. 

Abstractions may reference other Abstractions. In doing so, they depend upon and have knowledge of those other Abstractions. We want to ensure that that the concrete implementations for these Abstractions are resolved consistently.

# Cohesion and Coupling
_Cohesion_ and _Coupling_ are related and somewhat confusing terms. Before I describe the consistent management of _Cohesive Abstractions_, make sure you understand _Cohesion_ and _Coupling_. The two previous blogs focus upon them:
* [What Are Cohesion and Coupling?](https://jhumelsine.github.io/2024/11/07/cohesion-coupling.html) - Cohesion and coupling address when things should and should not be too sticky with one another
* [Coupling and Cohesion – Take 2](https://jhumelsine.github.io/2024/11/22/coupling-and-cohesion-2.html) - Let’s revisit these concepts one more time

# Cohesive Abstractions
__Cohesive Abstractions__ are when coupled elements are abstractions, such as interfaces. Our goal is to ensure that the concrete classes implementing the abstractions are consistent in their given context. I’ll continue with an example to demonstrate these concepts.

## Call Of Duty
Let’s assume that you’re a developer on a military game, such as [Call of Duty](https://www.callofduty.com/), and you’re in the _Weapons_ development team.

You’ll want to design a gun that supports behaviors, such as: ready, aim and fire.

<img src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/757ae6c4-085d-4a02-8eda-8c9b097c414a/dg0ph9f-476ed86e-b6c6-4c8f-9b6f-cf02ed84dcdb.jpg/v1/fit/w_600,h_337,q_70,strp/military_game_assets___guns_by_nikarlux_dg0ph9f-375w-2x.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7ImhlaWdodCI6Ijw9MzM3IiwicGF0aCI6IlwvZlwvNzU3YWU2YzQtMDg1ZC00YTAyLThlZGEtOGM5YjA5N2M0MTRhXC9kZzBwaDlmLTQ3NmVkODZlLWI2YzYtNGM4Zi05YjZmLWNmMDJlZDg0ZGNkYi5qcGciLCJ3aWR0aCI6Ijw9NjAwIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmltYWdlLm9wZXJhdGlvbnMiXX0.SaiA8q2khJIA3MeigEC-X9PrV4qCpPCNJlr9QDW5aJw" alt="Military Guns" title="Image Source: https://www.deviantart.com/nikarlux/art/Military-Game-Assets-Guns-968647587" width = "35%" align="right" style="padding-right: 20px;">
 
You realize there can be multiple guns, so you want some degree of abstraction.

But not everything is a gun. What about bazookas? Or what about previous weapon technology, such as the musket, or even the bow and arrow?

These can all be concrete examples of paired cohesive abstractions of a __Launcher__ and __Projectile__. Some concrete pairs could be: __Gun/Bullet__, __Bazooka/Shell__, __Musket/Ball__, __Bow/Arrow__, __Sling/Shot__, etc.

Ready, aim and fire don’t quite work for some of these, so let’s generalize them with: load, aim and launch. This first design describes the cohesive abstractions:
* The `Warrior` represents the player, and we’re going to only focus upon the `Warrior`’s ability to interact with a `Projectile` and its `Launcher`.
* The `Launcher` declares the `load(Projectile projectile)`, `aim()` and `launch()` methods. 
* The `Launcher` interface has dependency upon and knowledge of the `Projectile` interface. `Projectile` does not have dependency or knowledge of `Launcher`.
 
<img src="/assets/AbstractCohesion1.png" alt="Launcher/Projectile Abstract Cohesion"  width = "80%" align="center" style="padding-right: 35px;">

I think this is the first time in any of my blogs where I show one interface referencing another interface. `Launcher` has dependency knowledge of `Projectile`.

`Launcher` and `Projectile` are cohesive. A `Launcher` isn’t much use with a `Projectile`, and `Projectile` isn’t much use with a `Launcher`. They must be consistent. A  `Bazooka` would not be of much use if its loaded `Projectile` were a `Bullet` or `Arrow`.

The concrete implementations for `Launcher` and `Projectile` must be consistent matching pairs.

## Abstract Factory Design Pattern
One way to ensure consistent matching pairs is via the __Abstract Factory Design Pattern__. I wrote about this pattern briefly in the [Abstract Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html#abstract-factory) section of the [Factory Design Patterns blog](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html). And it appears in a future blog: [Abstract Factory Design Pattern](https://jhumelsine.github.io/2025/07/30/abstract-factory.html).

The __Abstract Factory Design Pattern__ is the first pattern presented by the Gang of Four (GoF) in their Design Pattern book, because it’s alphabetically first. It’s a tough pattern to understand, which is one reason why I think the GoF’s book is difficult to learn design patterns.

## The Abstraction of the Abstract Factory Pattern
An Abstract Factory Pattern is an extension of the [Factory Pattern]( https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html#factories) concept.

A  __Factory__ is a class that returns a concrete instance for an abstract reference.
An __Abstract Factory__ is an interface that declares a set of __Factory__ methods each of which returns concrete reference to an abstract definition.
The concrete classes that implement the __Abstract Factory__ must implement each of those __Factory__ methods and return concrete references for each, such that those concrete references are consistent.
Do you see what I mean about __Abstract Factory__ being challenging as the first pattern presented in the GoF?

It's not quite as confusing as it sounds at first blush, but it requires some thought before one reaches understanding. I'll layer in the concepts a few at a time.

Let’s start with the __Abstract Factory__ interface that's needed for the `Launcher`/`Projectile` pair. `WeaponsSystem` is the __Abstract Factory__ in this example. It declares two methods that virtually create a `Launcher` and a `Projectile`. That is, these two methods declare an abstract [contract](https://jhumelsine.github.io/2025/06/10/contracts.html) to create references for these abstractions, but they don't create the concrete instances themselves.
 
<img src="/assets/AbstractCohesion2.png" alt="Abstract of Abstract Factory"  width = "80%" align="center" style="padding-right: 35px;">

## Completing the Abstract Factory Design with Concrete Factories
The above only declares the abstractions. This diagram adds the concrete __Factories__ that define the instantiate the concrete instances for `Launcher` and `Projectile`, which will be resolved in this example by a `Rifle` and `Bullet` respectively:
* `Launcher` is implemented by `Rifle`.
* `Projectile` is implemented by `Bullet`, since this is the only type of `Projectile` that's consistent with a `Rifle`.
* `RifleWeaponsSystem` implements `WeaponsSystem`. Its methods instantiate a `Launcher`/`Rifle` and `Projectile`/`Bullet` respectively. The two are ensured to be consistent.
* `RifleWeaponSystemConfigurer` isn’t technical part of the Abstract Factory Pattern. It’s the [Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer), which is a missing concept in most GoF patterns. It creates the `RifleWeaponSystem` instance and injects it into the `Warrior`.
* This design adds the red dashed lines used to designate __ABSTRACT__, __CONCRETE__ and __CONFIGURE__ architecture boundaries, which were introduced in [What vs How](https://jhumelsine.github.io/2024/10/30/abstraction.html#what-vs-how) in a previous blog.
 
<img src="/assets/AbstractCohesion3.png" alt="Full Design with Rifle"  width = "100%" align="center" style="padding-right: 35px;">

## Another WeaponSystem Abstract Factory
The Abstract Factory requires more design elements. It’s an investment. Here is part of the return on that investment. When we want to add a Bazooka, we update the design as follows:
 
<img src="/assets/AbstractCohesion4.png" alt="Full Design with Bazooka"  width = "100%" align="center" style="padding-right: 35px;">

The `Rifle` and `Bazooka` designs are identical in their Abstraction regions above the red dashed horizontal line. The distinctions below the line are almost boilerplate.

The addition of `Bazooka` as a weapon system does not replace or invalidate the `Rifle` weapon system. I did not show the previous `Rifle` related classes due to space constraints. This design can support as many `Launcher`/`Projectile` weapon system as the team imagines.

## And Testing Too
We can easily test the code in the Abstract regions with the same design. The distinction is that it uses Test specific concrete classes:
 
<img src="/assets/AbstractCohesion5.png" alt="Full Testing Design"  width = "100%" align="center" style="padding-right: 35px;">

## Warrior with Multiple WeaponSystems
Rarely would a `Warrior` have only one `WeaponSystem`. Different `WeaponSystem`s will have different attributes, such as:
* Range
* Accuracy
* Stealthiness
* Firing repeatability
* Etc.

The following design shows how a few updates to the previous designs can support a `Warrior` with multiple `WeaponSystem`s with the assurance that the `Launcher` and `Projectile` will be consistent if each concrete `WeaponSystem` concrete class implements them consistently:
* Rather than a single `WeaponSystem` attribute, the `Warrior` has multiple `WeasonSystem`s as a `List`.
* Each `WeaponSystem` has attributes, listed above, but not shown in the `WeaponSystem` interface due to space constraints.
* The `getBestWeapon(Target target)` method evaluates the `WeaponSystem`s in the `List` against the `target` and determines which one would be the best choice.
* `WarriorConfigurer` adds `WeaponSystem`s to the `Warrior`. The set of `WeaponSystem` references could change at any time.
* I don’t have enough space to list the `WeaponSystem`, `Launcher` and `Projectile` concrete classes, but they follow the previous designs.
* Except for the `List` of `WeaponSystem`s, `getBestWeapon(Target target)` method and the `WarrierConfigurer` updates, the rest of this design is the same as the previous designs.
 
<img src="/assets/AbstractCohesion6.png" alt="Multiple Weapon Design"  width = "100%" align="center" style="padding-right: 35px;">

## Bigger Boom
I’m going to stay with the same design structure, but I’ll modify the context slightly. Instead of a `Warrior` with a hand weapon, this design features a `CombatVehicle` as a `tank` with a `TankGun`/`TankGunShell` pair.
 
<img src="/assets/AbstractCohesion7.png" alt="Tank Design"  width = "100%" align="center" style="padding-right: 35px;">

## Safety Critical
I moved from `Warrior` to `CombatVehicle` to feature a real-world example.

I worked on a military project where the goal was to move combat vehicles, such as tanks, with their crew via large military cargo planes. 
The crew could train by running simulations within their vehicles to familiarize themselves with terrain, landmark features, etc. of their destination during the hours while flying to their destination.

The simulations could involve spotting targets and firing their weapons. It would be very bad to fire a tank shell from inside the cargo plane. The system included a __TRAINER__ mode so that the guns would not actually fire the shells. I didn’t work on the __TRAINER__ feature directly; therefore, what I describe is mostly hypothetical.

Code like this was considered [___Safety Critical___](https://en.wikipedia.org/wiki/Safety-critical_system), meaning that if there’s a bug in the code, it could seriously harm or kill people. Blowing a hole in the side of the plane during transit would definitely qualify as a ___Safety Critical___ concern.

Since I didn’t work directly on this code, I don’t know how they planned to implement this behavior, but given other code I experienced on the project, I feared it might look something like the design below, where an `if` statement would check the `mode` and then only fire the `TankGun` when `mode` is not `TRAINER`.
 
<img src="/assets/AbstractCohesion8.png" alt="Safety Critical Tank Design"  width = "100%" align="center" style="padding-right: 35px;">

This looks like a reasonable choice, but I have several issues with this design:
* `Mode` feels like behavior associated with the `TankGun`, but it resides in the `CombatVehicle`. It will work, but what if other code interacts with the `TankGun` as well. It will need to have the same logic. `TRAINER` logic will be distributed across the codebase with low cohesion. How confident will we be that all the `TRAINER` cases have been covered? __NOTE:__ In case you haven't realized it, `mode` is starting to describe a [__state machine__](https://jhumelsine.github.io/2024/11/22/coupling-and-cohesion-2.html#state-machine).
* There’s no behavior when in `TRAINER` mode. While we don’t want to fire the gun while in `TRAINER` mode, we want code to behave as if we had fired it for a more realistic training simulation for the crew. An `else` statement could handle the non `TRAINER` mode, but there's an issue with that as described in the next bullet.
* The `if` statement only checks for `TRAINER`. What if there’s another mode, such as `MAINTENANCE`, for which we don’t want to fire the gun as well. This design will fire the gun in any non `TRAINER` mode whether desired or not. We can always enhance the implementation with `else if` or `else` statements for other `mode` values, but given the distributed low cohesive design, it would clutter the applications and the maintenance will be more difficult. And inevitably one of those checks will be omitted somehwere critical. 

## Concrete Trainer Classes
We can think of `TRAINER` mode like [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html). They are different versions of `Launcher` and `Projectile`. Here’s how they would appear in the design:
 
<img src="/assets/AbstractCohesion9.png" alt="Safety Critical TankTrainer Design"  width = "100%" align="center" style="padding-right: 35px;">

This design is more cohesive. The `if` logic has been removed from the `CombatVehicle`.

However, there are still a few items that bug me:
* How do I know that `TankGun` and `TankGunTrainer` will stay in sync with respect to behavior that’s not associated with firing the shell? The same question lingers for `TankShell` and `TankShellTrainer` too. That is, if there’s an update to `TankGun`, will there be a corresponding update to `TankGunTrainer`?
* `Mode` doesn’t appear in the design. It’s implied in the __CONFIGURE__ region, but there are no details. This needs a bit more work.

## Final Design For Now
I continued to think about the design. I have made a few enhancements to the design, which I will describe one design region at a time.

<img src="/assets/AbstractCohesion10.png" alt="Overall Safety Critical Design"  width = "100%" align="center" style="padding-right: 35px;">

### ABSTRACT
There are zero changes in the __ABSTRACT__ region. That’s one of benefits of this design. Throughout this blog, the __ABSTRACT__ region has barely changed.

### CONCRETE
I wanted to address the complete separation of `TankGun` and `TankGunTrainer` in the previous designs. Often, we want complete separation of concrete classes, but I have this nagging feeling that these two classes, along with the `TankShell`/`TankShellTrainer` classes, should be cohesive.

`TankGun` and `TankGunTrainer` should exhibit nearly identical behaviors, with the distinction that `TankGun` launches the `TankShell`, whereas `TankGunTrainer` emulates launching the `TankShell`. If the two classes are separated, we run the risk of updating `TankGun` without updating `TankGunTrainer`. Their core behavior would be inconsistent. This could result a training simulation that behaves differently than it would when active in combat. We don't want our tank crew to encounter any surprises when combat behaviors are different than training behaviors.

The previous design only used the [Strategy Design Pattern](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) for `TankGun` and `TankGunTrainer`. This updated design adds two more design patterns: [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) and [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html).

`TankGun` has been modified from a concrete class to an abstract class. Any previous `TankGun` implementation that accessed the ___Actual Tank Gun___ would be extracted as an abstract protected method declared in the abstract `TankGun` and defined and implemented in the new `TankGunAdapter`. `TankGunAdapter` would delegate to the ___Actual Tank Gun___ directly. `TankGunTrainer` would also have to define and implement the same abstract protected methods in `TankGun`, but `TankGunTrainer` would have no access the ___Actual Tank Gun___. Its implementations could be empty __NO-OP__ methods, or it could record the ___Actual Tank Gun___ request, much like a [Spy](https://jhumelsine.github.io/2024/07/02/test-doubles.html#spy) would.

This design retains the bulk of the Tank Gun behavior implementation in `TankGun`. `TankGunAdapter` would fulfill the behavior with the ___Actual Tank Gun___, whereas `TankGunTrainer` would not. This segregation will help ensure that the ___Actual Tank Gun___ will not be engaged while in __TRAINER__ mode.

### CONFIGURE
`Mode` resides within the `TankGunWeaponSystem` concrete factory. `TankGunConfigurer` has no concern about `mode`. Its single responsibility is to create the `TankGunWeaponSystem` and inject it into the `tank` instance of the `CombatVehicle`.

`TankGunWeaponSystem` creates the appropriate concrete `TankGun` and `TankGunShell` objects based upon __ACTIVE__ or __TRAINER__ `mode`s.

This is the only place where `mode` is considered, which limits the scope of incorrect configurations. All possible `mode` values are considered in the code snippet. If a new `mode` is added, such as __MAINTENANCE__, then this method will throw an Exception. __NOTE:__ Each concrete `WeaponSystem` will need to consider `mode` within its own context.

I listed this as the final design ___for now___, because I’m still not 100% satisfied with it, but I’m not going to continue beyond this design in this blog. 

A future consideration includes: How is the `CombatVehicle` `tank` updated when the `mode` changes?

I would consider using the __Observer Design Pattern__, which I have not presented yet (blog TBD):
* [Observer via Sourcemaking](https://sourcemaking.com/design_patterns/observer)
* [Observer via Refactoring.guru](https://refactoring.guru/design-patterns/observer)

Just to give a hint of what I'd do, I’d consider an __Observer__, probably in the __CONFIGURE__ region, which would receive `mode` update notifications and make adjustments to the `tank` as needed.

I’m still not convinced that the `fire()` method works in all concrete occurrences of the abstraction. Does `fire()` accurately model a __bow and arrow__ as well as an __assault weapon machine gun__?

# Summary
__Cohesive Abstraction__ occurs when multiple **Abstraction**s have relationships that need to remain consistent. The __Abstract Factory Design Pattern__ is one mechanism that helps maintain consistency.

One could argue that my designs are overengineered. That’s possible, but the design is modular with good dependency management. It can be modified without a complete redesign as has been shown throughout this blog as I've tweaked it.

## A Cautionary Tale
Is consistency and all this effort really worth the effort? Let me close with this cautionary tale.

>One of the products I worked on had a video feature where customers could request that their users upload a short video. Our product stored the video in the cloud with an external video vendor. The vendor would provide a URL to the video resource on their system, and we’d store that in a user/video database table.
>
>One day multiple customers started contacting customer support stating that they couldn’t access any of the videos. Customer support confirmed the behavior, but they were unable to find a workaround.
>
>Development got involved. It didn’t take us too long to realize that the user/video table was gone. Poof! Nada! Bupkis!
>
>__No. No. No. Oh God. This is really bad. This is really, really bad.__
>
>We confirmed that the videos still resided with the external vendor, but we didn’t know which video was associated with which user.
>
>Database backups restored most of the missing user/video connections; however, the backups were either two weeks or two months old. This was years ago, and I don’t remember which. Regardless, the most recent and relevant user/video connections were still missing.
>
>After some more investigation, we found a command in the video vendor’s API that returned the URL of our entire set of videos. Part of their URL contained our user ID that we had submitted when uploading the video. Because our vendor provided that really important nugget of information, only by chance, we were able to extract the user ID and reconstruct most of the user/video connections information. It wasn’t a complete restoration, since the table had additional context information that was presisted only in the user/video table. That information was completely gone and unrecoverable, but we were happy to give our customers the ability to view their user videos once more.
>
>How did this happen? We never found the root cause. We think it was a _comedy of errors_. This is mostly speculation, but we think that it involved one of our user/video automated tests. The test manipulated the database. The test designer didn’t want to leave any test-created artifacts in the database after completion, since they could affect the next execution of the test. The test created a clean slate by deleting the user/video table upon completion. We think the automated test was executed against the production environment. When the test cleaned the _test_ environment, it deleted the __production__ table.
>
>I thought I was going to lose my job. I had been recently repremanded for another vendor issue that affected customers. However, we were commended for finding the issue and resolving it well enough in a few hours. We took additional steps to prevent future disasters, such as removing developer and tester write-access to the production database.
>
>The user/video feature wasn’t a major feature in our product. This could have a ___much worse___ disaster. Core business tables could have been deleted, and most of them would not have had an option for reconstruction.

This personal experience is the main reason why I feel that consistency is worth the effort.

# References
[Previous References]( https://jhumelsine.github.io/2024/10/30/abstraction.html#references)
