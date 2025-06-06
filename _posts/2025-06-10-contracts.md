---
title: DRAFT – Software Contracts
description: Specifying the obligations and expectations between client and provider
unlisted: true
---
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ed/Sales_contract_Shuruppak_Louvre_AO3766.jpg/953px-Sales_contract_Shuruppak_Louvre_AO3766.jpg" alt="Sumerian Cuneiform Contract" title="Image Source: https://en.wikipedia.org/wiki/File:Sales_contract_Shuruppak_Louvre_AO3766.jpg" width = "50%" align="center" style="padding: 35px;">

# Introduction
[Contracts](https://en.wikipedia.org/wiki/Contract) have existed for millennia, shaping agreements across civilizations. The image above is a Sumerian contract circa 2600 BCE specifying the terms of selling a field and house.

Real world contracts, such as the one above, specify the legally binding __Obligations__  and __Expectations__ for a specified time between Client and Provider.

# Software Contracts
___Software Contracts___ allow developers of a software component to communicate component behaviors effectively to other developers who will invoke their software components. Good communication among developers through their code is core to Software Engineering.

Software Contracts specify the __Obligations__  and  __Expectations__ of behaviors of software components independent of the implementation.

## Contract by Design
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/Design_by_contract.svg/768px-Design_by_contract.svg.png" alt="Contract by Design" title="Image Source: https://en.wikipedia.org/wiki/File:Design_by_contract.svg" width = "40%" align="right" style="padding: 35px;">
 
[Bertrand Meyer](https://en.wikipedia.org/wiki/Bertrand_Meyer) created [Contract by Design](https://en.wikipedia.org/wiki/Design_by_contract) (DbC) as a concept for the [Eiffel Programming Language](https://en.wikipedia.org/wiki/Eiffel_(programming_language)).

Contract by Design includes:
* Preconditions: Valid Input
* Postconditions: Expected Output
* Side Effects: State Changes

## Obligations
Clients bear some responsibility in the relationship. To fulfill a contract, the Client is _obligated_ to provide accurate and valid information to the Service Provider. The contract should define what qualifies as accurate and valid so that Clients know their obligations.

If the Client fails to provide valid input (e.g., an invalid phone number), the Service Provider is not required to fulfill the request. Instead, the request can be rejected—possibly by throwing an exception.

Accepting inaccurate or invalid information and attempting to honor the contract produces [Garbage In, Garbage Out](https://en.wikipedia.org/wiki/Garbage_in,_garbage_out) results.

Client Obligations align with Preconditions in the DbC above.

## Expectations
If accurate and valid information has been provided, then the Client has the __expectation__ that the contract will be fulfilled/honored. That is, the software component will do what has been documented in the contract. For example, if the Client has provided a valid phone number, then the Client expects either the corresponding result or a clear indication that no data exists.

The contract does not define the means that will fulfill that expectation.

Client Expectations align with output values returned, postconditions and side effects in the DbC above.

## Real World Contract Example
Taxes are complicated in the United States. Many Americans seek assistance from tax professionals. All tax professionals support the same basic contract:
* Client Obligation: Provide tax documents and payment
* Client Expectation: Receive completed tax forms, ready for signature and filing

Tax professionals come in many forms with different degrees of sophistication, such as: Tax Attorneys, Certified Public Accountants, Tax Preparation Companies ([H&R Block](https://www.hrblock.com/)), Tax Preparation Software ([Intuit](https://www.intuit.com/)), or a trusted friend or family member. Each might employ different means to complete the forms, such as: Proprietary Software, deferring to a commercial Tax Software Package or filling the forms out by hand. Regardless of their means, they honor the contract.

## Software Contract Scope
Real world contracts vary in scope, such as the contracts for:
* Mergers of multi-billion-dollar corporations
* Buying or selling a house
* Terms of Service for an online service
* A personal I.O.U.

The scope of software contracts can be broad or narrow. Software contracts connect Client and Provider across various spans, such as:
* APIs, for example: [RESTful](https://en.wikipedia.org/wiki/REST)
* Interfaces, for example: Java packages
* Function/Method signatures including public, protected and private

Regardless of scope, the principles of a good contract specify the contract’s obligations and expectations in a meaningful way.

## What vs How
A well-crafted software contract defines __what__ a component does, not __how__ it does it. This distinction is critical: the contract outlines the expected behavior—inputs, outputs, and side effects—while deliberately hiding implementation details. By decoupling the _interface_ from the _implementation_, contracts enable flexibility, testability, and the possibility of multiple solutions that fulfill the same promises. Whether sorting data or retrieving a user record, the contract ensures consistency of outcome, allowing the underlying logic to evolve without breaking client expectations.

This separation is the essence of the [Strategy Design Pattern](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html).

# Client Point of View
<img src="https://images-na.ssl-images-amazon.com/images/S/compressed.photo.goodreads.com/books/1386924139i/16867.jpg" alt="The Design of Everyday Things" title="Image Source: https://www.goodreads.com/book/show/16867" width = "30%" align="right" style="padding: 35px;">

Cognitive scientist [Donald Norman](https://en.wikipedia.org/wiki/Don_Norman) is one of the pioneers of [User Centered Design](https://en.wikipedia.org/wiki/User-centered_design). He developed a theory that focuses upon the user’s experience. He applied this to everyday objects, in his book, [_The Design of Everyday Things_](https://en.wikipedia.org/wiki/The_Design_of_Everyday_Things), but the concepts apply to software contract too. Doors are one of his primary examples, which have been coined [Norman Doors](https://99percentinvisible.org/article/norman-doors-dont-know-whether-push-pull-blame-design/).

Opening a door should be obvious, but that’s not always the case:
>_When a device as simple as a door has to come with an instruction manual—even a one-word manual—then it is a failure, poorly designed._ — [Donald Norman](https://www.azquotes.com/quote/802024)

Norman described situations where the door designer hid all visible clues to operate doors for aesthetic purposes. Users would stand in front of these doors bewildered by how to open them.

I can identify with this to some degree. My last office location, [Bellworks](https://bell.works/new-jersey/explore/), which interestingly enough is the filming location for the exteriors for [Lumon Industries](https://lumon.industries/) in the TV show [_Severance_](https://en.wikipedia.org/wiki/Severance_(TV_series)), had Norman doors. All doors were glass with a meter long vertical bar attached on one side. All doors looked alike; some were push, some were pull and some were slide. I eventually memorized which door did what through trial and error. But sometimes I’d absent mindedly forget. I’d push, then pull, and finally slide.

Well-designed things scream how they are intended to be used. Contracts should be designed from the client’s point of view. Ideally their chosen names will reflect how the client interacts with the contract and not necessarily what the provider provides.

If contracts contain a good naming design, then their use within the client code should feel natural. That is, it should feel like the contract was designed specifically for the client’s domain. The class, methods and parameters of a contract should be self-explanatory.

Ideally your user should not need to read the documentation or comments to use your contract. This won't always be the case, but hopefully your contract names are expressive enough that someone reading code that accesses your contract can understand it in how it's being used. Documentation and comments won’t appear in client code that references your contract. Ideally, anyone reading the client code should not have to pause and look up your documentation or comments when encountering your referended contract elements within the production code to understand it.

Without care, a contract tends to reflect what was implemented rather than what the client wants or needs. Contracts feel like an afterthought, and that’s one reason why so many of them are difficult to use. Worse yet they may leak implementation details, known as a [Leaky Abstraction](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html#leaky-abstraction).

Here are some elements of a good contract:
* It is designed from the user’s point of view to support their intent.
* Its elements form a cohesive whole. Add something, and it’s out of place. Remove something, and the remaining elements fall apart.
* It observes the [Principle of Least Astonishment](https://en.wikipedia.org/wiki/Principle_of_least_astonishment), a principle stating that software should behave in a way users intuitively expect. That is, no surprises or _astonishment_ when its actual behavior is understood.
* It prefers Value Objects over Primitives:
    * [Primitive Obsession](https://sourcemaking.com/refactoring/smells/primitive-obsession)
    * [Value Object](https://en.wikipedia.org/wiki/Value_object)

<img src="https://imgs.xkcd.com/comics/workflow_2x.png" alt="Heating Spacebar xkcd Comic" title="Image Source: https://xkcd.com/1172/" width = "35%" align="right" style="padding: 35px;">

Keep contracts lean. Less is more. You can always add more later without breaking existing users. But once you remove something or change its behavior, it will affect existing users. Sometimes we may want to deprecate parts of contracts but do this cautiously. Once a contract is in general use, you can’t easily remove elements or change behaviors. [Hyrum’s Law](https://www.hyrumslaw.com/) predicts that with enough users, all observable behaviors of your system will become a dependency for somebody regardless of the developer’s intent.
 
Contracts may need versions to support updates or changes. This is common with RESTful APIs.

# Behavior-Driven/Test-Driven Development
How do we know that our contracts meet our users’ needs? Using [BDD](https://jhumelsine.github.io/2024/08/08/bdd.html) and [TDD](https://jhumelsine.github.io/2024/07/15/tdd.html) techniques, we act as the first users of our contracts.

We want our contracts to make sense to our users. BDD and TDD techniques allow us to specify contract expectations and obligations in a confirmable executable form. If it’s difficult for you to write tests interacting with your contract, then it will be more difficult for your users. Adjust the contract so that it is easier for your tests and for your users.

Traditionally BDD and TDD confirm behavior within code, but this case is a bit different. There is no code associated with a contract. A contract only specifies Obligations and Expectations. It doesn't implement them. This also allows you to define the contract before implementation details may influence it.

Since contracts do not depend upon their implementations, contract specification tests will tend to be _higher_ tests, possibly Acceptance Tests (AT) (Blog TBD). While ATs still follow the principles of BDD and TDD, they may feel more like complete user scenarios. This can be useful to gain a better understanding of how the user may interact with the contract rather than being concerned about the implementation of the contract.

Contracts are [Fixed/Stable Elements](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#stable-or-fixed-design-elements) in most designs. In UML class diagrams other design elements depend upon the contracts rather than the contracts depending upon those elements. This is visually represented by the dependency arrowheads modeling the relationship between elements consistently pointing inward toward contract elements and rarely away from them. The only time contracts depend upon other elements is when they depend upon other contracts. For example, Java Interfaces can only extend other Interfaces.

__NOTE:__ Technically interfaces depend upon their parameters, which can be other classes, but these tend to be core Business Elements that are implemented as Value Objects.

What code is being executed in contract tests that confirm expectations and obligations? This is going to sound counter intuitive, but there is no code that’s being executed. There is no implementation for the contract. The _code_ being executed will be [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html).

This is not a case of testing test doubles. This is a case of emulating the contract’s expectation and obligation behaviors via test doubles so that the tests not only specify contract behaviors but demonstrate them in an executable form.

These contract tests may serve an additional function. They can be the basis for future tests against the implementation. Rather than providing test double references for the contracts, provide an actual implementation. Some of the set up in the _Given_ portion of the test and confirmation in the _Then_ portion of the test may need to be updated for the implementation, but the overall scenario should be the same.

__Personal Note:__ I have not written executable tests against my contracts as I advocated above. However, before I learned TDD and BDD I wrote non-executable operational scenarios against my contracts to visualize how they would be used. They became the basis of my tests once I started to implement the code.

# Summary
Software development is, at its best, a discipline of communication—between people, between components, and across time. By thinking in terms of contracts, we make that communication explicit. We name what we expect and what we offer in return.

Software contracts define the boundaries between components, clarify behavior, and reduce ambiguity. Contracts lead to more intentional, testable, and maintainable code. Every function becomes a promise; every call, a handshake. Embracing contracts invites more resilient systems.

# References
 * [Donald Norman Quotations](https://www.goodreads.com/author/quotes/552.Donald_A_Norman)

