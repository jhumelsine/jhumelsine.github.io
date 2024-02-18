---
title: Chain of Responsibility Design Pattern
description: Delegate a request to a linked list of request handlers traversing the linked chain until one of the handlers can complete the request.
unlisted: true
---

<img src="https://live.staticflickr.com/7143/6463738329_55faa2b606_b.jpg" alt="Customer Service Decision Tree" title="Image Source: https://www.flickr.com/photos/davegray/6463738329" width = "60%" align="center" style="padding-right: 20px;">

 
# Introduction
The Chain of Responsibility Design Pattern is the next of the [Composable Design Patterns](https://jhumelsine.github.io/2024/01/03/composable-design-patterns-basic-concepts.html) series.

Chain of Responsibility (CoR) delegates a request to be completed by one request handler from a given set of request handlers. Since some handlers may not be able to complete the specific request, the request may be delegated to more than one handler. The request handlers are organized in a linked list, i.e., the _chain_, such that the least resource intensive handlers tend to reside earlier in the list and the more resource intensive handlers reside later in the list.

# Real World Analogies to Chain of Responsibility
Like [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html), CoR is not difficult to implement. Its main challenge is comprehension. Here are a few real-world examples to ease one into the CoR concept.

## Customer Support
<img src="https://live.staticflickr.com/5706/31270428786_f8a74a905c_b.jpg" alt="Customer Support Bank" title="Image Source: https://live.staticflickr.com/5706/31270428786_f8a74a905c_b.jpg" width = "20%" align="right" style="padding-right: 20px;">
 
Everyone has run into issues with a product at some point and has called customer support. Customer support begins with a bank of front-line customer support representatives doing their best to resolve the issue. But sometimes, they cannot.

The customer may escalate by asking to speak to a manager, who usually has additional resources to resolve the issue. If the manager cannot resolve the issue, then the customer may escalate to the manager’s manager.

<img src="https://media.giphy.com/media/v1.Y2lkPTc5MGI3NjExZ2Jjc3Y0Y3prNTRrZDF4bW1iaWZhdzJxNGxwNmhwbWJvN3V5ZWJ2eSZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/cLwoEpaLVa1iM3WRDO/giphy.gif" alt="Can I speak to a manager?" title="Image Source: https://media.giphy.com" width = "20%" align="right" style="padding-right: 20px;">

The customer may continue to escalate until their issue is resolved to their satisfaction, or they run out of managers or executives who can resolve the issue.

Customer support is a Chain of Responsibility from front-line representatives to layers of managers and finally to layers of executives. The front-line representative is the least resource intensive issue handler in that that representative may not have all the resources to satisfy the customer, but the company can afford to hire many of them to resolve most issues. Subsequent representatives in the management chain will have more resources at their disposal, but they are fewer in number.

## The Judicial System
<img src="https://c.pxhere.com/photos/25/a4/justice_statue_lady_justice_greek_mythology_roman_goddess_god's_law_goddess_of_the_law_oh_titi_youth-1186582.jpg!d" alt="Justice statue" title="Image Source: [https://media.giphy.com](https://pxhere.com/en/photo/1186582)" width = "20%" align="right" style="padding-right: 20px;">
 
The Judicial System in the United States, and I assume many other countries as well, is a type of Chain of Responsibility. If a party in a legal dispute is dissatisfied, then they may have the option to appeal the decision to a higher court. This proceeds until the dissatisfied party is satisfied, or the court refuses to hear the case, or the case reaches the highest court in the land.

## $PATH in Unix/Linux
In Unix/Linux, you can type the name of an executable from the command line. An executable can be in almost any directory in the system, and there may be many potential directories where the executable may reside.

`$PATH` is a shell variable that’s comprised of a colon separated list of directories where executables reside. The system will look for the executable one directory at a time. It will execute the first one it finds that matches the name. This is Chain of Responsibility.
__PATH__ variables are also used to find libraries, classes, man pages, etc. They exist in other operating systems too.

## Caches
A [Cache](https://en.wikipedia.org/wiki/Cache_(computing)) is a Chain of Responsibility. Caches are often quick, yet not always complete. If information is not found in a cache, then it is retrieved from a more resource intensive source of truth such as a database or web service.

## Bloom Filter
A [Bloom Filter](https://en.wikipedia.org/wiki/Bloom_filter) is a Chain of Responsibility, mainly because it’s a form of cache.

It’s a space-efficient probabilistic algorithm that will compute with 100% accuracy if a piece of information is not in a data store, but it is not accurate otherwise. I.e., its responses should be interpreted as:
* Not in the data store with 100% assurance.
* Possibly in the data store, at which point, delegating to the data store should be considered for 100% assurance.

## Switch and Cascading If/Else-If/Else Statements
`switch` statement and cascading `if/else-if/else` statements are a form of Chain of Responsility. The flow of control proceeds through the conditions until the first one is satisfied, and then its corresponding statement body is executed.

The main difference between `switch`/`if/else-if/else` and CoR is that the former is statically locked in the code and the former is dynamic. This is not unlike the comparison of inheritance and the [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) design pattern.

I’m not a huge fan of `switch`/`if/else-if/else` in the code base. I’ve encountered them being replicated repeatedly with different behaviors throughout the code base, rather organizing and encapsulating behavior within classes. `switch`/`if/else-if/else` code is difficult to understand and maintain. This is a [code smell](https://refactoring.guru/smells/switch-statements) and really a topic for another blog.

But sometimes these control flow structures are needed for algorithmic or business policy. For me, they become code smells when the control flow structures are based upon different type variations of the same theme rather.

# Design Structure
Chain of Responsibility and [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) are almost identical structurally, but they are distinct in behavior. I will highlight their similarities and differences before getting into the CoR’s design structure.

## How Chain of Responsibility and Decorator are the same
The UML class diagrams for Chain of Responsibility and Decorator look almost identical. Both feature an abstract class that delegates back to a version of itself. This defines the linked list for both.

Both patterns feature delegating calls down the linked list.

Properties of the linked list are so similar that the same delegate reference can be used for both CoR and Decorator behavior in the same class. In some cases, even in the same method.

## How Chain of Responsibility and Decorator are the same
The primary difference between Chain of Responsibility and Decorator is in how they delegate down the linked list.

Decorator always delegates down the linked list to the anchor object at the end and returns. Each object in the list can apply different behaviors on the delegate calls down the linked list and/or on the returns coming back. All objects in the list are always executed and all contribute to the aggregate behavior.

Chain of Responsibility delegates down the linked list until one of the handler objects in the linked list can complete the request. That handler object completes the request, and flow returns back the list. If the first handler object in the list is sufficient, then no other handler objects are executed. Only one handler object in the list handles the request and none of the others contribute to the completion of the request. It’s also possible that no object in the linked list can complete the request.

## GoF Chain of Responsibility
Here’s a summary of the GoF’s CoR design:
* I added a `RequestHandler` interface, which the GoF does not include in their diagram. I prefer an interface contract at the top of the design. In the GoF, their abstract `DelegatingRequestHandler`’s delegating reference is back itself.
* The `DelegatingRequestHandler` implements `RequestHandler`’s `handleRequest()` by delegating to its handler reference.
* The real work resides in the concrete classes that implement `DelegatingRequestHandler`. I’ve provided two as an example. One uses __A__ and the other uses __B__. These are placeholder names for specific request handler mechanisms.
* Each concrete class determines whether it can handle the request and if so, it handles it. Otherwise, it delegates to the base class `handleRequest()`, which delegates to the next handler in the linked list.
* The `Configurer`, which is not represented in the GoF’s diagram, creates the concrete handler instances, arranges them as a linked list and injects them into the `Client`.
* The `Client` has no dependency upon or knowledge of the concrete handlers. It only has a reference to a `RequestHandler` from which it calls `handleRequest()`.

<img src="/assets/ChainOfResponsibilityGoF.png" alt="Chain of Responsibility via GoF" width = "90%" align="center" style="padding-right: 20px;">

Here’s an example of the linked list of objects that can be created for this design:

<img src="/assets/ChainOfResponsibilityGoFObjects.png" alt="Chain of Responsibility via GoF with Objects" width = "65%" align="center" style="padding-right: 20px;">
 
In this example, __B__ is the more reliable and more resource intense handler. __A__ is the less reliable and less resource intense handler. While we could configure an __A__ handler after the __B__ handler, there’s no logical reason to do so.
Much like the GoF’s Decorator design, I have a few issues with their Chain of Responsibility design:
* If programming to an interface is so much better than programming to an implementation, then they should have placed an interface at the top of their design as I did.
* Their design demands too much responsibility upon the concrete classes. Their design requires a lot of faith that the concrete class developer will get the `if/else` logic right.
* I don’t like that the `else` case requires a call to the base class via `super.handleRequest()`. This violates encapsulation.
* There’s no guarantee that any of the handlers will be able to handle the request. If that’s the case, then the call to `super.handleRequest()` encounter a null reference. The GoF did put a null check in their example C++ code, but the concept is not represented in the design.

## Chain of Responsibility with Template Method
I want to remove depending upon faith in my fellow developers to get everything right in the concrete handlers.

This design incorporates the [Template Method Design Pattern](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) into the design, much as I did with Decorator:
* The logic to delegate has been moved up to the abstract `DelegatingRequestHandler` in `handleRequest(). Delegating down the chain is no longer the responsibility of the concrete handlers.
* Though `DelegatingRequestHandler.handleRequest()` contains the delegating logic, it still depends two abstract methods, `iCanProcessRequest()` and`processRequest()`, to handle concrete behavior specifics. Each concrete handler must implement both of these methods.
* `ConcreteRequestHandlerA` and `ConcreteRequestHandlerB` each implement `iCanProcessRequest()` and`processRequest()` as mentioned above, but relative to __A__ and __B__ respectively. The concrete handlers don’t even know they are part of a chain. Their only concerns are whether they can handle the request and handling the request.
* `DelegatingRequestHandler.handleRequest()` only requires two test cases:
    * Confirm that `processRequest()` is executed, when `iCanProcessRequest()` returns true.
    * Confirm that `requestHandler.requestHandler()` is executed, when `iCanProcessRequest()` returns false.
* I think that `DelegatingRequestHandler.handleRequest()` is the first design pattern we’ve seen that contains an `if` statement. Most design patterns don’t have high [Cyclomatic Complexity,](https://en.wikipedia.org/wiki/Cyclomatic_complexity) which is one reason I think they support a good design.
* `DelegatingRequestHandler.handleRequest()` is declared as `final` so that a concrete handler cannot override it, even if unintentionally.
* `DelegatingRequestHandler`’s `requestHandler` field attribute is described as __Non-Null__. This ensures that when referenced, it won’t throw an `NullPointerException`. I do not show how this __Non-Nullness__ can be achieved since there was insufficient space to present it. If I had the space, the `DelegatingRequestHandler` would define a constructor with a `RequestHandler` parameter. The constructor would throw an exception if it were null.
* `DelegatingRequestHandler`’s non-null `requestHandler` constraint means that each linked list must end with a non-delegating handler, which in this design is `AnchoringRequestHandler`. There’s no guarantee that any request handler will be able to handle the request and `AnchoringRequestHandler` handles this scenario. When the flow reaches `AnchoringRequestHandler` then the request has gone unhandled. `AnchoringRequestHandler` serves the same function as the `default` in a `switch` statement or the `else` of a cascading set of `if/else-if` statements. Default behavior will vary based upon context. I’ve provided a few options as comments within the design.

<img src="/assets/ChainOfResponsibilityTemplateMethod.png" alt="Chain of Responsibility using Template Method" width = "90%" align="center" style="padding-right: 20px;">

Here’s an example of the linked list of objects that can be created for this design, which is the same as the objects in the GoF’s CoR example, but with the addition of `anchoringRequestHandler` at the end of the chain:

<img src="/assets/ChainOfResponsibilityTemplateMethodObjects.png" alt="Chain of Responsibility via GoF with Objects" width = "65%" align="center" style="padding-right: 20px;">
 
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

## Chaining Data Sources
<img src="https://live.staticflickr.com/3470/3254883191_f555a28366_b.jpg" alt="Little Black Book" title="Image Source: https://www.flickr.com/photos/84609865@N00/3254883191/" width = "30%" align="right" style="padding-right: 20px;">

While I had never used Chain of Responsibility, it was the first idea that came to my mind. I went through several design iterations before settling upon something like this design:
 
* I called it an `AddressBook`. I always envisioned a swinging bachelor from the 1960s who might look a phone number for a weekend date by first consulting his __little black book__ and then the phone book and finally call directory assistance as his options thinned out.
* I’m calling the organizational entity a __Group__. It’s a generic placeholder for this example. It can represent any Group represented by its name at any place in the organizational hierarchy.
* The `AddressBook` returns an `Optional<Group>` upon a look up rather than `Group`, since it’s possible that there may not be a Group by that name in the organization.
* `AddressBookHandler` is where most of the design resides. It’s using Template Method, but it’s slightly different than what was shown with the generic CoR Template Method example above:
    * There is no separate `iCanProcessRequest()` and `processRequest()` methods. They are both represented in ` Optional<Group> getGroup(String name)`:
    * When `Optional<Group>` is not empty, then the request has been completed, and the `Group` can be returned
    * When `Optional<Group>` is empty, then delegate the request to the next `AddressBook` and return whatever it returns, even if empty.
    * When delegating to the next `AddressBook` and it returns an actual `Group`, then this design takes the opportunity to add the missing `Group` into the current `AddressBookHandler`. This is not part of the GoF’s CoR design, but I have included it mostly to populate the cache. This additional behavior to `addGroup` is the [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) design pattern.
* `AddressBookHandler` has three paths of execution that need to be tested:
   * When `getGroup(name)` finds and returns a `Group`.
   * When `getGroup(name)` does not find and returns an empty `Optional`, and then …:
      * `addressBookHandler.getGroupByName(name)` finds and returns a `Group`.
      * `addressBookHandler.getGroupByName(name)` does not find and returns an empty `Optional`.
* `GroupNotFound` returns an empty `Optional` in all cases as the default behavior when none of the `AddressBookHandlers` are able to find a `Group` by name.

<img src="/assets/ChainOfResponsibilityAddressBookA.png" alt="Address Book via Chain of Responsibility" width = "90%" align="center" style="padding-right: 20px;">

## Concrete AddressBookHandlers
I can’t quite fit the entire design on one diagram, since the `AddressBookHandler` details required considerable space. Here’s the second part of the design, which provides some additional design details for the concrete AddressBooks:
* This diagram replicates the AddressBook classes from the previous diagram, but it focuses more upon the concrete AddressBooks.
* Each concrete AddressBook implements the protected methods declared in `DelegatingAddressBook`.
* `CacheAddressBook` implements the methods via a `Map`.
* `DataBaseAddressBook` is grossly simplified. There’s insufficient room to represent more. What’s important is that `DataBaseAddressBook` delegates to the external `Database`, possibly via SQL. This delegation is an example of the [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) design pattern.
* `WebServiceAddressBook` is likewise the same as `DataBaseAddressBook`. It’s also grossly simplified due to space constraints. It delegates to the `WebService` possibly via REST. This delegation is also an example of the [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) design pattern.

<img src="/assets/ChainOfResponsibilityAddressBookB.png" alt="Address Book Concrete Classes via Chain of Responsibility" width = "90%" align="center" style="padding-right: 20px;">
 
## Cache Invalidation
The design as shown has an issue with stale cached `Group`s. I mentioned that the organizational hierarchy was dynamic. It could change at any time. As designed, once a `Group` is in the cache, it’s never updated or removed.

We were able to accommodate for this with another design feature, which I’ll describe briefly, but I won’t represent in the design. We were able to subscribe to updates or deletions from the Database and the WebService. When we received a notification that a `Group` had been updated or deleted, we deleted it from the `Cache` if present. A fresh version would be repopulated in the `Cache` upon the next request that referenced it.

The subscription and notification mechanism is the [Observer](https://sourcemaking.com/design_patterns/observer) design pattern, which I’ve not mentioned previously.

And to be technically accurate, our `DataBaseAddressBook` did not add the `Group` to the Database in its `addGroup(String name, Group group)` method. Another organization was responsible for the Database’s content. We only had read-only permission. Therefore the `addGroup(String name, Group group)` method, for which an implementation was required, was an empty method. The same was true for `WebServiceAddressBook.addGroup(String name, Group group)` method for similar reasons.

## Configurer
Composable designs provide the structural potential but not behavior. The behavior must be composed by a `Configurer` type of class.
My PM/CTO had described the possible configurations. They would be:

<img src="/assets/ChainOfResponsibilityAddressBookObjects.png" alt="Address Book via Chain of Responsibility Objects" width = "90%" align="center" style="padding-right: 20px;">
 
My first `Configurer` implementation was based upon building the linked list chain from a configuration String value, such as:
* ADDRESSBOOK_PATH=Cache:DataBase:WebService:GroupNotFound
* ADDRESSBOOK_PATH=Cache:WebService:GroupNotFound
* ADDRESSBOOK_PATH=Cache:DataBase:GroupNotFound

The `Configurer` would split $ADDRESSBOOK_PATH with a colon delimiter, acquire the corresponding object from an `AddressBookFactory`, not shown in the diagram, and chain them together into a linked list.

This worked fine in my testing, but it was not a good production quality solution. Our product was part of a fleet embedded systems for which there could be hundreds if not thousands of entities. Configuring each embedded system correctly was not realistic.
The first obvious observation was that ADDRESSBOOK_PATH always started with Cache and ended with GroupNotFound. The only parts that varied were DataBase and WebService.

Other environment variables implied whether the DataBase or a WebService was available. Therefore, I updated the `Configurer` so that it constructed ADDRESSBOOK_PATH based upon known values and environment variables. The configuration issue evaporated.

## Introducing a new Organization
My PM/CTO had hinted that the `AddressBook` might need to accommodate a sibling organization to the original customer. As I mentioned above, that never materialized.

However, if it were to materialize, then we would have had to consider the following:
* The sibling organization might have different concepts for a `Group`. We might have had to either translate the second organization’s terminology into the existing `Group` or potentially normalize `Group` such that it would work for both organizations possibly with translation for both organizations.
* The linked list chain would either contain `AddressBookHandler`s for both organizations, or if the organization could be ascertained from the `name` argument, then maybe the linked list chain could be constructed upon demand, or there could be two linked list chains, one for each organization, and the corresponding one would be used. It’s flexible enough to consider multiple options.

## Pattern Review
The AddressBook use case features more than just Chain of Responsibility. Design patterns are rarely used in isolation. They often work together when the edge of one blurs into the beginning of another one.

Here’s a quick summary of the design patterns featured or mentioned in this use case:
* Chain of Responsibility the pattern of this blog
* [Template Method](https://jhumelsine.github.io/2023/09/26/template-method-design-pattern.html) to move most of the CoR behavior into the abstract class
* [Decorator](https://jhumelsine.github.io/2024/02/08/decorator-design-pattern.html) to populate the Cache and other Address Book Handlers
* [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) to allow the design to interact with external entities
* [Observer](https://sourcemaking.com/design_patterns/observer) to keep the Cache up to date, but only mentioned this blog, not featured

# Chain of Responsibility Pros and Cons
The relative pros and cons of Chain of Responsibility are like those with most of the Composable design patterns.

## Cons
Composable behavior may be more difficult to diagnose with Chain of Responsibility. While it should be simple to understand individual concrete classes in a CoR design, the responsibility to orchestrate observable behavior resides in the `Configurer` in composing the objects from those classes correctly.
Integration or acceptance tests may be needed to confirm that desired behavior emerges when the objects are composed.

## Pros
Chain of Responsibility’s composable behavior is also a pro. Behavior is not statically locked into place as is the case with `switch` or `if/else-if/else` statements. When a new responsibility chain is desired, it may be a relatively simple matter of a new composition of objects instantiated from the existing handler classes in the design.

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
