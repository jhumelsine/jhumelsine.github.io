---
title: DRAFT – Software Contracts
description: Specifying the obligations and expectations between client and provider
unlisted: true
---
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ed/Sales_contract_Shuruppak_Louvre_AO3766.jpg/953px-Sales_contract_Shuruppak_Louvre_AO3766.jpg" alt="Sumarian Cuniform Contract" title="Image Source: https://en.wikipedia.org/wiki/File:Sales_contract_Shuruppak_Louvre_AO3766.jpg" width = "50%" align="center" style="padding: 35px;">

# Introduction
[Contracts](https://en.wikipedia.org/wiki/Contract) have been around for a long time. The image above is a Sumerian contract circa 2600 BCE specifying the terms of selling a field and house.

Real world contracts, such as the one above, specify the legally binding __Obligations__  and __Expectations__ for a specified time between client and provider. Lawyers are often involved.

# Software Contracts
___Software Contracts___ allow provider developers of a software component to communicate component behaviors effectively to the client developers who will invoke the providers’ software components. Good communication among developers through their code is core to Software Engineering.

Software Contracts specify the __Obligations__  and  __Expectations__ of behaviors provided by software components. Software Contracts document the context of __what__ the service provider has to offer and its use without conveying the means of __how__ it implements that offer.

## Contract by Design
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/e/ea/Design_by_contract.svg/768px-Design_by_contract.svg.png" alt="Contract by Design" title="Image Source: https://en.wikipedia.org/wiki/File:Design_by_contract.svg" width = "40%" align="right" style="padding: 35px;">
 
[Bertrand Meyer](https://en.wikipedia.org/wiki/Bertrand_Meyer) created [Contract by Design](https://en.wikipedia.org/wiki/Design_by_contract) (DbC) in conjunction with the [Eiffel Programming Language](https://en.wikipedia.org/wiki/Eiffel_(programming_language)).

Contract by Design includes:
* Preconditions
* Postconditions
* Side effects

## Obligations
Clients bear some responsibility in the relationship. To fulfill a contract, the Client is _obligated_ to provide accurate and valid information to the Service Provider. The contract should define what qualifies as accurate and valid.

If the Client does not provide accurate and valid information, then the Service Provider can refuse to honor the contract request. For example, if the Service Provider provides a query feature where a phone number is the query key, then if the Client does not provide a valid phone number, then the Service Provider can reject the query, possibly by throwing an exception.

Accepting inaccurate or invalid information and attempting to honor the contract produces [Garbage In, Garbage Out](https://en.wikipedia.org/wiki/Garbage_in,_garbage_out) (GIGO) results.

Client Obligations align with Preconditions in the DbC above.

## Expectations
If accurate and valid information has been provided, then the Client has the __expectation__ that the Service Provider will fulfill/honor the contract. That is, the Service Provider will do __what__ has been documented in the contract. For example, if the Client has provided a valid phone number, then the Client expects the query Service Provider to return information corresponding to that valid phone number or an indication that there is no information corresponding to that phone number.

The contract does not define the means that the Service Provider will use to fulfill that expectation.

Client Expectations align with output values returned, postconditions and side effects in the DbC above.

## Real World Contract Example
Taxes are complicated in the United States. Many Americans seek assistance from tax professionals. All tax professionals support the same basic contract:
* Obligation from Clients – Provide your tax paperwork for the year and payment.
* Expectation to Clients – Return completed tax forms for client signature and filing.

Tax professionals come in many forms with different degrees of sophistication, such as: Tax Attorneys, Accountants, Tax Preparation Companies (H&R Block), Tax Preparation Software (Intuit), or a trusted friend or family member. Each might employ different means to complete the forms, such as: Proprietary Software, Deferring to a commercial Tax Software Package or filling the forms out by hand. Regardless of their means, they honor the contract.

## Software Contract Scope
Real world contracts vary in scope, such as the contracts for:
* The merger of multi-billion-dollar corporations
* Buying or selling a house
* Terms of Service for an online service
* A personal I.O.U.

The scope of software contracts can be broad or narrow. Software contracts connect Client and Provider across various spans, such as:
* APIs, for example: [RESTful](https://en.wikipedia.org/wiki/REST)
* Interfaces, for example: Java packages
* Function/Method signatures including public, protected and private

Regardless of scope, the principles of a good contract specify the contract’s obligations and expectations in a meaningful way.

## What vs How
As mentioned above, the contract specifies the __what__ of the contract, while __how__ it is implemented is encapsulated within the code. It’s possible for multiple implementations to satisfy the same contract in different ways. This is the essence of the [Strategy Design Pattern](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html)

# Client Point of View
<img src="https://i.thriftbooks.com/api/imagehandler/m/B326C820FF54B9E2A477B457FEAB08BF30C18D18.jpeg" alt="The Design of Everyday Things" title="Image Source: https://www.thriftbooks.com/w/the-design-of-everyday-things_donald-a-norman/246934/item/3437580/#edition=2402022&idiq=4046535" width = "30%" align="right" style="padding: 35px;">

Cognitive scientist, [Donald Norman](https://en.wikipedia.org/wiki/Don_Norman), is one of the pioneers of User Centric Design. He developed a theory that focuses upon the user’s experience. He applied this to everyday objects, in his book, [_The Design of Everyday Things_](https://en.wikipedia.org/wiki/The_Design_of_Everyday_Things), but the concepts for contract interfaces too. Doors are one of his primary examples, which have been coined [Norman Doors](https://99percentinvisible.org/article/norman-doors-dont-know-whether-push-pull-blame-design/).

Opening a door should be obvious, but that’s not always the case:
>_When a device as simple as a door has to come with an instruction manual—even a one-word manual—then it is a failure, poorly designed._ — [Donald Norman](https://www.azquotes.com/quote/802024)

Norman described situations where the door designer hid all visible clues for aesthetic purposes. Users would stand in front of these doors bewildered by how to enter.

I can identify with this to some degree. My last office, [Bellworks](https://bell.works/new-jersey/explore/), which interestingly enough is the stand in for [Lumon Industries](https://lumon.industries/) in the TV show [Severance](https://en.wikipedia.org/wiki/Severance_(TV_series)), had funky doors. All doors were glass with a meter long vertical bar. All doors basically looked alike, but some were push, some where pull and some where slide. I eventually memorized through trial and error which door did what. But sometimes I’d get it work. I’d push, then I’ll pull and finally I’d slide.

Well-designed things scream how they are intended to be used. Contracts should be designed from the client’s point of view. Ideally the names chosen will reflect how the client interacts with the contract and not necessarily what the provider provides.

If contracts contain a good naming design, then their use within the client code should feel natural. That is, it should feel like the contract was designed specifically for the client’s domain. The class, methods and parameters of a contract should be self-explanatory. Ideally you user should not need to read your documentation or comments to use your contract. See Donald Norman’s quote above. That documentation and those comments won’t appear in client code that references your contract. Ideally, anyone reading the client code should not have to pause and look up your documentation or comments when encountering your contract elements when embedded within production code.

Without care, a contract tends to reflect what was implemented rather than what the client wants or needs. Contracts feel like an afterthought, and that’s why so many of them are difficult to use. Worse yet they leak implementation details, known as a [Leaky Abstraction](https://jhumelsine.github.io/2023/11/13/hexagonal-architecture-clean-architecture.html#leaky-abstraction).

What are some elements of a good contract:
* Designed from the user’s point of view to support their intent
* The elements of the contract fit together as a cohesive whole. Add something, and it’s out of place. Remove something, and the remaining elements fall apart.
* Observes the [Principle of Least Astonishment](https://en.wikipedia.org/wiki/Principle_of_least_astonishment). The elements of the contract should behave in a way that most users will expect it to behave, and therefore it will not astonish or surprise users.
* Prefers Value Objects over Primitives:
* [Primitive Obsession](https://sourcemaking.com/refactoring/smells/primitive-obsession}
* [Value Object](https://en.wikipedia.org/wiki/Value_object)

<img src="https://imgs.xkcd.com/comics/workflow_2x.png" alt="Heating Spacebar xkcd Comic" title="Image Source: https://xkcd.com/1172/" width = "35%" align="right" style="padding: 35px;">

Keep contracts lean. Less is more. You can always add more later without breaking existing users. But once you remove something or change its behavior, then this will affect users. Sometimes we may want to deprecate parts of contracts too.

Once a contract is in general use, you can’t remove elements or change behaviors. [Hyrum’s Law](https://www.hyrumslaw.com/) predicts that with a sufficient number of API users, all observable behaviors of your system will become a dependency for somebody regardless of your intent.
 
Contracts may need versions, which is common with RESTful APIs.

# Behavior-Driven/Test-Driven Development
How do we know that our contracts meet our users’ needs? We become the first users of our contracts using [BDD](https://jhumelsine.github.io/2024/08/08/bdd.html) and [TDD](https://jhumelsine.github.io/2024/07/15/tdd.html) techniques.

We want our contracts to make sense to our users. BDD and TDD techniques allow us to specify contract expectations and obligations in a confirmable executable form. If it’s difficult to write tests interacting with your contract, then it will be difficult for your user to. Adjust the contract so that it is easier for your tests and for your users.

Traditionally BDD and TDD confirm behavior within code, but this case is a bit different. There is no code associated with a contract. 

Since contracts do not depend upon their implementations, contract specification tests will tend to be _higher_ test possibly an Acceptance Test (AT) (AT Blog TBD). While ATs still follow the principles of BDD and TDD, they may feel more like complete user scenarios. This can be useful to get a better understanding of how the user may interact with the contract rather than being concerned about the implementation of the contract.

Contracts are [Fixed or Stable Elements](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#stable-or-fixed-design-elements) in most designs. In UML class diagrams other design elements, such as client classes and implementation classes, depend upon the contracts rather than the contracts depending upon those classes. When contracts have their own dependencies, they tend to be other contracts.

What code is being executed in a test that confirms the expectations and obligations? This is going to sound counter intuitive, but there is no code that’s being executed. There is no implementation for the contract. The _code_ being executed will be [Test Doubles](https://jhumelsine.github.io/2024/07/02/test-doubles.html).

This is not a case of testing test doubles. This is a case of emulating the contract’s expectation and obligation behaviors via test doubles so that the tests not only specify contract behaviors but demonstrate them in an executable form.

These contract tests may serve an additional function. They can be the basis for tests against the implementation. Rather than providing test double references for the contracts, provide the actual implementations. Some of the set up in the _Given_ portion of the test and confirmation in the _Then_ portion of the test may need to be updated for the implementation, but the overall scenario should be the same.

__True Confessions:__ I have not written executable tests against my contracts as I advocated above. However, before I learned TDD and BDD I wrote non-executable operational scenarios against my contracts to visualize how they would be used. They became the basis of my tests once I started to implement the code.

# Summary
__TBD__

# References
 __TBD__
•	https://en.wikipedia.org/wiki/Contract
•	https://xkcd.com/1172/
•	https://imgs.xkcd.com/comics/workflow_2x.png
•	https://99percentinvisible.org/article/norman-doors-dont-know-whether-push-pull-blame-design/
•	https://www.azquotes.com/quote/802024#google_vignette
•	https://www.google.com/search?sca_esv=0c50508b885de737&rlz=1C1EKKP_enUS733US733&sxsrf=AHTn8zp-_URf1GHV0CL15YVjt1LHSHEUJA:1744402744991&q=norman+doors&udm=2&fbs=ABzOT_CWdhQLP1FcmU5B0fn3xuWpA-dk4wpBWOGsoR7DG5zJBtmuEdhfywyzhendkLDnhcrUz6wxBwARHD96EKWkSbZoQZGasaHPJ9csj0AVVVUDNHqfR7gd1arUfaOpw1v5Icccwayh65rdsqdiyPvxAA9gXK95YqHnUzfZ5jo9jiMl2Q8DaMUR4I1U0kl1-ho1NSBjy_chexdcGuJmvrFewYJaqjljog&sa=X&sqi=2&ved=2ahUKEwiIh9fP5tCMAxXgFVkFHbcAElkQtKgLegQIExAB&biw=1438&bih=598&dpr=1.25
•	https://www.google.com/search?sca_esv=43140f9f49dcfdc2&rlz=1C1EKKP_enUS733US733&sxsrf=AHTn8zqdwx2uFTv6am26G3WUjR0dmp6tew:1744461759205&q=norman+doors&udm=2&fbs=ABzOT_CWdhQLP1FcmU5B0fn3xuWpA-dk4wpBWOGsoR7DG5zJBtmuEdhfywyzhendkLDnhcrUz6wxBwARHD96EKWkSbZoQZGasaHPJ9csj0AVVVUDNHqfR7gd1arUfaOpw1v5Icccwayh65rdsqdiyPvxAA9gXK95YqHnUzfZ5jo9jiMl2Q8DaMUR4I1U0kl1-ho1NSBjy_chexdcGuJmvrFewYJaqjljog&sa=X&sqi=2&ved=2ahUKEwif9-y7wtKMAxWtMmIAHaMyFnQQtKgLegQIFRAB&biw=1536&bih=738&dpr=1.25
•	https://www.google.com/search?q=the+design+of+everyday+things&rlz=1C1EKKP_enUS733US733&oq=The+Design+of+Everyday+Things&gs_lcrp=EgZjaHJvbWUqEggAEAAYQxjjAhixAxiABBiKBTISCAAQABhDGOMCGLEDGIAEGIoFMg8IARAuGEMYsQMYgAQYigUyBwgCEAAYgAQyBwgDEAAYgAQyBwgEEAAYgAQyBwgFEAAYgAQyBwgGEAAYgAQyBwgHEAAYgAQyBwgIEAAYgAQyBwgJEAAYgATSAQg5NTg4ajBqN6gCCLACAfEF_5d_ye_zH5Q&sourceid=chrome&ie=UTF-8
•	https://en.wikipedia.org/wiki/The_Design_of_Everyday_Things
•	https://www.goodreads.com/work/quotes/18518-the-psychology-of-everyday-things
•	https://en.wikipedia.org/wiki/REST
•	https://en.wikipedia.org/wiki/Garbage_in,_garbage_out
