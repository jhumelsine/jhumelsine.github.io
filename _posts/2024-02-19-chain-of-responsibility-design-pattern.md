---
title: Chain of Responsibility Design Pattern
description: Delegate a request through a linked chain of handlers until one of the handlers can complete the request.
unlisted: true
---

<img src="https://live.staticflickr.com/7143/6463738329_55faa2b606_b.jpg" alt="Customer Service Decision Tree" title="Image Source: https://www.flickr.com/photos/davegray/6463738329" width = "60%" align="center" style="padding-right: 20px;">

 
# Introduction
The Chain of Responsibility Design Pattern is the next of the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) series.

Chain of Responsibility (CoR) delegates a request through a set of handlers until one is able to complete the request.
The handlers are organized in a linked list, i.e., the _chain_, such that the least resource intensive handlers tend to reside earlier in the list and the more resource intensive handlers reside later in the list.

# Real World Analogies to Chain of Responsibility
Like [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html), CoR is not difficult to implement. Its main challenge is comprehension. Here are a few real-world examples to ease one into the CoR concept.

## Customer Support
<img src="https://live.staticflickr.com/5706/31270428786_f8a74a905c_b.jpg" alt="Customer Support Bank" title="Image Source: https://live.staticflickr.com/5706/31270428786_f8a74a905c_b.jpg" width = "20%" align="right" style="padding-right: 20px;">
 
Everyone has called customer support with an issue.
Customer support begins with a bank of front-line representatives doing their best to resolve the issue. But sometimes, they cannot, because they may not have the authority to resolve a challenging issue.

The customer may escalate by asking to speak to a manager, who has additional authority to resolve the issue. If the manager cannot resolve the issue, then the customer may escalate to the manager’s manager.

<img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZ2Jjc3Y0Y3prNTRrZDF4bW1iaWZhdzJxNGxwNmhwbWJvN3V5ZWJ2eSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/cLwoEpaLVa1iM3WRDO/giphy.gif" alt="Can I speak to a manager?" title="Image Source: https://media.giphy.com" width = "20%" align="right" style="padding-right: 20px;">

The customer may continue to escalate until either their issue is resolved, or they run out of managers.

Customer support is a Chain of Responsibility starting front-line representatives and proceeding through managers.
The front-line representatives have enough authority for most issues, but they may not have enough for more the challenging ones.
Front-line representatives can handle most requests leaving the more challenging requests to the diminishing number of managers who have more authority for the more challenging issues.

## The Judicial System
<img src="https://c.pxhere.com/photos/25/a4/justice_statue_lady_justice_greek_mythology_roman_goddess_god's_law_goddess_of_the_law_oh_titi_youth-1186582.jpg!d" alt="Justice statue" title="Image Source: https://pxhere.com/en/photo/118658" width = "20%" align="right" style="padding-right: 20px;">
 
The Judicial System in the United States is a type of Chain of Responsibility.

If a party in a legal dispute is dissatisfied, they have the option to appeal the decision to a higher court. This proceeds until the dissatisfied party is satisfied, the court refuses to hear the case, or the case reaches the highest court in the land.

## $PATH in Unix/Linux
In Unix/Linux, you can type the name of an executable from the command line.
The desired executable can be in almost any directory in the system.

`$PATH` is a shell variable that’s comprised of a colon separated list of directories where executables reside.
Unix/Linux will look for the executable one directory at a time.
It will execute the first one it finds that matches the name.
This is Chain of Responsibility.

Other __PATH__ variables are also used to find libraries, classes, man pages, etc. They exist in other operating systems too.

## Caches
A [Cache](https://en.wikipedia.org/wiki/Cache_(computing)) is a Chain of Responsibility.

Caches are quick, yet not always complete repositories of information. If information is not found in a cache, then it is retrieved from a more resource intensive source of truth such as a database or web service.

## Bloom Filter
A [Bloom Filter](https://en.wikipedia.org/wiki/Bloom_filter) is a Chain of Responsibility, mainly because it’s a form of cache.

It’s a space-efficient probabilistic algorithm that will efficiently determine with 100% accuracy if a piece of information is not in a data store, but it is not reliable when the information is present in the data store. I.e., its responses should be interpreted as:
* A 100% guarantee when the information is not in the data store.
* No guarantee when the information is in the data storey. A 100% guarantee is only possible by moving past the Bloom filter and accessing the data store directly.

## Switch and Cascading If/Else-If/Else Statements
<img src="https://upload.wikimedia.org/wikipedia/commons/d/d3/IF-THEN-ELSE-END_flowchart.png?20050817140753" alt="If/Then/Else/End Flowchart" title="Image Source: https://commons.wikimedia.org/wiki/File:IF-THEN-ELSE-END_flowchart.png" width = "20%" align="right" style="padding-right: 20px;">

`switch` statement and cascading `if/else-if/else` statements are a form of Chain of Responsility. The flow of control proceeds through the conditions until the first one is satisfied, and then its corresponding statement body is executed.

The main difference between `switch`/`if/else-if/else` and CoR is that the former is statically locked in the code and the former is dynamic. This is not unlike the comparison of inheritance and the [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) design pattern.

I’m not a huge fan of `switch`/`if/else-if/else` in the code base. I’ve encountered them being replicated repeatedly for different behaviors throughout the code base, rather organizing and encapsulating behaviors within their respective classes. `switch`/`if/else-if/else` code is difficult to understand and maintain. This is a [code smell](https://refactoring.guru/smells/switch-statements) and really a topic for another blog.

Sometimes these control flow structures are needed for algorithmic or business policy. For me, they become code smells when the control flow structures are based upon different type variations of the same common theme.

# Design Structure
Chain of Responsibility and [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) are almost identical structurally, but they are distinct in behavior. I will highlight their similarities and differences before getting into the CoR’s design structure.

## How Chain of Responsibility and Decorator are the same
The UML class diagrams for Chain of Responsibility and Decorator look almost identical. Both feature an abstract class that delegates back to a version of itself. This defines the linked list for both.

Both patterns feature delegating calls down the linked list.

Properties of the linked list are so similar that the same delegate reference can be used for both CoR and Decorator behavior in the same class. In some cases, even in the same method.

## How Chain of Responsibility and Decorator are the same
The primary difference between Chain of Responsibility and Decorator is in how they delegate down the linked list.

Decorator always delegates down the linked list to the anchor object at the end and returns. Each object in the list can apply different behaviors on the delegate calls down the linked list and/or on the returns coming back. All objects in the list are always executed and all contribute to the aggregate behavior.

Chain of Responsibility delegates down the linked list until one of the handler objects in the linked list can complete the request. That handler completes the request, and flow returns back the list without traversing to the other handlers in the list. If the first handler object in the list is sufficient, then no other handler objects are executed. Only one handler object in the list handles the request and none of the others contribute to the completion of the request.

It’s also possible that no object in the linked list can complete the request.

## GoF Chain of Responsibility
Here’s a summary of the GoF’s CoR design:
* I added a `RequestHandler` interface, which the GoF does not include in their diagram. I prefer an interface contract at the top of the design. In the GoF, their abstract `DelegatingRequestHandler`’s delegating reference is back itself.
* The `DelegatingRequestHandler` implements `RequestHandler`’s `handleRequest()` by delegating to its handler reference.
* The real work resides in the concrete classes that extend `DelegatingRequestHandler`. I’ve provided two as an example. One uses __A__ and the other uses __B__. These are placeholder names for specific request handler mechanisms.
* Each concrete class determines whether it can handle the request and if so, it handles it. Otherwise, it delegates to the base class `handleRequest()`, which delegates to the next handler in the linked list.
* The `Configurer`, which is not represented in the GoF’s diagram, creates the concrete handler instances, arranges them as a linked list and injects them into the `Client`.
* The `Client` has no dependency upon or knowledge of the concrete handlers. It only has a reference to a `RequestHandler` from which it calls `handleRequest()`.

<img src="/assets/ChainOfResponsibilityGoF.png" alt="Chain of Responsibility via GoF" width = "90%" align="center" style="padding-right: 20px;">

Here’s an example of the linked list of objects that can be created for this design:

<img src="/assets/ChainOfResponsibilityGoFObjects.png" alt="Chain of Responsibility via GoF with Objects" width = "65%" align="center" style="padding-right: 20px;">
 
In this example, __B__ is the more reliable and more resource intense handler. __A__ is the less reliable and less resource intense handler.
While we could configure an __A__ handler after the __B__ handler, there’s no logical reason to do so.

Much like the GoF’s Decorator design, I have a few issues with their Chain of Responsibility design:
* If programming to an interface is so much better than programming to an implementation, then they should have placed an interface at the top of their design as I did.
* Their design demands too much responsibility upon the concrete classes. Their design requires a lot of faith that the concrete class developer will get the `if/else` logic right.
* I don’t like that the `else` case requires a call to the base class via `super.handleRequest()`. This violates encapsulation.
* There’s no guarantee that any of the handlers will be able to handle the request. If that’s the case, then the call to `super.handleRequest()` will encounter a null reference. The GoF did put a null check in their example C++ code, but the concept is not represented in the design.

## Chain of Responsibility with Template Method
I want to remove my depending upon faith in my fellow developers to get everything right in the concrete handlers.

This design incorporates the [Template Method Design Pattern](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) into the design, much as I did with [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html):
* The logic to delegate down the chain has been moved up to the abstract `DelegatingRequestHandler` in `handleRequest()`. Delegating down the chain is no longer the responsibility of the concrete handlers or their developers.
* Though `DelegatingRequestHandler.handleRequest()` contains the delegating logic, it still depends upon two abstract methods, `iCanProcessRequest()` and`processRequest()`, for concrete behavior specifics. Each concrete handler must implement both of these methods.
* `ConcreteRequestHandlerA` and `ConcreteRequestHandlerB` each implement `iCanProcessRequest()` and`processRequest()` as mentioned above, but relative to __A__ and __B__ respectively. The concrete handlers don’t know they are part of a chain. Their only concerns are whether they can handle the request and handle its processing.
* `DelegatingRequestHandler.handleRequest()` only requires two test cases:
    * Confirm that `processRequest()` is executed, when `iCanProcessRequest()` returns true.
    * Confirm that `requestHandler.requestHandler()` is executed, when `iCanProcessRequest()` returns false.
* `DelegatingRequestHandler.handleRequest()` is declared as `final` so that a concrete handler cannot override it, even if unintentionally.
* `DelegatingRequestHandler`’s `requestHandler` field attribute is described as __Non-Null__. This ensures that when referenced, it won’t throw an `NullPointerException`. I do not show how this __Non-Nullness__ can be achieved since there was insufficient space to present it. If I had the space, the `DelegatingRequestHandler` would define a constructor with a `RequestHandler` parameter. The constructor would throw an exception if it were null.
* `DelegatingRequestHandler`’s non-null `requestHandler` constraint means that each linked list chain must end with a non-delegating handler, which in this design is `AnchoringRequestHandler`. There’s no guarantee that any request handler will be able to handle the request. When the flow reaches `AnchoringRequestHandler` then the request has gone unhandled. `AnchoringRequestHandler` serves the same function as the `default` in a `switch` statement or the `else` of a cascading set of `if/else-if` statements. Default behavior will vary based upon context. I’ve provided a few options as comments within the design.

<img src="/assets/ChainOfResponsibilityTemplateMethod.png" alt="Chain of Responsibility using Template Method" width = "90%" align="center" style="padding-right: 20px;">

Here’s an example of the linked list of objects that can be created for this design, which is the same as the objects in the GoF’s CoR example, but with the addition of `anchoringRequestHandler` at the end of the chain:

<img src="/assets/ChainOfResponsibilityTemplateMethodObjects.png" alt="Chain of Responsibility via GoF with Objects" width = "75%" align="center" style="padding-right: 20px;">
 
# Use Case – Address Book
This Chain of Responsibility Use Case example is based upon an actual work assignment I had. 

## The Problem Scenario
One day our PM/CTO called me into his office. He described a data management situation to me. Rather than attempt to recreate a 12-year-old conversation from memory, I’ll highlight the main points:
* Our customer’s organization was highly hierarchical and dynamic.
* Users needed to know their place in the organization hierarchy which could change at any time due to its dynamic nature.
* Organization hierarchy had two data stores: a database and a webservice.
* Due to resource constraints of the users, some only had access to the database. Some only had access to the webservice. Some had access to both.
* The webservice was the source of truth. The database was a copy for those users who didn’t have access to the webservice.
* He wanted a cache if possible.
* There was also the possibility that another organization’s hierarchy could be added in the future, and could the design accommodate that too? This never happened while I was on the project, but it was something to keep in mind.

Then he asked if I knew if a design pattern might help with this.

## Chaining Data Sources
<img src="https://live.staticflickr.com/3470/3254883191_f555a28366_b.jpg" alt="Little Black Book" title="Image Source: https://www.flickr.com/photos/84609865@N00/3254883191/" width = "30%" align="right" style="padding-right: 20px;">

While I had never used Chain of Responsibility, it was the first idea that came to my mind. I went through several design iterations before settling upon something like this design:
 
* I called it an `AddressBook`. I envisioned a swinging bachelor from the 1960s who might look for a phone number for a weekend date by first consulting his __little black book__ and then the __phone book__ and finally call __directory assistance__ as his options thinned out.
* I’m calling the organizational entity a __Group__ in this example. It’s a generic term. It can represent any named Group at any position in the organizational hierarchy.
* The `AddressBook` returns an `Optional<Group>` upon a look up rather than `Group`, since it’s possible that there may not be a `Group` by that name in the organization.
* `AddressBookHandler` is where most of the design details resides. It’s using [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html), but it’s slightly different than what was shown with the generic CoR Template Method example above:
    * There is no separate `iCanProcessRequest()` and `processRequest()` methods. Both behaviors are represented in `Optional<Group> getGroup(String name)`:
    * When `Optional<Group>` is not empty, then the request has been completed, and the `Optional<Group>` can be returned
    * When `Optional<Group>` is empty, then delegate the request to the next `AddressBook` and return whatever it returns, even if empty.
    * When delegating to the next `AddressBook` and it returns a non-empty `Optional<roup>`, then this design takes the opportunity to add the missing `Group` into the current `AddressBookHandler`. This is not part of the GoF’s CoR design, but I have included it mostly to populate the cache. This additional behavior to call `addGroup(String name, Group group)` is the [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) design pattern.
* `AddressBookHandler` has three paths of execution that need to be tested:
   * When `getGroup(name)` finds a `Group` and returns a non-empty `Optional<Group>`.
   * When `getGroup(name)` does not find a `Group` and returns an empty `Optional<Group>`, and then:
      * `addressBookHandler.getGroupByName(name)` finds a `Group` and returns a non-empty `Optional<Group>`.
      * `addressBookHandler.getGroupByName(name)` does not find a `Group` and returns an empty `Optional<Group>`.
* `GroupNotFound` returns an empty `Optional` in all cases as the default behavior since none of the `AddressBookHandlers` were able to find a `Group` by name.

<img src="/assets/ChainOfResponsibilityAddressBookA.png" alt="Address Book via Chain of Responsibility" width = "90%" align="center" style="padding-right: 20px;">

## Concrete AddressBookHandlers
I can’t quite fit the entire design on one diagram, since the `AddressBookHandler` details required considerable space.
If I were a design only for me, I wouldn't have added the concrete classes above.
The abstract `AddressBookHandler` would have been sufficient for the design.
But since this design is trying to convey the overall design, I included them above.

Here’s the second part of the design, which provides some additional design details for the concrete AddressBooks:
* This diagram replicates the AddressBook classes from the previous diagram, but it focuses more upon the concrete AddressBooks.
* Each concrete AddressBook implements the protected methods declared in `DelegatingAddressBook`.
* `CacheAddressBook` implements the methods via a `Map`.
* `DataBaseAddressBook` is grossly simplified. There’s insufficient room to represent more. What’s important is that `DataBaseAddressBook` delegates to the external `Database`, possibly via SQL. This delegation is an example of the [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) design pattern.
* `WebServiceAddressBook` is likewise the same as `DataBaseAddressBook`. It’s also grossly simplified due to space constraints. It delegates to the `WebService` possibly via REST. This delegation is also an example of the [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) design pattern.

<img src="/assets/ChainOfResponsibilityAddressBookB.png" alt="Address Book Concrete Classes via Chain of Responsibility" width = "90%" align="center" style="padding-right: 20px;">

If this were a design just for me, it would have started with the abstract `AddressBookHandler` as before, but without the interface it implements. And if more space were needed for each concrete Address Book class, then the above could have been split into three separate diagrams to provide additional details for each concrete class as needed.
 
## Cache Invalidation
The design as shown has an issue with stale cached `Group`s. I mentioned that the organizational hierarchy was dynamic. It could change at any time. As designed, once a `Group` is in the cache, it’s never updated or removed.

We were able to accommodate for this with another design feature in the real project, which I’ll describe briefly, but I won’t represent in the design example.
We subscribed to updates and deletions from the Database and the WebService. When we received a notification that a `Group` had been updated or deleted, we deleted it from the `Cache`. A fresh version would be repopulated in the `Cache` upon the next request that referenced it.

The subscription and notification mechanism is the [Observer](https://sourcemaking.com/design_patterns/observer) design pattern, which I’ve not mentioned previously, but I plan to blog about it in the future.

And to be technically accurate, our `DataBaseAddressBook` did not add the `Group` to the Database in its `addGroup(String name, Group group)` method. Another organization was responsible for the Database’s content. We only had read-only permission. Therefore the `addGroup(String name, Group group)` method, for which an implementation was required, was an empty method. The same was true for `WebServiceAddressBook.addGroup(String name, Group group)` method for similar reasons.

## Configurer
Composable designs provide the structural potential but not behavior. The behavior must be composed by a `Configurer` type of class.
My PM/CTO had described the possible configurations. They would be:

<img src="/assets/ChainOfResponsibilityAddressBookObjects.png" alt="Address Book via Chain of Responsibility Objects" width = "90%" align="center" style="padding-right: 20px;">
 
My first `Configurer` implementation was based upon building the linked list chain from a configuration String value, such as:
* `ADDRESSBOOK_PATH=Cache:DataBase:WebService:GroupNotFound`
* `ADDRESSBOOK_PATH=Cache:WebService:GroupNotFound`
* `ADDRESSBOOK_PATH=Cache:DataBase:GroupNotFound`

The `Configurer` would split `$ADDRESSBOOK_PATH` with a colon delimiter, acquire the corresponding object from an `AddressBookFactory`, not shown in the diagram, and chain them together into a linked list.

This worked fine in my testing, but it was not a good production quality solution. Our product was part of a fleet embedded systems for which there could be hundreds if not thousands of entities. Configuring each embedded system correctly was not realistic.

The first obvious observation was that `ADDRESSBOOK_PATH` always started with Cache and ended with GroupNotFound. The only parts that varied were DataBase and WebService.

Other environment variables implied whether the DataBase or a WebService was accessible in that environment. Therefore, I updated the `Configurer` so that it constructed `ADDRESSBOOK_PATH` based upon known values and environment variables. The configuration issue evaporated.

## Introducing a new Organization
My PM/CTO had hinted that the `AddressBook` might need to accommodate a sibling organization in addition to the original customer. As I mentioned above, that never materialized.

However, if it were to materialize, then we would have had to consider the following:
* The sibling organization might have different concepts for a `Group`. We might have had to either translate the second organization’s terminology into the existing `Group` or potentially normalize `Group` such that it would work for both organizations possibly with translation for both organizations.
* There could be several linked list chain management possibilities including:
    * Create a chain comprised of `AddressBookHandler`s from both organizations.
    * Ascertain the organization from its name, and then configure a chain on the fly that consisted of only `AddressBookHandler`s for that organization.
    * Ascertain the organization from other environment variables, and then configure a chain on the fly that consisted of only `AddressBookHandler`s for that organization.
    * Others? Composition resides in the `Configurer`, which is unstable/flexible and therefore invisible to the rest of the design. We could try different approaches and update them without an impact upon the rest of the design.

## Pattern Review
The AddressBook use case features more than just Chain of Responsibility. Design patterns are rarely used in isolation. They often work together.

Here’s a quick summary of the design patterns featured or mentioned in this use case:
* Chain of Responsibility the pattern of this blog
* [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) to move most of the CoR behavior into the abstract class and away from the concrete Address Book handlers and their developers
* [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) to populate the Cache and other Address Book Handlers when an unfound `Group` is found by a subsequent handler.
* [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) to allow the design to interact with external entities
* [Observer](https://sourcemaking.com/design_patterns/observer) to keep the Cache up to date, but only mentioned this blog, not featured

# Chain of Responsibility Pros and Cons
The relative pros and cons of Chain of Responsibility are like those with most of the Composable design patterns.

## Cons
Composable behavior may be more difficult to diagnose with Chain of Responsibility. While it should be simple to understand individual concrete classes in a CoR design, the responsibility to orchestrate observable behavior resides in the `Configurer`.
Integration or acceptance tests may be needed to confirm that desired behavior emerges when the objects are composed.

## Pros
Chain of Responsibility’s composable behavior is also a pro. Behavior is not hardcoded into place as is the case with `switch` or `if/else-if/else` statements. When a new responsibility chain is desired, it may be a relatively simple matter of a new composition of objects instantiated from the existing handler classes in the design.

Sometimes new handler responsibility is beyond the scope of a new composition. A new handler class may need to be implemented. Existing concrete classes can be implementation references to provide examples for new concrete class implementation.

The classes can be unit tested with ease. Even the abstract `DelegatingRequestHandler` class can be unit tested with a dependency upon a [test double](https://martinfowler.com/bliki/TestDouble.html) without depending upon another concrete class in the design.

There may be many concrete classes in a CoR design, but they tend to be independent. They don’t depend upon or know about each other. Therefore, new concrete classes can be added with fear of breaking existing classes. It’s also easier to identify and remove deprecated classes when they are no longer needed.

# Summary
Chain of Responsibility is …

# References
There are many online resources with diagrams and implementations in different programming languages. Here are some free resources:
* TBD

Here are some resources that can be purchased or are included in a subscription service:
* TBD
