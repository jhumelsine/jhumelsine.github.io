---
title: Test Doubles
description: Emulate dependencies without depending upon dependencies
unlisted: true
---

# Introduction
I’ve used the term ___Test Double___ in several previous blogs with the [promise]( https://jhumelsine.github.io/2024/06/23/unt-test-elements.html#set-up--ie-arrangegiven) to provide more details:

> To isolate the SUT from its dependencies, we override these dependencies with Test Doubles. __I’ll provide more details about Test Doubles in the next blog__. For now, a Test Double emulates dependency behavior with the [SUT](https://en.wikipedia.org/wiki/Test_double) being none to the wiser. Test Doubles tend to be small snippets of code that emulate a specific dependency behavior that’s only needed within the context of each test.

This is where I follow up upon that promise.

# Test Double Raison D'être
Why do we use Test Doubles? Why don’t we test the code as is? Once more I’ll reference my previous blog entry:

> We replace production dependencies with Test Doubles mostly because Test Doubles tend to be easier to configure and execute faster in the test than the production dependencies. We also have complete control over dependency behaviors via Test Doubles. It may be very difficult to force behavior in a production dependency. For example, how challenging would it be to force a production dependency to throw a specific exception, such as `OutOfMemoryError`, consistently on demand?

In short:
* Configuration convenience
* Test execution speed
* Complete control

The Test Double is not part of the SUT. It is an element of the test configuration that supports the SUT by taking the place of the actual dependency. It’s easy to confuse what’s part of the SUT and what’s a Test Double. I swear that I’ve seen tests that were confirming Test Double behavior.

The term _Test Double_ is inspired by the term _Stunt Double_. A Stunt Double is a replacement for an actor in a TV show or movie when the scene is too dangerous for the actor. The Stunt Double is a temporary substitute, who mostly looks like the actor but has special skills. A Test Double is a temporary substitute, that mostly emulates a dependency but has special skills.

<img src="/assets/StuntDoubles.jpeg" alt="Stunt Doubles" align="center" width = "50%" style="padding-right: 20px;">
 
# Test Double Worthy Dependencies
Almost all software has dependencies, but not all dependencies require Test Doubles in testing. There are no absolute rules. The same dependency may require a Test Double in some tests and none in other tests.
The decision is often based upon what I listed above: configuration, speed and control.

## Dependencies That Do Not Tend To Require Test Doubles
Programming language features and language utilities don’t tend to require Test Doubles. I’ve never provided a Test Double for `String` even if it is technically a dependency.

[Value Objects]( https://en.wikipedia.org/wiki/Value_object), [Data Transfer Objects](https://en.wikipedia.org/wiki/Data_transfer_object) (DTO), and other relatively small and isolated classes don’t tend to require Test Doubles. They usually require no configuration, they’re fast and they have no external dependencies.

## Dependencies That Do Tend To Require Test Doubles
External dependencies, such as databases, file systems, internet calls, time, etc. tend to require Test Doubles.

## Project Component Dependencies Can Go Either Way
Dependencies upon other components in the project may or may not require Test Doubles. It depends upon the scenario.

Project components that are relatively small and isolated don’t tend to require Test Doubles. Using them as-is confirms more of your system but may require more debugging when tests fail.

Project components that are more complex or have their own external dependencies may require Test Doubles.

# As Testing Scope Changes, Test Double Needs Change
A project component may require a Test Double in unit testing, but it may not require a Test Double in integration testing. Lower-level/unit testing is more narrowly scoped so that we can confirm an individual component. Test Doubles help define that boundary. Higher-level/integration testing is more broadly scoped so that we can confirm that the configuration and assembly of components work together. These tests will tend to use the project component dependencies rather than their Test Doubles.

However, higher-level testing will still probably include Test Doubles. We won’t jump from unit testing individual components to testing the entire system in one hop. Higher-level tests confirm larger parts of the system together, but not the whole system. The larger parts still have boundaries, and Test Doubles will complete the testing environment for those larger boundaries as well.

# Test Double Example
A Test Double is a special case of the [Strategy Design Pattern](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html). A Test Double is a specific Strategy implementation, but with the distinction that it exists only for testing purposes. A Test Double even made a cameo appearance in the [Testing](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html#Testing) section with:

<img src="/assets/ShapeTesting.png" alt="Shape Testing UML Class Diagram" align="center" width = "95%" style="padding-right: 20px;">

Let’s consider how this diagram from nine months ago foreshadowed concepts that I’m writing about now:
* `ComputerAidedDesign` is the SUT. This test confirms that its `render()` method will drawn any `Shape`s that have been added to it.
* `TestDoubleShape` is a Spy, as mentioned in the [previous blog]( https://jhumelsine.github.io/2024/06/23/unt-test-elements.html# confirmation--ie-assertthen). It implements `Shape.draw()`, but it doesn’t actually draw anything. Its only behavior is to remember that its `draw()` method has been called and provides a means, `isDrawn()`, to report that information.
* `ComputerAidedDesignTest` is a test class with the `render_DrawsShapes()` method. It uses the __Given/When/Then__ story telling structure that I mentioned in the [previous blog](https://jhumelsine.github.io/2024/06/23/unt-test-elements.html#test-elements) as well:
    * __Given__ that we have a `ComputerAidedDesign` instance with a `TestDoubleShape` added to it …
    * __When__ `render()` is invoked for the `ComputerAidedDesign` instance …
    * __Then__ assert that the added `TestDoubleShape` was drawn. 

Notice that `ComputerAidedDesign` and `Shape` have no arrows pointing away from them. All arrows from purple test related elements to blue application related elements point from purple to blue. This means that the blue boxes have no knowledge nor dependency upon the purple boxes. More details about why this matters is provided in [Hexagonal Architecture – Why it works](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html).

SUT will always be the blue boxes. Test Doubles will always (well almost always) be the purple boxes.

# Test Double Types
There are several types of Test Doubles. Structurally they are all the same. Their distinction is based upon the amount of emulated behavior they provide. Test Double type names haven’t become standardized. You may see the same term used in different ways. For example, ___Mock___ is a term often used for _Test Double_ in general, especially with [Mockito](https://en.wikipedia.org/wiki/Mockito), but it also has a more restrictive meaning within the overall Test Double family.

Here is how I view them. Each Test Double type tends to be an extension of the one before it. When defining Test Doubles, I recommend starting with the least functional Test Double and then extending it as needed.

I’ll use a Customer Service feature in my Test Double examples. Customer Service will have dependencies upon the following interfaces:
```java
interface CustomerRepo {
    Customer getCustomer(CustomerId customerId);
}

interface Authorization {
    boolean isAuthorized(User user);
}
```

Let’s assume that the SUT `CustomerService` only allows authorized `User`s to access customer information. Its API is:
```java
class CustomerService {
    public CustomerService(CustomerRepo customerRepo, Authorization authorization) {…}
    public Customer getCustomer(CustomerId customerId, User user) throws UnauthorizedException {…}
}
```

We want to design tests that confirm authorization without knowing the implementation of `CustomerService`. We only know that it has two dependencies: `CustomerRepo` and `Authorization`.

## Null or Dummy
Sometimes you don’t need anything. Even if the SUT contains dependencies, you may not need to provide Test Doubles. The flow of execution through the SUT may not reference those dependencies, so there’s no need for a Test Double.
A Dummy is an implementation that mostly provides default implementations or an implementation that throws a `NotImplementedException`. Default implementations would be:
* Boolean => false
* String => “”
* int => 0
* float => 0.0
* etc.
In the example below:
* `CustomerRepo` is null since it should not be accessed.
* `Authorization` is a dummy.

```java
@Test
public void getCustomer_ThrowsUnauthorizedException_WhenUserIsNotAuthorized() throws Exception {
    // Given
    Authorization auth = new Authorization() {
        @Override
        public boolean isAuthorized(User user) {
            return false;
        }
    };

    CustomerService customerService = new CustomerService(null, auth);

    try {
        // When
        Customer customer = customerService.getCustomer(new CustomerId("123"), null);
        fail("Expecting thrown UnauthorizedException");
    } catch (UnauthorizedException e) {
        // Then passes
    }
}
```

## Stub
The `Authorization` Dummy above was sufficient for that scenario, but that was more accidental than intentional.
A Stub is one step up from a Dummy. The implementation is minimal, but it tends to be intentional.
Let’s consider a test that uses Stubs. The User is 
```java
@Test
public void getCustomer_ReturnsCustomer_WhenUserIsAuthorized() throws Exception {
    // Given
    Customer persistedCustomer = new Customer();
    CustomerRepo repo = new CustomerRepo() {
        @Override
        public Customer getCustomer(CustomerId customerId) {
            return persistedCustomer;
        }
    };
    
    Authorization auth = new Authorization() {
        @Override
        public boolean isAuthorized(User user) {
            return true;
        }
    };

    CustomerService customerService = new CustomerService(repo, auth);
        
    // When
    Customer foundCustomer = customerService.getCustomer(new CustomerId("123"), null);
        
    // Then
    assertEquals(persistedCustomer, foundCustomer);
}
```

## Mock
The Stubbed test above is a good start, but how good is it? From the two tests, we can assume that `isAuthorized(User user)` has been called, but has it been called for the expected user? It passed even when the `User` is `null`. That feels a bit sketchy.

A Mock adds assertions into the Test Double to confirm invariants. Let’s update the `Authorize` Stub to a Mock to confirm the User and the CustomerId.

```java
@Test
public void getCustomer_ReturnsCustomer_WhenConfirmedUserIsAuthorized() throws Exception {
    Customer persistedCustomer = new Customer();
    User requestingUser = new User();
    CustomerRepo repo = new CustomerRepo() {
        @Override
        public Customer getCustomer(CustomerId customerId) {
            if (!"123".equals(customerId.getId())) {
                fail(String.format("customerId Mismatch! expectedId=123, actualId=%s", customerId.getId()));
            }
            return persistedCustomer;
        }
    };
    
    Authorization auth = new Authorization() {
        @Override
        public boolean isAuthorized(User user) {
            if (user != requestingUser) {
                fail(String.format("User Mismatch! expectedUser=%s, actualUser=%s", requestingUser.toString(), user.toString()));
            }
            return true;
        }
    };

    CustomerService customerService = new CustomerService(repo, auth);
        
    // When
    Customer foundCustomer = customerService.getCustomer(new CustomerId("123"), requestingUser);
        
    // Then
    assertEquals(persistedCustomer, foundCustomer);
}
```

## Spy
The Mock above provides more confidence, but do we really know for sure that `isAuthorized(User user)` has been called for the `User`. The Mocked version of the test only confirms that if `isAuthorized(User user)` is called that it will return `true` for the specific User. We can infer to some degree from the Null/Dummy test above that it was called, but I don’t think it’s definitive enough.
A Spy Test Double can help with this. A Spy Test Double records its interactions and returns them via a query:
Let’s make this test more obvious by upgrading the Test Double to a Spy:
```java
class AuthorizationSpy implements Authorization {
    private User requestingUser;
    private List<String> actions = new LinkedList<>();

    public AuthorizationSpy(User requestingUser) {
        this.requestingUser = requestingUser;
    }

    @Override
    public boolean isAuthorized(User user) {
        actions.add(String.format("isAuthorized with user=%s", user.toString()));
        if (user != requestingUser) {
            fail(String.format("User Mismatch! expectedUser=%s, actualUser=%s", requestingUser.toString(), user.toString()));
        }
        return true;
    }
    
    public List<String> getActions() {
        return actions;
    }
}

@Test
public void getCustomer_ReturnsCustomer_WhenConfirmedUserIsAuthorized() throws Exception {
    // Given
    User requestingUser = new User() {
        @Override
        public String toString() {
            return "requestedUser";
        }
    };

    Customer persistedCustomer = new Customer();
    CustomerRepo repo = new CustomerRepo() {
        @Override
        public Customer getCustomer(CustomerId customerId) {
            if (!"123".equals(customerId.getId())) {
                fail(String.format("customerId Mismatch! expectedId=123, actualId=%s", customerId.getId()));
            }

            return persistedCustomer;
        }
    };
    
    AuthorizationSpy authSpy = new AuthorizationSpy(requestingUser);
    CustomerService customerService = new CustomerService(repo, authSpy);
        
    // When
    Customer foundCustomer = customerService.getCustomer(new CustomerId("123"), requestingUser);
        
    // Then
    assertEquals(persistedCustomer, foundCustomer);
    assertEquals("[isAuthorized with user=requestedUser]", authSpy.getActions().toString());
}
```

There’s more going on with the Spy. I can’t just use an anonymous class as I did with the previous Test Doubles, since the Spy has behavior not defined in `Authorization`. Therefore, I implemented a named class, `AuthorizationSpy` implementing `Authorization`, so that it has a public method from which to acquire the actions recording interactions with it.

For complete confirmation, we should have another test where `isAuthorized(User user)` returns `false` in a Spy to confirm that an `UnauthorizedException` will be thrown. Since this is getting long, I won’t provide the code. It would be a variation on the previous example.

## Fake
A Fake is a Test Double on steroids. Fakes tend to emulate full behavior, but in a manner that won’t work in production. For example, there are Database Fakes that only store data in memory. They provide the complete set of DB behaviors, but as soon as the test is done, the DB Fake fades away.

Since Fakes implement so much behavior, I don’t think they are used often unless it’s a package obtained externally. I doubt that most developers are implementing their own Fakes.

# How Do We Know That Emulated Test Double Behavior Is Accurate?
How do we know that the emulated Test Double behavior accurately represents the dependency’s behavior? A test is only as reliable as the emulated behavior provided by a Test Double.

This feels like a legitimate concern, but I’m going to argue that within the realm of unit testing, it’s not that much of a concern.

The dependency’s behavior should be well defined via contract defined via its interface or public methods. Test Doubles emulating dependency behavior defined via clear and well-defined contracts should not be a major challenge. However, if dependency contracts aren’t clear or well defined, then we have some options:
* Contact the dependency provider for clarification
* Create tests that interact with the dependencies to observe and confirm their behaviors, which we can then emulate in the Test Doubles.

Test Doubles emulate dependency behavior based upon the dependency’s contract. Unit tests confirm SUT functionality and its interactions with its dependencies as defined by its contract via Test Doubles emulating that contract. The unit test does not confirm that the dependencies honor their own contracts. Dependency confirmation is the responsibility of the dependency provider.

It took me a long time to realize this and appreciate the distinction. Before I understood contracts and their implied boundaries, I thought tests could only confirm the whole software rather than using these contracts and boundaries to separate software components so they could be confirmed separately.
Sometimes dependency contracts are vague. The dependency provider may have one thought in mind, whereas the dependency user may have another thought in mind.

Unit testing won’t catch these contract interpretation inconsistencies. This is where integration testing becomes important. I’ll devote a blog to these concerns in the series.

# Test Double Inclusion
The SUT should be unaware of its dependency reference origins so that the SUT functions in production exactly in the same way that it functions in its unit test.
[Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) is probably the most common technique, but it only works when the SUT is loosely coupled with its dependencies.

Here are two examples from the Dependency Injection blog.

Injecting a production dependency, `MyClass`:

<img src="/assets/DependencyInjection.png" alt="Dependency Injection" width = "95%" align="center" style="padding-right: 20px;">
 
The following is the same SUT, but injecting a Test Double, `MyTestDouble`:

<img src="/assets/DependencyInjectionTesting.png" alt="Dependency Injection Testing" width = "95%" align="center" style="padding-right: 20px;">
 
`ClientApplication` and `MyInterface` are the SUT. They are unaware of any dependency specifics beyond the red boundary for which all arrows of knowledge and dependency cross that boundary by pointing toward the SUT and never away from it.

__NOTE:__ When an SUT is not loosely coupled with its dependencies, then it may need to be refactored that will be loosely coupled before Test Doubles can be added.

## Constructor Injection
Constructor Injection is probably the most common technique. I’ve used this technique in my examples above with Test Doubles being injected into the `CustomerService` via a constructor.

[Configurers](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer) manage this. A Production Configurer will inject production dependencies. A Test Configurer, usually the test as well, will inject Test Doubles.

We can have both Production and Test [Configurers](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#configurers) since no other elements in the design tends to depend upon them. In design diagrams, arrows point away from them, not into them. They are invisible to the rest of the design. They are invisible to each other.

Therefore, we can drop the SUT in many different test and production scenarios without their having knowledge or dependency upon those scenarios.

## Method Injection
Method Injection is much like Constructor Injection, except that the dependency is injected via a method, which is not the constructor. Some classes may have a set accessor which allows dependencies to be updated after object construction.

## Dependency Frameworks
Some frameworks, such as the [Spring Framework](https://spring.io/projects/spring-framework), provide mechanisms, such as the [@Autowired](https://www.baeldung.com/spring-autowire) annotation, but they are mostly wrappers to facilitate dependency injection.

## Method Override
Sometimes the dependency is within the SUT class itself, which can happen when the SUT is tightly coupled to its dependencies, which is common with Legacy Code.

I will feature Method Override in the next blog, but I will briefly describe it here:
* Method Override works to decouple tight dependencies in the SUT.
* The dependency may be external, such as a direct call to a DB API. The dependency may be another class in the project. Or the dependency may be code within the method being tested.
* Separate the dependency into its own method using the [Extract Method](https://refactoring.guru/extract-method) refactoring. Most IDEs provide a tool to do most of this for you. The extracted method will probably be declared as `private` by default. We will need to remove this so that it’s package-private. This will keep the method hidden from elements outside the package, but it will be accessible to elements inside the package, specifically the test code. __NOTE:__ This is Java terminology. Other languages may have different terminology.
* When creating the SUT class in the test, override the extracted method and inject the Test Double behavior needed for the test.

Method Override leaks some implementation details into the tests, which makes them a bit more brittle. I use Method Override sparingly, but sometimes it’s the best technique until the SUT can be refactored more so that the dependencies are truly loosely coupled.

# Roll Your Own Test Doubles Or Use A Framework?
My examples in this blog have been hand rolled Test Doubles. This is not your only option. There are Test Double frameworks, such as [Mockito](https://site.mockito.org/).

I created my own Test Doubles as I was learning how to implement unit tests. This gave me the freedom and power to define Test Doubles that did exactly what I wanted them to do. It also helped me distinguish SUT from Test Doubles, which can be a bit confusing when first learning how to implement unit tests. It also provided an environment where I truly learned what I could and could not do with Test Doubles rather than copying some code I found online. However, there was a tradeoff. My own Test Doubles were larger and took more time to implement.

As I gained more confidence, I started to dip my toe into the Mockito waters. I was able to create and inject Test Double behavior more quickly, but it also came with tradeoffs. Mockito Test Double defined behavior can be a bit cryptic. Its specifications are inconsistent. When you make a specification mistake, it often won’t alert you. It just won’t work as you think it should. I spent a lot of time scratching my head staring at my SUT implementation when the real problem was in the Mockito Test Double specification. There were times when I just couldn’t emulate the Test Double behavior, that I wanted. This could have been behavior that Mockito did not support or it could have been Mockito knowledge that I didn’t quite have yet. I’d roll my own Test Double when I couldn’t continue with Mockito.

Even with these tradeoffs, I migrated to Mockito for my Test Doubles and created my own when I had no other known options. Mockito was faster to specify in most cases.

While this is completely subjective, I recommend the same path. Create your own Test Doubles until you gain confidence. Then slowly migrate to a Test Double Framework, such as Mockito.

# Summary
Test Doubles has been the focus of this blog, but there’s something more subtle afoot. Many ideas from previous blogs are coalescing into a common theme and Test Doubles has been a catalyst for that reaction.

Here’s a brief review of some previous ideas with blog references that are coalescing:
* _Program to an interface, not an implementation_ from [Design Pattern Principles](https://jhumelsine.github.io/2023/09/06/design-pattern-principles.html). This principle is crucial for loose coupling.
* [Strategy Design Pattern](https://jhumelsine.github.io/2023/09/21/strategy-design-pattern.html) and to lesser degree [Adapter](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) and [Façade](https://jhumelsine.github.io/2023/09/29/adapter-design-pattern.html) as well to design for loose coupling.
* [Dependency Injection](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html) with [Configurer](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer) to resolve dependencies.
* [Hexagonal Architecture – Structure](https://jhumelsine.github.io/2023/10/28/hexagonal-architecture-structure.html) especially hexagonally shaped boundaries to highlight contracts and boundaries:
<img src="/assets/HexArchHexagons.png" alt="Hexagons" width = "100%" align="center" style="padding-right: 35px;">
* [Dependency and Knowledge Management](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html), which was presented in the context of Hexagonal Architecture, which also applies beyond the Hexagonal Architecture design, to allow SUTs to be unaware of their execution environments.

Future blogs will introduce additional concepts that will coalesce as well. I feel there may be a grand unified theory of software engineering that’s still beyond my grasp. If there is such a grand unified theory, I’d be willing to bet that Dependency and Knowledge Management is part of it.

# References
* Previous [blog references](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#references)
* [Test Doubles](https://en.wikipedia.org/wiki/Test_double) on Wikipedia
* [Test Doubles](https://martinfowler.com/bliki/TestDouble.html) by Martin Fowler
* [Test Doubles](http://xunitpatterns.com/Test%20Double.html) on xUnit Patterns
* [Test Doubles](https://testsigma.com/blog/test-doubles/) on Test Sigma Blog
* [Test Doubles in Android](https://developer.android.com/training/testing/fundamentals/test-doubles)
* [Test Doubles](https://abseil.io/resources/swe-book/html/ch13.html) Chapter 13 from [Software Engineering at Google](https://abseil.io/resources/swe-book) by Andrew Trenk and Dillon Bly
* [Test Doubles](https://learn.microsoft.com/en-us/archive/msdn-magazine/2007/september/unit-testing-exploring-the-continuum-of-test-doubles) from MSDN Magazine from Microsoft
* [Mockito](https://en.wikipedia.org/wiki/Mockito) on Wikipedia
* [Mockito](https://site.mockito.org/) on Mockito.org
* [Mockito](https://github.com/mockito/mockito) on GitHub
* [Mockito Guide](https://dzone.com/refcardz/mockito) on DZone
* [Mockito RefCard](https://dzone.com/refcardz/mockito) on DZone
* [Mockito Tutorial](https://www.baeldung.com/mockito-series) on Baeldung
* [Mokito's Mock Methods](https://www.baeldung.com/mockito-mock-methods) on Baeldung
* [Clean Unit Tests with Mockito](https://reflectoring.io/clean-unit-tests-with-mockito/) on Refactoring.io
