---
title: DRAFT – What Is Cohesive Abstraction?
description: How to manage cohesion among abstractions consistently
unlisted: true
---

# Introduction
This blog is a continuation of the previous [Abstract](https://jhumelsine.github.io/2024/10/30/abstraction.html) blog where I follow up with _Cohesive Abstractions_. Abstractions may reference other Abstractions. We want to ensure that when this occurs, the Abstraction references are resolved consistently.

# Cohesion and Coupling
Before I get into consistent management of _Cohesive Abstractions_, I need to explain _Cohesion_ and _Coupling_.

## Cohesion
It took me a while before I had a solid understanding of [Cohesion](https://en.wikipedia.org/wiki/Cohesion_(computer_science)).

Elements are cohesive when there’s an intrinsic relationship among them. The relationship is often gestalt in that the elements only have value when combined. Additionally, when there’s a change in their form or function most if not all all elements in the cohesive relationship change.

<img src="https://i0.pickpik.com/photos/475/455/659/screwdrivers-screws-red-black-preview.jpg" alt="Screw and Screwdriver" title="Image Source: https://www.pickpik.com/screwdrivers-screws-red-black-tools-work-6612" width = "35%" align="left" style="padding-right: 20px;">
 
The intrinsic relationship between __screw__ and __screwdriver__ is an example. Neither is functional without the other. The screwdriver must also match the screw. A flathead screwdriver won’t work on a Phillips head screw.

<img src="https://live.staticflickr.com/4102/4780764147_ca8650f6c4_b.jpg" alt="Nuts and Bolts" title="Image Source: https://www.flickr.com/photos/tudedude/4780764147" width = "35%" align="right" style="padding-right: 20px;">
 
__Nuts__ and __bolts__ are cohesive. They only function when screwed together, and when their sizes must match.

We want our designs to have high cohesion. That is, software elements with intrinsic relationships should be near one another in the design.

Low cohesion occurs when related software elements have been distributed across the design. For example, you’re enhancing an existing behavior, and it requires changes to a dozen files distributed throughout the entire design. Additionally, those files are cluttered with code that’s not directly related to the behavior that you’re updating.

<img src="https://upload.wikimedia.org/wikipedia/commons/f/fd/State_Machine.jpg?20090820151828" alt="State Machine Diagram" title="Image Source: https://commons.wikimedia.org/wiki/File:State_Machine.jpg" width = "35%" align="left" style="padding-right: 20px;">

Another egregious example of low cohesion, which I've seen all too often is __state machine__ behavior (blog TBD) that has been distributed across the implementation. This tends to occur when a class has a `private` attribute possible with the name of `status` or `state`. It's type is often an `enum`, but I've also seen it as a `boolean` and even a `String`. Its implied privacy is violated with `get` and `set`, which provide direct accesses.

Rather than keep the state machine behavior encapsulated within the class, the class is only the reponsible to retain the `state`. Other classes will `get` the `state` make business logic decisions and `set` the `state` with a new value. Not only does this low cohesive design distribute the implementation across far flung areas of the design, it distributes the knowedge. It makes it more difficult to know the __known state__ of the state machine.

Low cohesion has at least two concerns:
* Did you update all the files that need to be updated?
* What if your update breaks the other content that’s not directly related to the behavior you’re updating?

With cohesion software elements, a change to one element may require a change to the other related cohesive elements, like how a change in screw head causing a change in the screwdriver. With highly cohesive software elements, they are close enough to one another, such as in the same package, making it more likely that all required updates will occur consistently.

I consider the methods of an `interface` cohesive when:
* The methods are functionally related to and support one another.
* The removal of one of the methods diminishes the whole.

The traditional [Create, Read, Update and Delete](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) (CRUD) operations are cohesive in my mind. Remove anyone of them, and the remaining operations are incomplete. Once, the analysis of my requirements defined Create, Update and Delete requirements for a set of domain elements. There were no Read requirements. I asked my requirements creator why I was implementing all these features for data for which there were no defined users. _Oops._

A cohesive interface tends to follow the [Interface Segregation Principle](https://en.wikipedia.org/wiki/Interface_segregation_principle) (ISP). An interface that’s not cohesive, will not follow ISP, since it contains methods that can be separated and still retain their cohesiveness.

## Coupling
[Coupling](https://en.wikipedia.org/wiki/Coupling_(computer_programming)) is often uttered in the same breath with _cohesion_. It took me a while to gain a solid understanding of coupling, especially when used with cohesion. They seemed similar yet different, and I wasn’t sure how to distinguish them.

Coupling is also about connected concepts that cannot be easily disconnected, but with coupling they do not contain an intrinsic relationship.

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/3/37/2021-05-15_14_56_22_A_Reese%27s_Peanut_Butter_Cup_broken_into_two_pieces_in_the_Franklin_Farm_section_of_Oak_Hill%2C_Fairfax_County%2C_Virginia.jpg/1200px-2021-05-15_14_56_22_A_Reese%27s_Peanut_Butter_Cup_broken_into_two_pieces_in_the_Franklin_Farm_section_of_Oak_Hill%2C_Fairfax_County%2C_Virginia.jpg?20210515225457" alt="Reece's Cup" title="Image Source: https://commons.wikimedia.org/wiki/File:2021-05-15_14_56_22_A_Reese%27s_Peanut_Butter_Cup_broken_into_two_pieces_in_the_Franklin_Farm_section_of_Oak_Hill,_Fairfax_County,_Virginia.jpg" width = "35%" align="right" style="padding-right: 20px;">
 
Reece’s Peanut Butter Cups _couple_ chocolate and peanut butter. I know for some, a peanut butter cup would be an excellent example of cohesion, but I'm the odd duck who doesn't like peanut butter cups, even if I like chocoloate and peanut butter separately.

We want our designs to have loose coupling. That is, we don’t want unrelated elements to be stuck together too tightly.

This isn’t the best analogy, but a Reece’s Cup is loosely coupled because each major ingredient has its own separate identity before being combined into the candy. They are only coupled as a confectionary in the factory which manufactures them.

A Reece’s Cup would be tightly coupled if it were the only form from which we could get chocolate or peanut butter. Imagine scraping the insides from a Reece’s Cup as the only means to make a peanut butter sandwich.

Tight coupling occurs when disparate software concepts are stuck together. For example, the communication framework, business logic and persistence implementations are intertwined in the same method. A change to the communication framework may affect the business logic. A change in the persistence my affect the communication framework, etc. Reusing the business logic in another context becomes impossible.

Many modern software practices, such as [Design Patterns](https://jhumelsine.github.io/2023/08/24/its-your-move.html), [Hexagonal Architecture/Ports & Adapters](https://jhumelsine.github.io/2023/10/24/hexagonal-architecture-introduction.html), and others help address tight coupling. They promote loose coupling. The first [Design Pattern Principle](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html) addresses this, even if it doesn’t state so directly: __Program to an interface, not an implementation__

## Coupling and Cohesion
Tight coupling and low cohesion, as described above, are undesirable design traits.

A good design features loose coupling of software elements that don’t change together and high cohesion for those software elements that do change together.

This blog will focus upon high cohesion among abstractions and how to ensure consistency.

# Cohesive Abstractions
__Cohesive Abstractions__ are when the coupled elements are abstractions, such as interfaces. I’ll continue with an example to demonstrate these concepts.

## Call To Duty
Let’s assume that you’re a developer on a military game, such as [Call to Duty](https://www.callofduty.com/), and you’re in the _Weapons_ development team.

You’ll want to design a gun that supports behaviors, such as: load, aim and fire.

<img src="https://images-wixmp-ed30a86b8c4ca887773594c2.wixmp.com/f/757ae6c4-085d-4a02-8eda-8c9b097c414a/dg0ph9f-476ed86e-b6c6-4c8f-9b6f-cf02ed84dcdb.jpg/v1/fit/w_600,h_337,q_70,strp/military_game_assets___guns_by_nikarlux_dg0ph9f-375w-2x.jpg?token=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJ1cm46YXBwOjdlMGQxODg5ODIyNjQzNzNhNWYwZDQxNWVhMGQyNmUwIiwiaXNzIjoidXJuOmFwcDo3ZTBkMTg4OTgyMjY0MzczYTVmMGQ0MTVlYTBkMjZlMCIsIm9iaiI6W1t7ImhlaWdodCI6Ijw9MzM3IiwicGF0aCI6IlwvZlwvNzU3YWU2YzQtMDg1ZC00YTAyLThlZGEtOGM5YjA5N2M0MTRhXC9kZzBwaDlmLTQ3NmVkODZlLWI2YzYtNGM4Zi05YjZmLWNmMDJlZDg0ZGNkYi5qcGciLCJ3aWR0aCI6Ijw9NjAwIn1dXSwiYXVkIjpbInVybjpzZXJ2aWNlOmltYWdlLm9wZXJhdGlvbnMiXX0.SaiA8q2khJIA3MeigEC-X9PrV4qCpPCNJlr9QDW5aJw" alt="Military Guns" title="Image Source: https://www.deviantart.com/nikarlux/art/Military-Game-Assets-Guns-968647587" width = "35%" align="right" style="padding-right: 20px;">
 
You realize there can be multiple guns, so you want some degree of abstraction.

But not everything is a traditional gun. What about bazookas? Or what about previous technology, such as the musket, or even the bow and arrow?

These can all be concrete examples of cohesive abstractions of a Launcher and Projectile with concrete pairings such as: Gun/Bullet, Bazooka/Shell, Musket/Ball, Bow/Arrow, Sling/Shot, etc.

Load, aim and fire don’t quite work for some of these, so let’s generalize them a bit with: load, aim and launch. This first design describes the cohesive abstractions:
* The player is a `Warrior`, and we’re going to only focus upon the `Warrior`’s ability to fire a `Projectile` via a `Launcher`.
* The `Launcher` declares the `load(Projectile projectile)`, `aim()` and `launch()` methods. 
* The `Launcher` interface has dependency knowledge of the `Projectile` interface.
 
<img src="/assets/AbstractCohesion1.png" alt="Launcher/Projectile Abstract Cohesion"  width = "70%" align="center" style="padding-right: 35px;">

I think this is the first time in any of my blogs where I show one interface referencing another interface. `Launcher` has dependency knowledge of `Projectile`.

`Launcher` and `Projectile` have are cohesive. A `Launcher` isn’t much use with a `Projectile` and `Projectile` isn’t much use with a `Launcher`. And they must be consistent. While a `Bazooka` is a `Launcher`, it would be of much use if the `Projectile` were a `Bullet`.

The concrete implementations for `Launcher` and `Projectile` must be matching pairs.

## Abstract Factory Design Pattern
One way to help ensure this is via the __Abstract Factory Design Pattern__. I wrote about this pattern briefly in the [Abstract Factory](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html#abstract-factory) section of the [Factory Design Patterns blog](https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html).

The __Abstract Factory Design Pattern__ is the first pattern presented by the Gang of Four (GoF) in their Design Pattern book, because it’s alphabetically first. It’s a tough pattern to understand, which is one reason why I think the GoF’s book is difficult to learn design patterns. I will present the Abstract Factory Design pattern, and here are two good additional online resources:
* [Abstract Factory via Sourcemaking](https://sourcemaking.com/design_patterns/abstract_factory)
* [Abstract Factory via Refactoring.guru](https://refactoring.guru/design-patterns/abstract-factory)

## The Abstraction of the Abstract Factory Pattern
An Abstract Factory can be tough to understand, but it’s not really that complicated. It’s an extension of the [Factory Pattern]( https://jhumelsine.github.io/2023/10/07/factory-design-patterns.html#factories) concept.

A __Factory__ is a concrete class that returns a concrete instance to resolve an abstract reference. An __Abstract Factory__ is an interface that declares a set of __Factory__ methods each of which return concrete instances for those __Factory__ methods such that the set of returned references are consistent. It’s abstract so that different sets of consistent concrete instances can be resolved. See what I mean about this being a difficult pattern as the first one in the GoF.

Let’s start with the __Abstract Factory__ interface that would be needed for the `Launcher`/`Projectile` pair. `WeaponsSystem` has been added. It is the Abstract Factory, which declares two methods that virtually create a `Launcher` and a `Projectile`.
 
<img src="/assets/AbstractCohesion2.png" alt="Abstract of Abstract Factory"  width = "70%" align="center" style="padding-right: 35px;">

## Complete Abstract Factory Design with Concrete Factories
The above only shows the abstraction. This diagram adds the concrete __Factories__ that define the instantiate the concrete instances for `Launcher` and `Projectile`:
* `Launcher` is implemented by `Rifle` in this example.
* `Projectile` is implemented by `Bullet`, since this is the only type of `Projectile` that's consistent with a `Rifle`.
* `RifleWeaponsSystem` implements `WeaponsSystem`. Its methods instantiate a `Launcher`/`Rifle` and `Projectile`/`Bullet` respectively. The two are ensured to be consistent.
* `RifleWeaponSystemConfigurer` isn’t technical part of the Abstract Factory Pattern. It’s the Configurer, which is a missing concept in most GoF patterns. It creates the `RifleWeaponSystem` instance and injects it into the `Warrior`.
* This design adds the red dashed lines used to designate __ABSTRACT__, __CONCRETE__ and __CONFIGURE__ architecture boundaries, which were introduced in [What vs How](https://jhumelsine.github.io/2024/10/30/abstraction.html#what-vs-how) in the previous blog.
 
<img src="/assets/AbstractCohesion3.png" alt="Full Design with Rifle"  width = "80%" align="center" style="padding-right: 35px;">

## Another WeaponSystem Abstract Factory
The Abstract Factory requires more design elements. It’s an investment. Here is part of the return on that investment. When we want to add a Bazooka, we update design as follows:
 
<img src="/assets/AbstractCohesion4.png" alt="Full Design with Bazooka"  width = "80%" align="center" style="padding-right: 35px;">

The `Rifle` and `Bazooka` designs are identical in their Abstraction regions above the red dashed horizontal line. The distinctions below the line are almost boilerplate.

## And Testing Too
We can easily test the code in the Abstract regions with the same design. The distinction is that it uses Test specific concrete classes:
 
<img src="/assets/AbstractCohesion5.png" alt="Full Testing Design"  width = "80%" align="center" style="padding-right: 35px;">

## Warrior with Multiple WeaponSystems
Rarely would a `Warrior` have only one `WeaponSystem`. Different `WeaponSystem`s will have different attributes, such as:
* Range
* Accuracy
* Stealthiness
* Firing repeatability
* Etc.

The following design shows how with a few updates, the previous designs can be extended to support a `Warrior` with multiple `WeaponSystem`s with the assurance that the `Launcher` and `Projectile` will be consistent if each concrete `WeaponSystem` class implements them consistently:
* Rather than a single `WeaponSystem` attribute, the `Warrior` has multiple `WeasonSystem`s as a `List`.
* Each `WeaponSystem` has attributes, listed above, but not shown in the `WeaponSystem` interface due to space constraints.
* The `getBestWeapon(Target target)` method evaluates the `WeaponSystem`s in the `List` against the `target` and determines which one would be the best choice.
* `WarriorConfigurer` adds `WeaponSystem`s to the `Warrior`. The set of `WeaponSystem` references could change at any time.
* I don’t have enough space to list the `WeaponSystem`, `Launcher` and `Projectile` concrete classes, but they follow the previous designs.
* Except for the `List` of `WeaponSystem`s, `getBestWeapon(Target target)` method and the `WarrierConfigurer` updates, the rest of this design is the same as the previous designs.
 
<img src="/assets/AbstractCohesion6.png" alt="Multiple Weapon Design"  width = "80%" align="center" style="padding-right: 35px;">

## Bigger Boom
I’m going to stay with the same design structure, but I’ll modify the context slightly. Instead of a `Warrior` with a hand weapon, this design features a `CombatVehicle` as a `tank` with a `TankGun`.
 
<img src="/assets/AbstractCohesion7.png" alt="Tank Design"  width = "80%" align="center" style="padding-right: 35px;">

## Safety Critical
I moved from `Warrior` to `CombatVehicle` to feature a real-world example.

I worked on a military project where the goal was to move combat vehicles, such as tanks, with their crew via large cargo planes. In transit, the crew would train by running localized simulations in their vehicles to familiarize themselves with terrain, landmark features, etc. of their destination via electronic maps in the hours in the air.

The simulations could involve spotting targets and firing their weapons. It would be very bad to fire a tank shell from inside the cargo plane. Our software would feature a __TRAINING__ mode so that the guns would not actually fire the shells.

I didn’t work on these features directly. Code like this would be considered ___Safety Critical___, meaning that if there’s a bug in the code, it could seriously harm or kill people. Blowing a hole in the side of the plane would definitely qualify as ___Safety Critical___.

Since I didn’t work directly on this code, I don’t know how they planned to implement this, but given other code I experienced on the project, I feared it might look something like the design below, where an `if` statement checks the `mode` and it only fires the `TankGun` when `mode` is not `TRAINING`.
 
<img src="/assets/AbstractCohesion8.png" alt="Safety Critical Tank Design"  width = "80%" align="center" style="padding-right: 35px;">

I have several issues with this design:
* `Mode` feels like behavior associated with the `TankGun`, but it resides in the `CombatVehicle`. It will work, but what if other code interacts with the `TankGun` as well. It will need to have the same logic. `TRAINING` logic will be distributed across the codebase with low cohesion. How confident will we be that all the `TRAINING` cases have been covered? __NOTE:__ In case you haven't realized it, `mode` is starting to define a __state machine__.
* There’s no behavior when in `TRAINING` mode. While we don’t want to fire the gun while in `TRAINING` mode, we want code to behave as if we had fired it for a more accurate training scenario.
* The `if` statement only checks for `TRAINING`. What if there’s another mode, such as `MAINTENANCE` for which we don’t want to fire the gun as well. This design will fire the gun in any non `TRAINING` mode whether desired or not. We can always enhance the implementation with `else if` or `else` statements for other `mode` values, but given the distributed low cohesive design, it would clutter the applications and the maintenance will be more difficult.

## Concrete Training Classes
We can think of `TRAINING` mode almost like [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html). They are different versions of `Launcher` and `Projectile`. Here’s how they would appear in the design:
 
<img src="/assets/AbstractCohesion9.png" alt="Safety Critical TankTraining Design"  width = "80%" align="center" style="padding-right: 35px;">

This design is more cohesive. The `if` logic has been removed from the `CombatVehicle`.

However, there are still a few items that bug me:
* How do I know that `TankGun` and `TankGunTraining` will stay in sync with respect to behavior that’s not associated with firing the shell? The same question lingers for `TankShell` and `TankShellTraining` too. That is, if there’s an update to `TankGun` will there be a corresponding update to `TankGunTraining`?
* `Mode` doesn’t appear in the design. It’s implied in the __CONFIGURE__ region, but there are no details. This needs a bit more work.

## Final Design For Now
I continued to think about the design. I thought I could do better. I have made a few enhancements to the design, which I’ll describe one design region at a time.

### ABSTRACT
There are zero changes in the __ABSTRACT__ region. That’s one of benefits of this design. Throughout this blog, the __ABSTRACT__ region has barely changed.

### CONCRETE
I wanted to address the complete separation of `TankGun` and `TankGunTraining` in the previous designs. Often, we want complete separation of concrete classes, but I kept having the nagging feeling that these two classes, along with the `TankShell` classes, should be cohesive.

`TankGun` and `TankGunTraining` should exhibit nearly identical behaviors, with the distinction that `TankGun` launches the `TankShell`, whereas `TankGunTraining` emulates launching the `TankShell`. If the two classes are separated, we run the risk of updates to `TankGun` and `TankGunTraining` not being implemented consistently. This could result a training scenario that behaves differently when active in combat. We don't want our tank crew to encounter any surprises when combat behaviors are different than training behaviors.

The previous design only used the [Strategy Design Pattern](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) for `TankGun` and `TankGunTraining`. This updated design adds two more design patterns: [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) and [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html).

`TankGun` has been modified from a concrete class to an abstract class. Any previous `TankGun` implementation that accessed the Tank Gun hardware would be extracted as an abstract protected method declared in the abstract `TankGun` and defined and implemented in the new `TankGunAdapter`. `TankGunAdapter` would delegate to the ___Tank Gun hardware___ directly. `TankGunTrainer` would also have to define and implement the same abstract protected methods in `TankGun`, but `TankGunTrainer` would have no access the ___Tank Gun hardware___. Its implementations could be empty __NO-OP__ methods, or it could record the Tank Gun hardware request, much like a [Spy](https://jhumelsine.github.io/2024/07/02/test-doubles.html#spy) would.

This design retains the bulk of the Tank Gun behavior implementation in `TankGun`. `TankGunAdapter` would fulfill the behavior with the Tank Gun hardware, whereas `TankGunTraining` would not. This segregation will help ensure that the ___Tank Gun hardware___ will not be engaged while in __TRAINING__ mode.

### CONFIGURE
`Mode` resides within the `TankGunWeaponSystem` concrete factory. `TankGunConfigurer` has no concern about `mode`. Its single responsibility is to create the `TankGunWeaponSystem` and inject it into the `tank` instance of the `CombatVehicle`.

`TankGunWeaponSystem` creates the appropriate concrete `TankGun` and `TankGunShell` objects based upon __ACTIVE__ or __TRAINING__ `mode`s.

This is the only place where `mode` is considered, which limits the scope of incorrect configurations. All possible `mode` values are considered. If a new `mode` is added, such as `Maintenance`, then this method will throw an Exception. __NOTE:__ Each concrete `WeaponSystem` will need to consider `mode` within its own context.

I listed this as the final design ___for now___, because I’m still not 100% satisfied with it, but I’m not going to continue beyond this design here. 

Future considerations include: How is the `CombatVehicle` `tank` updated when the `mode` changes? I would consider using the __Observer Design Pattern__, which I have not presented yet (blog TBD):
* [Observer via Sourcemaking](https://sourcemaking.com/design_patterns/observer)
* [Observer via Refactoring.guru](https://refactoring.guru/design-patterns/observer)

Briefly, I’d consider an __Observer__, probably in the __CONFIGURE__ region, which would receive `mode` update notifications and make adjustments to the `tank` as needed.

I’m still not convinced that the `fire()` method works in all concrete occurrences of the abstraction. Does `fire()` accurately model a __bow and arrow__ as well as an __assault weapon machine gun__?

<img src="/assets/AbstractCohesion10.png" alt="Overall Safety Critical Design"  width = "80%" align="center" style="padding-right: 35px;">

# Summary
__Cohesive Abstraction__ occurs when multiple **Abstraction**s have relationships that need to remain consistent. The __Abstract Factory Design Pattern__ is one mechanism that helps maintain consistency.

One could argue that my designs are overengineered. That’s possible, but the design is modular with good dependency management. It can be modified without a complete redesign as has been shown throughout this blog.

## A Cautionary Tale
Is consistency and all this effort really worth the effort? Let me close with this cautionary tale.

One of the products I worked on had a video feature where customers could request that their users upload a short video. Our product stored the video in the cloud with an external video vendor. The vendor would provide a URL to the video resource on their system, and we’d store that in a user/video database table.

One day multiple customers started contacting customer support stating that they couldn’t access any of the videos. Customer support confirmed the behavior, but they were unable to find a workaround. Development got involved.

It didn’t take us too long to realize that the user/video table was gone. Poof! Nada! Bupkis!

__Oh God, Oh God, Oh God … This is really really really bad.__

We confirmed that the videos still resided with the external vendor, but we didn’t know which video was associated with which user.

Database backups restored most of the missing user/video connections; however, the backups were either two weeks or two months old. This was years ago, and I don’t remember which. Regardless, the most recent and relevant user/video connections were still missing.

After some more investigation, we found a command in the video vendor’s API that returned the URL of our entire set of videos. Part of their URL contained our user ID that we had submitted when uploading the video. Because our vendor provided that really important nugget of information only by chance, we were able to extract the user ID and reconstruct most of the user/video connections information. It wasn’t a complete restoration, since the table had additional context information. That information was completely gone and unrecoverable, but we were happy to give our customers the ability to view their user videos once more.

How did this happen? We never found the root cause. We think it was a _Comedy of Errors_. This is mostly speculation, but we think that it involved one of our user/video automated tests. The test manipulated the database. The test designer didn’t want to leave any test-created artifacts in the database after completion, since they could affect the next execution of the test. The test created clean slate for each execution by deleting the user/video table upon completion. We think the automated test was executed against the production environment. When the test cleaned the _test_ environment, it deleted the __production__ table.

I thought I was going to lose my job. However, we were commended for finding the issue and resolving it well enough in a few hours. We took additional steps to prevent future disasters, such as removing developer and tester write-access to the production database.

The user/video feature wasn’t a major feature in our product. This could have a ___much worse___ disaster. Important tables could have been deleted, and most of them would not have had an option to reconstruct them.

This personal experience is the main reason why I feel that consistency is worth the effort.

# References
[Previous References]( https://jhumelsine.github.io/2024/10/30/abstraction.html#references)
