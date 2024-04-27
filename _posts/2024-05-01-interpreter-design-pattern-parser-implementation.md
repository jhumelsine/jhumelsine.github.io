---
title: Interpreter Design Pattern – Scanner and Parser Implementation
description: Implementing the Scanner and Parser for the Rational Expression Evaluator Use Case
unlisted: true
---

<img src="https://miro.medium.com/v2/resize:fit:828/format:webp/0*Z-EsrBvHP6n403Kq.jpg" alt="Code Listing on Screen with Magnifying Glass" title="Image Source: https://seniorbrogrammer.medium.com/3-good-software-engineering-practices-489674e8586e" width = "50%" align="center" style="padding-right: 35px;">

# Introduction
This blog continues the [Interpreter Design Pattern](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html) series by implementing a Scanner and Parser for the Rational Expression Evaluator using the theory and practice described in [Scanner and Parser](https://jhumelsine.github.io/2024/04/25/interpreter-design-pattern-parser.html).

This blog wraps up the Rational Expression Evaluator with a fully functioning Domain-Specific Language (DSL) implementation. For background on the journey that concludes here, see:
* [Rational Number Evaluator Use Case Introduction](https://jhumelsine.github.io/2024/03/18/interpreter-design-pattern-dsls.html#rational-number-evaluator-use-case) where the Rational Number Evaluator DSL was first introduced.
* [Rational Number Evaluator Grammar Use Case](https://jhumelsine.github.io/2024/04/02/interpreter-design-pattern-grammars.html#rational-number-evaluator-grammar-use-case) where the Rational Number Evaluator grammar was presented.
* [Rational Expression Evaluator Design](https://jhumelsine.github.io/2024/04/07/interpreter-design-pattern-design.html#grammar-to-design-via-use-case) where a UML class design for the Rational Expression Evaluator based upon the grammar was presented. I think this is when I started changing the name of the use case from _Rational Number Evaluator_ to _Rational Expression Evaluator_.
* [Rational Expression Evaluator Implementation](https://jhumelsine.github.io/2024/04/17/interpreter-design-pattern-implementation.html) where the Rational Expression Design classes were implemented based upon the design using [Test-Driven Development](https://en.wikipedia.org/wiki/Test-driven_development) (TDD) techniques for behavior confirmation and remain on track.
* [An Introduction to Scanner and Parser Theory and Practice](https://jhumelsine.github.io/2024/04/25/interpreter-design-pattern-parser.html) where Scanner and Parser fundamentals were presented in preparation for the implementation presented here.

Each phase from __Domain Specific Language__ to __Grammar__ to __Design__ to __Implementation__ to __Scanner/Parser__ has flowed almost seamlessly from one to the next. Though it's a bit of a cliché, and I cringe a bit whenever I state it, I still feel it applies: __The design and code almost writes themselves.__

There's still work to be done, but if the DSL and its Grammar are well thought out, that work tends to proceed smoothly.

# Scanner
While I devoted quite a bit of space writing about how [Scanners](https://jhumelsine.github.io/2024/04/25/interpreter-design-pattern-parser.html#scanners) can be implemented as [State Machines](https://jhumelsine.github.io/2024/04/25/interpreter-design-pattern-parser.html#finite-automata) derived from [Regular Languages](https://en.wikipedia.org/wiki/Regular_language), I’m not going to leverage much of that in my implementation.

My Rational Expression Evaluator DSL has simple tokens. They are integers, alphanumerics and several symbols. I think I can hand-craft something simple.

## Scanner Interface
Here’s my initial `Scanner` interface:
```java
interface Scanner {
    // Peek several tokens ahead.
    public Token peek(int index);

    // Consume a token making it unavailable in subsequent Scanner calls.
    public Token consume();
}
```

This interface is inspired by Iterators. I want the `Parser` to be able to peek ahead for `Token`s in the __OR__ rule-based methods and consume `Token`s in the __AND__ rule-based methods.

### Iterator
Iterator is a Gang of Four (GoF) Design Pattern. See:
* [Iterator at SourceMaking](https://sourcemaking.com/design_patterns/iterator)
* [Iterator at Refactoring.guru](https://refactoring.guru/design-patterns/iterator)

Why would the GoF devote a design pattern to something so obvious and ubiquitous? In [The Big Wheel Keeps on Turning](https://jhumelsine.github.io/2023/08/28/wheels.html) I used the _wheel_ as an example of a real world pattern to emphasize that patterns have been around for a long time. I wrapped up the example with:

> The wheel feels obvious to us since it’s so ubiquitous, but it took centuries if not millennia to perfect. Pre-Columbian indigenous peoples in the Americas, even with their own sophisticated technology, never developed it.

Sometimes patterns, such as the _wheel_, are only obvious once we’ve seen them or categorized them. I doubt that most developers would implement their own Iterators these days unless they wanted the ability to inject a [Specification](https://jhumelsine.github.io/2024/03/06/specification-design-pattern.html) into an Iterator. Iterators are built into most programming languages or their utility libraries. We don’t even think twice about Iterators these days.

I can assure you that Iterators were not as obvious and ubiquitous as they are today. Much like the wheel, they are only obvious in hindsight.

Iterator abstracts traversing a collection of data. I learned Data Structures in college in 1982. I’ve checked my Data Structures textbook, and I can’t find any references to anything that remotely looks like an Iterator. I remember quite explicitly learning the following traversal techniques in that course:
* For an _array_, use a `for` loop with an integer index, and access each array element via the index value.
* For a _linked list_, assign a pointer to the first node in the list. Then execute the body of a `while` loop while the pointer is not `null`. In the loop body, access the node being pointed at and reassign the pointer the `next` value of the current node.
* For a _tree_, recursively descend the left sub-tree, access the current node, and recursively descend the right sub-tree.

I remember the first time I was introduced to the Iterator concept. It was the mid 1990s and my project was using an internal proprietary framework/library which used Iterator internally. We could choose any data structure mechanism we desired, but we had to implement `first()` and `next()` so that the framework/library would know how to iterate whatever data structure we decided to use.

I was very confused by the requirement to provide implementations for `first()` and `next()`. In hindsight, these are two common methods for Iterator implementations.

## Token
The `Scanner` interface references `Token`. I debated whether to return `Token` or `String`. I decided upon `Token`. It’s a [Value Object](https://en.wikipedia.org/wiki/Value_object) which allows for more context and additional token related methods, whose need will emerge as I work on the `Parser` implementation. I’ll add them when needed.
```java
class Token {
    private final String tokenValue;

    public Token(String tokenValue) {
        this.tokenValue = tokenValue;
    }

    public String getValue() {
        return tokenValue;
    }
}
```

## Scanner Tests
Here is a basic test for `Scanner`. It confirms a bit more than I usually prefer unit tests, but I wanted to get the basics in place:
* `toString()` returns a string version of the internal data of the `Scanner`.
* `peek(0)` returns the current token. It does not change the state of the `Scanner`. It’s idempotent, so I’ve asserted it twice.
* A `peek(int)` beyond the remaining list of `Token`s should throw an exception.
* `consume()` returns the current token. It does change the state of the `Scanner`. It is not idempotent.
* `consume()` should throw an exception once all tokens are consumed.
* The `IndexOutOfBoundsException` is a bit leaky, but I’m going to use it for now just to keep the implementation smaller.

```java
            Scanner scanner = new ScannerImpl("  A  ");
            assertEquals("index=0, tokens=[A]", scanner.toString());

            Token peekToken1 = scanner.peek(0);
            assertEquals("A", peekToken1.getValue());
            assertEquals("index=0, tokens=[A]", scanner.toString());

            Token peekToken2 = scanner.peek(0);
            assertEquals("A", peekToken2.getValue());
            assertEquals("index=0, tokens=[A]", scanner.toString());

            Token consumedToken1 = scanner.consume();
            assertEquals("A", consumedToken1.getValue());
            assertEquals("index=1, tokens=[A]", scanner.toString());

            try {
                Token peekToken3 = scanner.peek(0);
                throw new Exception(); // Should throw IndexOutOfBoundsException and not reach here.
            } catch (IndexOutOfBoundsException e) {
                // Expected. No more tokens available after consume();
            }

            try {
                Token consumedToken2 = scanner.consume();
                throw new Exception(); // Should throw IndexOutOfBoundsException and not reach here.
            } catch (IndexOutOfBoundsException e) {
                // Expected. No more tokens available after previous consume();
            }
```

I will be adding more `Scanner` tests, but I won’t repeat them here. They will be in the complete implementation at the bottom of this blog.

## Scanner Implementation
This Scanner Implementation below is only complete enough to allow the test above to pass.

```java
class ScannerImpl implements Scanner {
    private int iteratorIndex = 0;
    private List<String> tokens = new ArrayList<>();

    public ScannerImpl(String input) {
        tokens.add(input.trim());
    }

    public Token peek(int index) {
        return new Token(tokens.get(iteratorIndex + index));
    }

    public Token consume() {
        return new Token(tokens.get(iteratorIndex++));
    }

    @Override
    public String toString() {
        return String.format("index=%d, tokens=%s", iteratorIndex, tokens.toString());
    }
}
```

I have been using [Test-Driven Development](https://en.wikipedia.org/wiki/Test-driven_development) (TDD) as I progress with the `ScannerImpl` implementation in conjunction with writing this blog. … I have completed `ScannerImpl` using TDD. Here is some context about the implementation:

* I’ve kept the `Scanner` implementation to bare bones. If I were designing a DSL for production use:
    * I would have kept track of line and column information in the input so that when a syntax error is encountered, it would indicate issue with context.
    * I would have supported comments, and they would have been stripped out in the `Scanner` implementation.
    * I would not have leaked an `IndexOutOfBoundsException`. I would have caught it and returned an Exception that is specific to the `Scanner` such as `NoMoreTokensException`.
    * I would have been more particular about alphanumerics starting with a letter. My current `Scanner` implementation will accept __123ABC__ as a `Token`. Most scanners would balk at an alphanumeric starting with a digit, especially as an identifier.
* My implementation has two basic phases:
    * Split the String into tokens based upon whitespace.
    * Refine each whitespace delimited token one symbol at a time with DSL defined symbols, such as `+`, `-`, `=`, etc., as their own `Token`s and all other symbols concatenated together as one `Token`.
* My implementation is a form of state machine. It’s evaluating the string one symbol at a time as it determines which are `Token`s.

```java
class ScannerImpl implements Scanner {
    private int iteratorIndex = 0;
    private List<String> tokens = new ArrayList<>();

    public ScannerImpl(String input) {
        for (String whitespaceDelimitedToken : Arrays.asList(input.trim().split("\\s+"))) {
            refinedScanning(whitespaceDelimitedToken);
        }
    }

    private void refinedScanning(String token) {
        String accumulatingToken = "";

        for (int i = 0; i < token.length(); i++) {
            String nextSymbol = token.substring(i, i+1);
            if ("+-*/()=,".contains(nextSymbol)) {
                if (!accumulatingToken.isEmpty()) {
                    tokens.add(accumulatingToken);
                    accumulatingToken = "";
                }
                tokens.add(nextSymbol);
            } else {
                accumulatingToken = accumulatingToken + nextSymbol;
            }
        }

        if (!accumulatingToken.isEmpty()) tokens.add(accumulatingToken);
    }

    public Token peek(int index) {
        return new Token(tokens.get(iteratorIndex + index));
    }

    public Token consume() {
        return new Token(tokens.get(iteratorIndex++));
    }

    @Override
    public String toString() {
        return String.format("index=%d, tokens=%s", iteratorIndex, tokens.toString());
    }
}

```

# Parser
The `Parser` implementation will more closely follow the techniques I described in [Scanner and Parser]( https://jhumelsine.github.io/2024/04/25/interpreter-design-pattern-parser.html):

__OR__ and __AND__ concepts are important in the grammar, design, implementation and parser. __OR__ and __AND__ concepts are their common thread. Define a solid grammar using __OR__ and __AND__ rules and the design, implementation and parser practically write themselves.
* __OR__ rules:
    * Map to __IS-A__ relationships in the design.
    * Map to interfaces and `implements` in the class implementation.
    * Map to a method that peeks ahead looking at tokens to determine which of several more specific rule methods to call recursively.
* __AND__ rules:
    * Map to __HAS-A__ relationships in the design.
    * Map to field attributes in the class implementation.
    * Map to a method that consumes tokens, call the rule methods recursively that define the rule and assemble all elements into an object.

The `Parser` implementation will mirror the steps that were taken for the [class implementation](https://jhumelsine.github.io/2024/04/17/interpreter-design-pattern-implementation.html). Even the tests will basically be the same, except now they will be confirming the `Parser`’s ability to create and construct to the class objects rather than the functionality of the classes themselves.

## Rational
Let’s start with parsing Rational.

### First Rational Test and Implementation
The first Design to Implementation test was:
```java
assertEquals("0", new Rational("0").evaluate(null).toString());
```

The same test will be modified as a `Parser` test by removing the specific `Rational` reference and replacing it with `ScannerImpl` and `Parser` references as so:
```java
assertEquals("0", new Parser(new ScannerImpl("0")).parse().evaluate(null).toString());
```

Everything else is the same. Rather than explicitly creating a `Rational` object, the `Parser` and `ScannerImpl` will create it for us implicitly. The other `Parser` tests will follow the same technique to replace the explicit class creation with the `Parser` and `ScannerImpl`.

Here is the initial `Parser` implementation with just enough provided to allow this test to pass.

Look at the parallels of these methods and the equivalent UML classes in [Design to Implementation – Where to Start?](https://jhumelsine.github.io/2024/04/17/interpreter-design-pattern-implementation.html#where-to-start) Recall that parsers are [Configurers](https://jhumelsine.github.io/2023/10/09/dependency-injection-design-pattern.html#configurer) so they have knowledge of the classes in the design.

```java
class Parser {
    private final Scanner scanner;

    public Parser(Scanner scanner) {
        this.scanner = scanner;
    }

    public Statement parse() {
        return parseExpression();
    }

    private Expression parseExpression() {
        return parseRational();
    }

    private Rational parseRational() {
        return new Rational(scanner.consume().getValue());
    }

}
```

I’ll add more tests to cover more scenarios and their corresponding implementations.

### Complete Rational Test and Implementation
`Rational` will be the most complicated part of the `Parser`, since it needs to manage `Rational` values in three forms:
* _Integer_, such as `5`
* _Integer / Integer_, such as `1/2`
* _Integer Integer / Integer_, such as `5 1/2`

Here’s the updated methods that handle all three cases. It could possibly use some refactoring, but I think it’s okay for now:
```java
    private Rational parseRational() {
        Token token = scanner.peek(0);

        if (token.isMinus()) {
            scanner.consume();
            return new Rational("-" + parseNumber());
        }

        return new Rational(parseNumber());
    }

    private String parseNumber() {
        Token token0 = scanner.peek(0);

        if (scanner.getRemainingTokenCount() > 2) {
            Token token1 = scanner.peek(1);
            Token token2 = scanner.peek(2);

            if (scanner.getRemainingTokenCount() > 3) {
                Token token3 = scanner.peek(3);

                if (token0.isInteger() && token1.isInteger() && token2.isSlash() && token3.isInteger()) {
                    return String.format("%s %s %s %s", scanner.consume().getValue(), scanner.consume().getValue(), scanner.consume().getValue(), scanner.consume().getValue());
                }
            }
            
            if (token0.isInteger() && token1.isSlash() && token2.isInteger()) {
                return String.format("%s %s %s", scanner.consume().getValue(), scanner.consume().getValue(), scanner.consume().getValue());
            }
        }

        return scanner.consume().getValue();
    }
```

I have added a few more methods to `Scanner` and `Token` as I needed them. I’ll continue this as I progress with the rest of the implementation. See the complete implementation at the end of the blog for what's been added.

We can think of `Rational` as a combination of __OR__ and __AND__ rules. It peeks at tokens to determine which of the three `Rational` forms applies, and it consumes the tokens that apply in constructing the `Rational` object.

## SubtractOp
This is where things get fun and interesting. Let’s start with the tests:
```java
        assertEquals("1", new Parser(new ScannerImpl("-(4, 3)")).parse().evaluate(null).toString());
        assertEquals("2 1/2", new Parser(new ScannerImpl("-(4, 1 1/2)")).parse().evaluate(null).toString());
        assertEquals("-3", new Parser(new ScannerImpl("-(2, 5)")).parse().evaluate(null).toString());
```

This is the first time a real expression has been parsed and evaluated. Technically `Rational` is an expression, but it’s not quite the same.

### Expression OR Rule
This is the grammar rule for Expressions:
```
Expression ::= Identifier | Rational | AddOperation | SubtractOperation | MultipleOperation | DivideOperation
```

Expression is an __OR__ rule for which the `SubtractOperation` is a defining rule. The `Parser` needs to identify which rule to descend to for additional processing. Right now, the implementation supports `Rational` by default, and I’m adding `SubtractOperation`.

The implementation will need to peek ahead at a few tokens to make this decision. If the next two tokens are `-` and `(` then it’s a `SubtractOperation` `Expression`. Otherwise, it’s a `Rational` `Expression`.

Here’s the implementation so far:
```java
    private Expression parseExpression() throws Exception {
        Token token0 = scanner.peek(0);

        if (scanner.getRemainingTokenCount() > 1) {
            Token token1 = scanner.peek(1);
            
            if (token0.isMinus() && token1.isOperationStart()) {
                return parseSubtractOp();
            }
        }

        return parseRational();
    }
```

### SubtractOperation AND Rule
This is the grammar for `SubtractOperation`:
```
SubtractOperation ::= - ( Expression, Expression)
```

The `SubtractOperation` parse method will consume tokens and recursively parse the two `Expression`s and return a `SubtractOp` object. Most of the implementation is concrete `Token` management. Obtaining the `Expression`s is trivial.
```java
    private SubtractOp parseSubtractOp() throws Exception {
        Token minusToken = scanner.consume();

        if (!minusToken.isMinus()) throw new Exception("Expected Minus Symbol (-). Got: " + minusToken);

        Token operationStartToken = scanner.consume();

        if (!operationStartToken.isOperationStart()) throw new Exception("Expected Operation Start Symbol ((). Got: " + operationStartToken);

        Expression expression1 = parseExpression();

        Token delimiterToken = scanner.consume();

        if (!delimiterToken.isDelimiter()) throw new Exception("Expected Delimieter Symbol (,). Got: " + delimiterToken);

        Expression expression2 = parseExpression();
        
        Token operationFinishToken = scanner.consume();

        if (!operationFinishToken.isOperationFinish()) throw new Exception("Expected Operation Finish Symbol ()). Got: " + operationFinishToken);

        return new SubtractOp(expression1, expression2);
    }
```

## DivideOp
`DivideOp` is so much like `SubtractOp` that it’s not worth repeating here. See complete code below.

There’s quite a bit of similar code, but I don’t think I’m going to try and refactor it. NOTE: I have refactored it as can be seen in the complete implementation.

## AddOp
`AddOp` is like `SubtractOp`, except that it supports multiple `Expression`s. Here are some examples of the previous `AddOp` unit tests:
```java
        {
            MultiOperandsOp addOp = new AddOp();
            addOp.addExpression(new Rational("1/2"));
            addOp.addExpression(new Rational("1/6"));
            addOp.addExpression(new Rational("1/6"));
            assertEquals("5/6", addOp.evaluate(null).toString());
        }

        {
            MultiOperandsOp addOp = new AddOp();
            addOp.addExpression(new Rational("1 1/2"));
            addOp.addExpression(new Rational("2 1/6"));
            addOp.addExpression(new Rational("3 1/6"));
            addOp.addExpression(new Rational("4 1/6"));
            assertEquals("11", addOp.evaluate(null).toString());
        }
```

Here’s what the `Parser` based equivalent tests look like. All of the previous construction disappears when using the `Parser` which will manage it:
```java
        assertEquals("7", new Parser(new ScannerImpl("+(4, 3)")).parse().evaluate(null).toString());
        assertEquals("2/3", new Parser(new ScannerImpl("+(1/2, 1/6)")).parse().evaluate(null).toString());
        assertEquals("5/6", new Parser(new ScannerImpl("+(1/2, +(1/6, 1/6))")).parse().evaluate(null).toString());
        assertEquals("5/6", new Parser(new ScannerImpl("+(1/2, 1/6, 1/6)")).parse().evaluate(null).toString());
        assertEquals("11", new Parser(new ScannerImpl("+(1 1/2, 2 1/6, 3 1/6, 4 1/6)")).parse().evaluate(null).toString());
```

Note that the third test above is a nested Addition test. It’s an additional expression that contains another addition expression.

The implementation is also slightly different, since it needs to support any number of `Expression` operands:
```java
    private AddOp parseAddOp() throws Exception {
        Token plusToken = scanner.consume();

        if (!plusToken.isPlus()) throw new Exception("Expected Minus Symbol (+). Got: " + plusToken);

        Token operationStartToken = scanner.consume();

        if (!operationStartToken.isOperationStart()) throw new Exception("Expected Operation Start Symbol ((). Got: " + operationStartToken);

        AddOp addOp = new AddOp();

        while (true) {
            addOp.addExpression(parseExpression());

            Token nextToken = scanner.peek(0);

            if (nextToken.isDelimiter()) {
                scanner.consume();
            } else if (nextToken.isOperationFinish()) {
                break;
            }
        }

        return addOp;
    }
```

## MultiplyOp
`MultiplyOp` is so like `AddOp` that it’s not worth repeating here. See complete code below.

There’s quite a bit of similar code, but I don’t think I’m going to try and refactor it. NOTE: I have refactored it as can be seen in the complete implementation.

## Identifier
`Identifier` was trivial to add to the `Parser`. See complete code below.

## Assignment
`Assignment` was trivial to add to the `Parser`. See complete code below.

## HORRIBLE Code
I had presented some unit test code in [One Final Test]( https://jhumelsine.github.io/2024/04/17/interpreter-design-pattern-implementation.html#one-final-test) where I declared:
> It works, but it’s __HORRIBLE__. Unfortunately, this is where the Gang of Four and other Interpreter Design Pattern write ups tend to leave you. … I am bound and determined to complete this DSL use case [with a Scanner and Parser].

Here is what the new unit test looks like with the `Scanner` and `Parser` implementations:
```java
            // a = 1 4/5
            // b = 2 5/7
            // c = a * b
            // d = a + b + c
            // e = a + b + c + d
            // f = a * b * c * d * e

            Context context = new Context();
            assertEquals("1 4/5", new Parser(new ScannerImpl("a = 1 4/5")).parse().evaluate(context).toString());
            assertEquals("2 5/7", new Parser(new ScannerImpl("b = 2 5/7")).parse().evaluate(context).toString());
            assertEquals("4 31/35", new Parser(new ScannerImpl("c = *(a, b)")).parse().evaluate(context).toString());
            assertEquals("9 2/5", new Parser(new ScannerImpl("d = +(a, b, c)")).parse().evaluate(context).toString());
            assertEquals("18 4/5", new Parser(new ScannerImpl("e = +(a, b, c, d)")).parse().evaluate(context).toString());
            assertEquals("4218 10488/30625", new Parser(new ScannerImpl("f = *(a, b, c, d, e)")).parse().evaluate(context).toString());

            assertEquals("1 4/5", new Parser(new ScannerImpl("a")).parse().evaluate(context).toString());
            assertEquals("2 5/7", new Parser(new ScannerImpl("b")).parse().evaluate(context).toString());
            assertEquals("4 31/35", new Parser(new ScannerImpl("c")).parse().evaluate(context).toString());
            assertEquals("9 2/5", new Parser(new ScannerImpl("d")).parse().evaluate(context).toString());
            assertEquals("18 4/5", new Parser(new ScannerImpl("e")).parse().evaluate(context).toString());
            assertEquals("4218 10488/30625", new Parser(new ScannerImpl("f")).parse().evaluate(context).toString());
```

It’s so much nicer, and the final asserts are mostly redundant.

I still need a little bit more to make it a true DSL that a user can use it.

# Scanner/Parser Summary
I was able to complete the `Scanner`, `ScannerImpl`, `Token` and `Parser` classes from scratch in one day. The class implementations are about 314 lines of code. The tests are an additional 231 lines of code, but they were mostly a translation of the design implementation unit test scenarios so that they could confirm `Parser` behavior.

For the most part, the code flew out of my fingertips. The mathematical rigor I presented in [Scanner and Parser](https://jhumelsine.github.io/2024/04/25/interpreter-design-pattern-parser.html) dictated most of the implementation. TDD allowed me to implement in small manageable chucks without concerns that I was breaking previous behavior.

NOTE: I broke previous behavior a few times, for example, when I forgot to take the minus sign into consideration when parsing. The existing minus sign tests failed, and I immediately knew how to address it. I also made a few too many changes and tests failed in a way I didn’t initially understand. I reverted those changes, got back to passing, and proceeded in smaller steps without further incident.

The recursive nature of parsers has always fascinated me, especially in the __AND__ rule implementations. When the code makes that recursive descending call, it doesn’t know how many tokens will be consumed or how much processing will be completed. It could be a few a tokens or it could be thousands of tokens. And when that processing has completed, it returns exactly to where it left off. I know we can count on this because of the theory, but it always feels a little magical when I implement it and see it working.

# REPL
The pieces are done, except for one. How does the user interact with my Rational Expression Evaluator DSL?

I think the user should interact with this DSL via a Read-Eval-Print Loop ([REPL](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)). This was simple to add. It is not technically part of the Interpreter design, but it's needed to complete the entire user DSL experience.

It’s a loop in `main` that reads in a string from standard `in` and evaluates it. The `REE>` prompt stands for __Rational Expression Evaluator__:
```java
public class RationalEvaluator {
    public static void main(String[] args) throws Exception {
        Test.test(); // Comment this out to start up faster.

        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

        Context context = new Context();
        while (true) {
            System.out.print("REE> ");
            System.out.println(new Parser(new ScannerImpl(reader.readLine().trim())).parse().evaluate(context).toString());
        }
    }
}
```

Here’s an example of the REPL. Everything on a `REE>` line has been typed in by me on my keyboard. The following line is what’s been evaluated and printed:
```
java RationalEvaluator.java
REE> 1
1
REE> 1/2
1/2
REE> 2/4
1/2
REE> 7/3
2 1/3
REE> +(1 3/5, 3 7/8)
5 19/40
REE> +(1 4/5, *(6/3, 2/3, 7/9), /(5/7, 1/19))
2 113/135
REE> 234234/12312
19 17/684
REE>
```

One more thing is missing. Right now, the REPL will attempt to evaluate the expression when the string is entered. But what if the expression is so large that the user wishes to use several lines to make it clearer?

I will update the REPL code so that it will accumulate input until there’s a complete expression, which I can determine when the number of open and closed parentheses are balanced. I won’t provide that update here, but it will be in the complete code below.

# Summary
__We’re done!__

This wraps up the description, theory, practice, design, implementation, et al. associated with the Interpreter Design Pattern. __Congratulations if you made it this far.__

I have one more follow up Interpreter blog, but it will be feature the DSL I designed and implemented in my career and some of the experienced our team had with the pattern. It will not present any new Interpreter concepts or details.

# References
See: [Interpreter Design Pattern Introduction/References](https://jhumelsine.github.io/2024/03/12/interpreter-design-pattern-introduction.html#references) for overall references.

## The Complete Implementation
Here’s the entire implementation up to this point as one file. Copy and paste it into a Java environment and execute it. If you don’t have Java, try this [Online Java Environment]( https://www.tutorialspoint.com/java/online-java-compiler.php). However, it's a little glitchy with the REPL interface. I've gotten a few bash errors where I shouldn't be getting them.

A few highlights:
* The REPL portion accommodates expressions across multiple lines.
* The entire implementation is just over 950 lines. This is the entire Rational Expression Evaluator DSL implementation with tests.
* The code is about half implementation and half test.
* I refactored the Subtract/Divide Operator and Add/Multiply Operator parse methods. The similar/duplicate code has been consolidated.
* Additional public methods were added to `Scanner` and `Token`. They are included in the code below.

```java
import java.util.*;
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class RationalEvaluator {
    public static void main(String[] args) throws Exception {
        Test.test(); // Comment this out to start up faster.

        BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));

        String accumulatingExpression = "";
        int parenthesesBalanceCounter = 0;
        Context context = new Context();
        while (true) {
            System.out.print("REE> ");
            String expression = reader.readLine().trim();
            accumulatingExpression = accumulatingExpression + " " + expression;
            parenthesesBalanceCounter += getParenthesesBalanceCounter(expression);
            if (parenthesesBalanceCounter == 0) {
                System.out.println(new Parser(new ScannerImpl(accumulatingExpression)).parse().evaluate(context).toString());
                accumulatingExpression = "";
            }
        }
    }

    private static int getParenthesesBalanceCounter(String expression) {
        int parenthesesBalanceCounter = 0;

        for (int i = 0; i < expression.length(); i++) {
            switch (expression.charAt(i)) {
                case '(': parenthesesBalanceCounter++; break;
                case ')': parenthesesBalanceCounter--; break;
                default:;
            }
        }

        return parenthesesBalanceCounter;
    }
}

interface Statement {
    Rational evaluate(Context context);
}

class Context {
    private final Map<String, Rational> state = new HashMap<>();

    public void add(String identifier, Rational rational) {
        state.put(identifier, rational);
    }

    public boolean containsKey(String identifier) {
        return state.containsKey(identifier);
    }

    public Rational getRational(String identifier) {
        return state.get(identifier);
    }
}

interface Expression extends Statement {}

class Rational implements Expression {
    private final int numerator;
    private final int denominator;

    public Rational(String rationalString) {
        int integer = 0;
        int numerator;
        int denominator;

        if (!rationalString.contains("/")) {
            numerator = Integer.parseInt(rationalString.trim());
            denominator = 1;
        } else {
            String[] integers = rationalString.split("/");
            if (integers[0].trim().contains(" ")) {
                String[] tokens = integers[0].trim().split("\\s+");
                integer = Integer.parseInt(tokens[0].trim());
                numerator = Integer.parseInt(tokens[1].trim());
            } else {
                numerator = Integer.parseInt(integers[0].trim());
            }
            denominator = Integer.parseInt(integers[1].trim());
            numerator += integer * denominator ;
        }

        int divisor = denominator;
        while (divisor > 1) {
            if (numerator % divisor == 0 && denominator % divisor == 0) {
                numerator = numerator / divisor;
                denominator = denominator / divisor;
            } else {
                divisor--;
            }
        }

        this.numerator = numerator;
        this.denominator = denominator;

        if (denominator == 0) throw new ArithmeticException();
    }

    @Override
    public Rational evaluate(Context context) {
        return this;
    }

    public int getNumerator() {
        return numerator;
    }

    public int getDenominator() {
        return denominator;
    }

    @ Override
    public String toString() {
        if (denominator == 1) {
            return String.valueOf(numerator);
        } else if (Math.abs(numerator) > denominator) {
            return String.format("%d %d/%d", numerator/denominator, Math.abs(numerator%denominator), denominator);
        }
        return String.valueOf(numerator) + "/" + String.valueOf(denominator);
    }

}

abstract class BinaryOp implements Expression {
    private final Expression operand1;
    private final Expression operand2;

    public BinaryOp(Expression operand1, Expression operand2) {
        this.operand1 = operand1;
        this.operand2 = operand2;
    }

    @Override
    public Rational evaluate(Context context) {
        Rational rational1 = operand1.evaluate(context);
        Rational rational2 = operand2.evaluate(context);

        return operation(rational1.getNumerator(), rational1.getDenominator(), rational2.getNumerator(), rational2.getDenominator());
    }

    abstract protected Rational operation(int numerator1, int denominator1, int numerator2, int denominator2);
}

class SubtractOp extends BinaryOp {
    public SubtractOp(Expression op1, Expression op2) {
        super(op1, op2);
    }

    @Override
    protected Rational operation(int numerator1, int denominator1, int numerator2, int denominator2) {
        int numerator = numerator1 * denominator2 - numerator2 * denominator1;
        int denominator = denominator1 * denominator2;

        return new Rational(String.format("%d/%d", numerator, denominator));
    }
}

class DivideOp extends BinaryOp {
    public DivideOp(Expression op1, Expression op2) {
        super(op1, op2);
    }

    @Override
    protected Rational operation(int numerator1, int denominator1, int numerator2, int denominator2) {
        int numerator = numerator1 * denominator2;
        int denominator = denominator1 * numerator2;

        return new Rational(String.format("%d/%d", numerator, denominator));
    }
}

abstract class MultiOperandsOp implements Expression {
    private final List<Expression> expressions = new LinkedList<>();

    public void addExpression(Expression expression) {
        expressions.add(expression);
    }

    @Override
    public Rational evaluate(Context context) {
        Rational accumulator = getIdentity();
        for(Expression expression : expressions) {
            Rational operand = expression.evaluate(context);
            accumulator = operation(accumulator.getNumerator(), accumulator.getDenominator(), operand.getNumerator(), operand.getDenominator());
        }

        return accumulator;
    }

    abstract protected Rational getIdentity();

    abstract protected Rational operation(int numerator1, int denominator1, int numerator2, int denominator2);
}

class AddOp extends MultiOperandsOp {
    @Override
    protected Rational getIdentity() {
        return new Rational("0");
    }

    @Override
    protected Rational operation(int numerator1, int denominator1, int numerator2, int denominator2) {
        int numerator = numerator1 * denominator2 + numerator2 * denominator1;
        int denominator = denominator1 * denominator2;

        return new Rational(String.format("%d/%d", numerator, denominator));
    }
}

class MultiplyOp extends MultiOperandsOp {
    @Override
    protected Rational getIdentity() {
        return new Rational("1");
    }

    @Override
    protected Rational operation(int numerator1, int denominator1, int numerator2, int denominator2) {
        int numerator = numerator1 * numerator2;
        int denominator = denominator1 * denominator2;

        return new Rational(String.format("%d/%d", numerator, denominator));
    }
}

class Identifier implements Expression {
    private final String identifier;

    public Identifier(String identifier) {
        this.identifier = identifier;
    }

    @Override
    public Rational evaluate(Context context) {
        if (!context.containsKey(identifier)) return new Rational("0");
        return context.getRational(identifier);
    }
}

class Assignment implements Statement {
    private final String identifier;
    private final Expression expression;

    public Assignment(String identifier, Expression expression) {
        this.identifier = identifier;
        this.expression = expression;
    }

    @Override
    public Rational evaluate(Context context) {
        Rational rational = expression.evaluate(context);
        context.add(identifier, rational);
        return rational;
    }
}

interface Scanner {
    // Peek several tokens ahead.
    public Token peek(int index);

    // Consume a token making it unavailable in subsequent Scanner calls.
    public Token consume();

    public int getRemainingTokenCount();
}

class ScannerImpl implements Scanner {
    private int iteratorIndex = 0;
    private List<String> tokens = new ArrayList<>();

    public ScannerImpl(String input) {
        for (String whitespaceDelimitedToken : Arrays.asList(input.trim().split("\\s+"))) {
            refinedScanning(whitespaceDelimitedToken);
        }
    }

    private void refinedScanning(String token) {
        String accumulatingToken = "";

        for (int i = 0; i < token.length(); i++) {
            String nextSymbol = token.substring(i, i+1);
            if ("+-*/()=,".contains(nextSymbol)) {
                if (!accumulatingToken.isEmpty()) {
                    tokens.add(accumulatingToken);
                    accumulatingToken = "";
                }
                tokens.add(nextSymbol);
            } else {
                accumulatingToken = accumulatingToken + nextSymbol;
            }
        }

        if (!accumulatingToken.isEmpty()) tokens.add(accumulatingToken);
    }

    public Token peek(int index) {
        return new Token(tokens.get(iteratorIndex + index));
    }

    public Token consume() {
        return new Token(tokens.get(iteratorIndex++));
    }

    public int getRemainingTokenCount() {
        return tokens.size() - iteratorIndex;
    }

    @Override
    public String toString() {
        return String.format("index=%d, tokens=%s", iteratorIndex, tokens.toString());
    }
}

class Token {
    private final String tokenValue;

    public Token(String tokenValue) {
        this.tokenValue = tokenValue;
    }

    public String getValue() {
        return tokenValue;
    }

    public boolean isAssignment() {
        return "=".equals(tokenValue);
    }

    public boolean isPlus() {
        return "+".equals(tokenValue);
    }

    public boolean isMinus() {
        return "-".equals(tokenValue);
    }
    
    public boolean isStar() {
        return "*".equals(tokenValue);
    }

    public boolean isSlash() {
        return "/".equals(tokenValue);
    }

    public boolean isOperationStart() {
        return "(".equals(tokenValue);
    }

    public boolean isOperationFinish() {
        return ")".equals(tokenValue);
    }

    public boolean isDelimiter() {
        return ",".equals(tokenValue);
    }

    public boolean isInteger() {
        try {
            Integer.parseInt(tokenValue);
            return true;
        } catch (NumberFormatException e) {
            return false;
        }
    }
}

class Parser {
    private final Scanner scanner;

    public Parser(Scanner scanner) {
        this.scanner = scanner;
    }

    // OR Rule. Determine which specific Statement rule to apply.
    public Statement parse() throws Exception {
        if (scanner.getRemainingTokenCount() > 1) {
            if (scanner.peek(1).isAssignment()) {
                return parseAssignment();
            }
        }

        return parseExpression();
    }

    // AND Rule. Consume tokens and build object from its components.
    private Assignment parseAssignment() throws Exception {
        Token variableNameToken = scanner.consume();

        Token assignmentToken = scanner.consume();
        if (!assignmentToken.isAssignment()) throw new Exception("Expected Assignment Symbol (=). Got: " + assignmentToken);

        return new Assignment(variableNameToken.getValue(), parseExpression());
    }

    // OR Rule. Determine which specific Expression rule to apply.
    private Expression parseExpression() throws Exception {
        Token token0 = scanner.peek(0);

        if (scanner.getRemainingTokenCount() > 1) {
            Token token1 = scanner.peek(1);

            if (token1.isOperationStart()) {
                if (token0.isMinus() || token0.isSlash()) {
                    return parseBinaryOp();
                }

                if (token0.isPlus() || token0.isStar()) {
                    return parseMultiOperandsOp();
                }
            }
        }

        if (!token0.isInteger() && !token0.isMinus()) {
            return parseIdentifier();
        } 

        return parseRational();
    }

    // AND Rule. Consume tokens and build object from its components.
    private BinaryOp parseBinaryOp() throws Exception {
        Token operatorToken = scanner.consume();

        Token operationStartToken = scanner.consume();
        if (!operationStartToken.isOperationStart()) throw new Exception("Expected Operation Start Symbol ((). Got: " + operationStartToken);

        Expression expression1 = parseExpression();

        Token delimiterToken = scanner.consume();
        if (!delimiterToken.isDelimiter()) throw new Exception("Expected Delimiter Symbol (,). Got: " + delimiterToken);

        Expression expression2 = parseExpression();
        
        Token operationFinishToken = scanner.consume();
        if (!operationFinishToken.isOperationFinish()) throw new Exception("Expected Operation Finish Symbol ()). Got: " + operationFinishToken);

        return getBinaryOp(operatorToken, expression1, expression2);
    }

    private BinaryOp getBinaryOp(Token operatorToken, Expression expression1, Expression expression2) throws Exception {
        if (operatorToken.isMinus()) {
            return new SubtractOp(expression1, expression2);
        } else if (operatorToken.isSlash()) {
            return new DivideOp(expression1, expression2);
        } else {
            throw new Exception("Unexpected operator symbol: " + operatorToken.getValue());
        }
    }

    // AND Rule. Consume tokens and build object from its components.
    private MultiOperandsOp parseMultiOperandsOp() throws Exception {
        MultiOperandsOp multiOperandsOp = getMultiOperandsOp(scanner.consume());

        Token operationStartToken = scanner.consume();
        if (!operationStartToken.isOperationStart()) throw new Exception("Expected Operation Start Symbol ((). Got: " + operationStartToken);

        while (true) {
            multiOperandsOp.addExpression(parseExpression());
            
            Token nextToken = scanner.peek(0);
            if (nextToken.isDelimiter()) {
                scanner.consume();
            } else if (nextToken.isOperationFinish()) {
                break;
            }
        }

        return multiOperandsOp;
    }

    private MultiOperandsOp getMultiOperandsOp(Token operatorToken) throws Exception {
        if (operatorToken.isPlus()) {
            return new AddOp();
        } else if (operatorToken.isStar()) {
            return new MultiplyOp();
        } else {
            throw new Exception("Unexpected operator symbol: " + operatorToken.getValue());
        }
    }

    private Identifier parseIdentifier() {
        return new Identifier(scanner.consume().getValue());
    }

    private Rational parseRational() {
        Token token = scanner.peek(0);

        if (token.isMinus()) {
            scanner.consume();
            return new Rational("-" + parseNumber());
        }

        return new Rational(parseNumber());
    }

    private String parseNumber() {
        Token token0 = scanner.peek(0);

        if (scanner.getRemainingTokenCount() > 2) {
            Token token1 = scanner.peek(1);
            Token token2 = scanner.peek(2);

            if (scanner.getRemainingTokenCount() > 3) {
                Token token3 = scanner.peek(3);

                if (token0.isInteger() && token1.isInteger() && token2.isSlash() && token3.isInteger()) {
                    return String.format("%s %s %s %s", scanner.consume().getValue(), scanner.consume().getValue(), scanner.consume().getValue(), scanner.consume().getValue());
                }
            }
            
            if (token0.isInteger() && token1.isSlash() && token2.isInteger()) {
                return String.format("%s %s %s", scanner.consume().getValue(), scanner.consume().getValue(), scanner.consume().getValue());
            }
        }

        return scanner.consume().getValue();
    }

}

class Test {
    public static void test() throws Exception {
        testDesignElements();

        testScanner();

        testParser();
    }

    private static void testDesignElements() throws Exception {
        assertEquals("0", new Rational("0").evaluate(null).toString());
        assertEquals("1", new Rational("1 ").evaluate(null).toString());
        assertEquals("-1", new Rational(" -1").evaluate(null).toString());
        assertEquals("5", new Rational(" 5 ").evaluate(null).toString());
        assertEquals("5", new Rational("    5   ").evaluate(null).toString());

        assertEquals("1/2", new Rational(" 1/2 ").evaluate(null).toString());
        assertEquals("-1/2", new Rational(" -1/2 ").evaluate(null).toString());
        assertEquals("2/3", new Rational("2 / 3").evaluate(null).toString());
        try {
            new Rational("1/0").evaluate(null);
            throw new Exception(); // Should throw ArithmeticException and not reach here.
        } catch (ArithmeticException e) {
            // Expected. Division by zero.
        }
        try {
            new Rational("0/0").evaluate(null);
            throw new Exception(); // Should throw ArithmeticException and not reach here.
        } catch (ArithmeticException e) {
            // Expected. Division by zero.
        }

        assertEquals("1/2", new Rational("2/4").evaluate(null).toString());
        assertEquals("1/2", new Rational("32/64").evaluate(null).toString());
        assertEquals("1/4", new Rational("30 / 120").evaluate(null).toString());

        assertEquals("1 2/5", new Rational("7/5").evaluate(null).toString());
        assertEquals("2 1/4", new Rational("36/16").evaluate(null).toString());
        assertEquals("-2 1/4", new Rational("-36/16").evaluate(null).toString());
        assertEquals("5", new Rational("15 / 3").evaluate(null).toString());
        assertEquals("-3", new Rational("-15 / 5").evaluate(null).toString());

        assertEquals("1 5/6", new Rational("1 5/6").evaluate(null).toString());
        assertEquals("2 2/3", new Rational(" 2 4 / 6 ").evaluate(null).toString());
        assertEquals("4 1/2", new Rational("    2     10 /  4       ").evaluate(null).toString());
        assertEquals("5", new Rational("    3   4   /   2   ").evaluate(null).toString());

        assertEquals("1", new SubtractOp(new Rational("4"), new Rational("3")).evaluate(null).toString());
        assertEquals("2 1/2", new SubtractOp(new Rational("4"), new Rational("1 1/2")).evaluate(null).toString());
        assertEquals("-3", new SubtractOp(new Rational("2"), new Rational("5")).evaluate(null).toString());

        assertEquals("2", new DivideOp(new Rational("4"), new Rational("2")).evaluate(null).toString());
        assertEquals("3/4", new DivideOp(new Rational("3"), new Rational("4")).evaluate(null).toString());
        assertEquals("1", new DivideOp(new Rational("3"), new Rational("3")).evaluate(null).toString());
        assertEquals("57/110", new DivideOp(new Rational("3 4/5"), new Rational("7 1/3")).evaluate(null).toString());
        try {
            new DivideOp(new Rational("1"), new Rational("0")).evaluate(null);
            throw new Exception(); // Should throw ArithmeticException and not reach here.
        } catch (ArithmeticException e) {
            // Expected. Division by zero.
        }
        try {
            new DivideOp(new Rational("0"), new Rational("0")).evaluate(null);
            throw new Exception(); // Should throw ArithmeticException and not reach here.
        } catch (ArithmeticException e) {
            // Expected. Division by zero.
        }

        {
            MultiOperandsOp addOp = new AddOp();
            addOp.addExpression(new Rational("4"));
            addOp.addExpression(new Rational("3"));
            assertEquals("7", addOp.evaluate(null).toString());
        }

        {
            MultiOperandsOp addOp = new AddOp();
            addOp.addExpression(new Rational("1/2"));
            addOp.addExpression(new Rational("1/6"));
            assertEquals("2/3", addOp.evaluate(null).toString());
        }

        {
            MultiOperandsOp addOp = new AddOp();
            addOp.addExpression(new Rational("1/2"));
            addOp.addExpression(new Rational("1/6"));
            addOp.addExpression(new Rational("1/6"));
            assertEquals("5/6", addOp.evaluate(null).toString());
        }

        {
            MultiOperandsOp addOp = new AddOp();
            addOp.addExpression(new Rational("1 1/2"));
            addOp.addExpression(new Rational("2 1/6"));
            addOp.addExpression(new Rational("3 1/6"));
            addOp.addExpression(new Rational("4 1/6"));
            assertEquals("11", addOp.evaluate(null).toString());
        }

        {
            MultiOperandsOp multiplyOp = new MultiplyOp();
            multiplyOp.addExpression(new Rational("4"));
            multiplyOp.addExpression(new Rational("3"));
            assertEquals("12", multiplyOp.evaluate(null).toString());
        }

        {
            MultiOperandsOp multiplyOp = new MultiplyOp();
            multiplyOp.addExpression(new Rational("4 1/2"));
            multiplyOp.addExpression(new Rational("3 1/3"));
            multiplyOp.addExpression(new Rational("2 1/6"));
            assertEquals("32 1/2", multiplyOp.evaluate(null).toString());
        }

        {
            Context context = new Context();
            assertEquals("0", new Identifier("a").evaluate(context).toString()); // Evaluates to zero when not found.

            context.add("a", new Rational("5"));
            assertEquals("5", new Identifier("a").evaluate(context).toString());

            context.add("a", new Rational("7"));
            assertEquals("7", new Identifier("a").evaluate(context).toString());
        }

        {
            Context context = new Context();
            assertEquals("0", new Identifier("a").evaluate(context).toString()); // Evaluates to zero when not found.

            assertEquals("5", new Assignment("a", new Rational("5")).evaluate(context).toString());
            assertEquals("5", new Identifier("a").evaluate(context).toString());

            assertEquals("7", new Assignment("a", new Rational("7")).evaluate(context).toString());
            assertEquals("7", new Identifier("a").evaluate(context).toString());
        }

        {
            // a = 1 4/5
            // b = 2 5/7
            // c = a * b
            // d = a + b + c
            // e = a + b + c + d
            // f = a * b * c * d * e

            Context context = new Context();

            new Assignment("a", new Rational("1 4/5")).evaluate(context);

            new Assignment("b", new Rational("2 5/7")).evaluate(context);

            MultiplyOp cProduct = new MultiplyOp();
            cProduct.addExpression(new Identifier("a").evaluate(context));
            cProduct.addExpression(new Identifier("b").evaluate(context)); 
            new Assignment("c", cProduct).evaluate(context);

            AddOp dSum = new AddOp();
            dSum.addExpression(new Identifier("a").evaluate(context));
            dSum.addExpression(new Identifier("b").evaluate(context));
            dSum.addExpression(new Identifier("c").evaluate(context));
            new Assignment("d", dSum).evaluate(context);

            AddOp eSum = new AddOp();
            eSum.addExpression(new Identifier("a").evaluate(context));
            eSum.addExpression(new Identifier("b").evaluate(context));
            eSum.addExpression(new Identifier("c").evaluate(context));
            eSum.addExpression(new Identifier("d").evaluate(context));
            new Assignment("e", eSum).evaluate(context);

            MultiplyOp fProduct = new MultiplyOp();
            fProduct.addExpression(new Identifier("a").evaluate(context));
            fProduct.addExpression(new Identifier("b").evaluate(context));
            fProduct.addExpression(new Identifier("c").evaluate(context));
            fProduct.addExpression(new Identifier("d").evaluate(context));
            fProduct.addExpression(new Identifier("e").evaluate(context));
            new Assignment("f", fProduct).evaluate(context);

            assertEquals("1 4/5", new Identifier("a").evaluate(context).toString());
            assertEquals("2 5/7", new Identifier("b").evaluate(context).toString());
            assertEquals("4 31/35", new Identifier("c").evaluate(context).toString());
            assertEquals("9 2/5", new Identifier("d").evaluate(context).toString());
            assertEquals("18 4/5", new Identifier("e").evaluate(context).toString());
            assertEquals("4218 10488/30625", new Identifier("f").evaluate(context).toString());
        }

    }

    private static void testScanner() throws Exception {
        {
            Scanner scanner = new ScannerImpl("  A  ");
            assertEquals("index=0, tokens=[A]", scanner.toString());
            assertEquals(1, scanner.getRemainingTokenCount());

            Token peekToken1 = scanner.peek(0);
            assertEquals("A", peekToken1.getValue());
            assertEquals("index=0, tokens=[A]", scanner.toString());
            assertEquals(1, scanner.getRemainingTokenCount());

            Token peekToken2 = scanner.peek(0);
            assertEquals("A", peekToken2.getValue());
            assertEquals("index=0, tokens=[A]", scanner.toString());
            assertEquals(1, scanner.getRemainingTokenCount());

            Token consumedToken1 = scanner.consume();
            assertEquals("A", consumedToken1.getValue());
            assertEquals("index=1, tokens=[A]", scanner.toString());
            assertEquals(0, scanner.getRemainingTokenCount());

            try {
                Token peekToken3 = scanner.peek(0);
                throw new Exception(); // Should throw IndexOutOfBoundsException and not reach here.
            } catch (IndexOutOfBoundsException e) {
                // Expected. No more tokens available after consume();
            }

            try {
                Token consumedToken2 = scanner.consume();
                throw new Exception(); // Should throw IndexOutOfBoundsException and not reach here.
            } catch (IndexOutOfBoundsException e) {
                // Expected. No more tokens available after previous consume();
            }
        }

        {
            Scanner scanner = new ScannerImpl("A");
            assertEquals("index=0, tokens=[A]", scanner.toString());
            assertEquals(1, scanner.getRemainingTokenCount());

            try {
                Token peekToken = scanner.peek(1);
                throw new Exception(); // Should throw IndexOutOfBoundsException and not reach here.
            } catch (IndexOutOfBoundsException e) {
                // Expected. No more tokens available after consume();
            }
        }

        {
            Scanner scanner = new ScannerImpl("      A       B       ");
            assertEquals("index=0, tokens=[A, B]", scanner.toString());
            assertEquals(2, scanner.getRemainingTokenCount());

            Token peekToken0 = scanner.peek(0);
            assertEquals("A", peekToken0.getValue());
            assertEquals("index=0, tokens=[A, B]", scanner.toString());
            assertEquals(2, scanner.getRemainingTokenCount());

            Token peekToken1 = scanner.peek(1);
            assertEquals("B", peekToken1.getValue());
            assertEquals("index=0, tokens=[A, B]", scanner.toString());
            assertEquals(2, scanner.getRemainingTokenCount());

            Token consumedToken0 = scanner.consume();
            assertEquals("A", consumedToken0.getValue());
            assertEquals("index=1, tokens=[A, B]", scanner.toString());
            assertEquals(1, scanner.getRemainingTokenCount());

            Token consumedToken1 = scanner.consume();
            assertEquals("B", consumedToken1.getValue());
            assertEquals("index=2, tokens=[A, B]", scanner.toString());
            assertEquals(0, scanner.getRemainingTokenCount());
        }

        {
            Scanner scanner = new ScannerImpl("A BC DEF");
            assertEquals("index=0, tokens=[A, BC, DEF]", scanner.toString());
        }

        {
            Scanner scanner = new ScannerImpl("A + BC - DEF , ( )");
            assertEquals("index=0, tokens=[A, +, BC, -, DEF, ,, (, )]", scanner.toString());
        }

        {
            Scanner scanner = new ScannerImpl("++");
            assertEquals("index=0, tokens=[+, +]", scanner.toString());
        }

        {
            Scanner scanner = new ScannerImpl("++++ A +++");
            assertEquals("index=0, tokens=[+, +, +, +, A, +, +, +]", scanner.toString());
        }

        {
            Scanner scanner = new ScannerImpl("--");
            assertEquals("index=0, tokens=[-, -]", scanner.toString());
        }

        {
            Scanner scanner = new ScannerImpl("+--+ A ---");
            assertEquals("index=0, tokens=[+, -, -, +, A, -, -, -]", scanner.toString());
        }

        {
            Scanner scanner = new ScannerImpl("+A--B*+*C=(DE))123,,))");
            assertEquals("index=0, tokens=[+, A, -, -, B, *, +, *, C, =, (, DE, ), ), 123, ,, ,, ), )]", scanner.toString());
        }

        {
            Scanner scanner = new ScannerImpl("ABC123 123ABC");
            assertEquals("index=0, tokens=[ABC123, 123ABC]", scanner.toString());
        }
    }

    private static void testParser() throws Exception {
        assertEquals("0", new Parser(new ScannerImpl("0")).parse().evaluate(null).toString());
        assertEquals("1", new Parser(new ScannerImpl("1")).parse().evaluate(null).toString());
        assertEquals("-1", new Parser(new ScannerImpl(" -1")).parse().evaluate(null).toString());
        assertEquals("5", new Parser(new ScannerImpl(" 5 ")).parse().evaluate(null).toString());
        assertEquals("5", new Parser(new ScannerImpl("    5   ")).parse().evaluate(null).toString());

        assertEquals("1/2", new Parser(new ScannerImpl(" 1/2 ")).parse().evaluate(null).toString());
        assertEquals("-1/2", new Parser(new ScannerImpl(" -1/2 ")).parse().evaluate(null).toString());
        assertEquals("2/3", new Parser(new ScannerImpl("2 / 3")).parse().evaluate(null).toString());

        try {
            new Parser(new ScannerImpl("1/0")).parse().evaluate(null);
            throw new Exception(); // Should throw ArithmeticException and not reach here.
        } catch (ArithmeticException e) {
            // Expected. Division by zero.
        }
        try {
            new Parser(new ScannerImpl("0/0")).parse().evaluate(null);
            throw new Exception(); // Should throw ArithmeticException and not reach here.
        } catch (ArithmeticException e) {
            // Expected. Division by zero.
        }

        assertEquals("1/2", new Parser(new ScannerImpl("2/4")).parse().evaluate(null).toString());
        assertEquals("1/2", new Parser(new ScannerImpl("32/64")).parse().evaluate(null).toString());
        assertEquals("1/4", new Parser(new ScannerImpl("30 / 120")).parse().evaluate(null).toString());

        assertEquals("1 2/5", new Parser(new ScannerImpl("7/5")).parse().evaluate(null).toString());
        assertEquals("2 1/4", new Parser(new ScannerImpl("36/16")).parse().evaluate(null).toString());
        assertEquals("5", new Parser(new ScannerImpl("15 / 3")).parse().evaluate(null).toString());
        assertEquals("-3", new Parser(new ScannerImpl("-15 / 5")).parse().evaluate(null).toString());

        assertEquals("1 5/6", new Parser(new ScannerImpl("1 5/6")).parse().evaluate(null).toString());
        assertEquals("2 2/3", new Parser(new ScannerImpl(" 2 4 / 6 ")).parse().evaluate(null).toString());
        assertEquals("4 1/2", new Parser(new ScannerImpl("    2     10 /  4       ")).parse().evaluate(null).toString());
        assertEquals("5", new Parser(new ScannerImpl("    3   4   /   2   ")).parse().evaluate(null).toString());

        assertEquals("1", new Parser(new ScannerImpl("-(4, 3)")).parse().evaluate(null).toString());
        assertEquals("2 1/2", new Parser(new ScannerImpl("-(4, 1 1/2)")).parse().evaluate(null).toString());
        assertEquals("-3", new Parser(new ScannerImpl("-(2, 5)")).parse().evaluate(null).toString());

        assertEquals("2", new Parser(new ScannerImpl("/(4, 2)")).parse().evaluate(null).toString());
        assertEquals("3/4", new Parser(new ScannerImpl("/(3, 4)")).parse().evaluate(null).toString());
        assertEquals("1", new Parser(new ScannerImpl("/(3, 3)")).parse().evaluate(null).toString());
        assertEquals("57/110", new Parser(new ScannerImpl("/(3 4/5, 7 1/3)")).parse().evaluate(null).toString());


        try {
            new Parser(new ScannerImpl("/(1, 0)")).parse().evaluate(null);
            throw new Exception(); // Should throw ArithmeticException and not reach here.
        } catch (ArithmeticException e) {
            // Expected. Division by zero.
        }
        try {
            new Parser(new ScannerImpl("/(0, 0)")).parse().evaluate(null);
            throw new Exception(); // Should throw ArithmeticException and not reach here.
        } catch (ArithmeticException e) {
            // Expected. Division by zero.
        }

        assertEquals("7", new Parser(new ScannerImpl("+(4, 3)")).parse().evaluate(null).toString());
        assertEquals("2/3", new Parser(new ScannerImpl("+(1/2, 1/6)")).parse().evaluate(null).toString());
        assertEquals("5/6", new Parser(new ScannerImpl("+(1/2, +(1/6, 1/6))")).parse().evaluate(null).toString());
        assertEquals("5/6", new Parser(new ScannerImpl("+(1/2, 1/6, 1/6)")).parse().evaluate(null).toString());
        assertEquals("11", new Parser(new ScannerImpl("+(1 1/2, 2 1/6, 3 1/6, 4 1/6)")).parse().evaluate(null).toString());

        assertEquals("12", new Parser(new ScannerImpl("*(4, 3)")).parse().evaluate(null).toString());
        assertEquals("32 1/2", new Parser(new ScannerImpl("*(4 1/2, 3 1/3, 2 1/6)")).parse().evaluate(null).toString());

        {
            Context context = new Context();
            assertEquals("0", new Parser(new ScannerImpl("a")).parse().evaluate(context).toString()); // Evaluates to zero when not found.

            context.add("a", new Rational("5"));
            assertEquals("5", new Parser(new ScannerImpl("a")).parse().evaluate(context).toString());

            context.add("a", new Rational("7"));
            assertEquals("7", new Parser(new ScannerImpl("a")).parse().evaluate(context).toString());
        }

        {
            Context context = new Context();
            assertEquals("0", new Parser(new ScannerImpl("a")).parse().evaluate(context).toString()); // Evaluates to zero when not found.

            assertEquals("5", new Parser(new ScannerImpl("a = 5")).parse().evaluate(context).toString());
            assertEquals("5", new Parser(new ScannerImpl("a")).parse().evaluate(context).toString());
            assertEquals("7", new Parser(new ScannerImpl("a = 7")).parse().evaluate(context).toString());
            assertEquals("7", new Identifier("a").evaluate(context).toString());
        }

        {
            // a = 1 4/5
            // b = 2 5/7
            // c = a * b
            // d = a + b + c
            // e = a + b + c + d
            // f = a * b * c * d * e

            Context context = new Context();
            assertEquals("1 4/5", new Parser(new ScannerImpl("a = 1 4/5")).parse().evaluate(context).toString());
            assertEquals("2 5/7", new Parser(new ScannerImpl("b = 2 5/7")).parse().evaluate(context).toString());
            assertEquals("4 31/35", new Parser(new ScannerImpl("c = *(a, b)")).parse().evaluate(context).toString());
            assertEquals("9 2/5", new Parser(new ScannerImpl("d = +(a, b, c)")).parse().evaluate(context).toString());
            assertEquals("18 4/5", new Parser(new ScannerImpl("e = +(a, b, c, d)")).parse().evaluate(context).toString());
            assertEquals("4218 10488/30625", new Parser(new ScannerImpl("f = *(a, b, c, d, e)")).parse().evaluate(context).toString());

            assertEquals("1 4/5", new Parser(new ScannerImpl("a")).parse().evaluate(context).toString());
            assertEquals("2 5/7", new Parser(new ScannerImpl("b")).parse().evaluate(context).toString());
            assertEquals("4 31/35", new Parser(new ScannerImpl("c")).parse().evaluate(context).toString());
            assertEquals("9 2/5", new Parser(new ScannerImpl("d")).parse().evaluate(context).toString());
            assertEquals("18 4/5", new Parser(new ScannerImpl("e")).parse().evaluate(context).toString());
            assertEquals("4218 10488/30625", new Parser(new ScannerImpl("f")).parse().evaluate(context).toString());
        }
    }
    
    private static void assertEquals(String expected, String actual) throws Exception {
        if (!expected.equals(actual)) {
            System.out.format("expected=%s, actual=%s\n", expected, actual);
            throw new Exception();
        }
    }

    private static void assertEquals(int expected, int actual) throws Exception {
        if (expected != actual) {
            System.out.format("expected=%d, actual=%d\n", expected, actual);
            throw new Exception();
        }
    }

}
```
