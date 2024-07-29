---
title: Yuri, the Programming Assignment and Me
description: The TDD indoctrination of youthful programmers via testing propaganda, OR just show them how it’s beneficial
---

# Introduction
Yuri lives in my neighborhood. He’s roughly my son’s age, and they were good friends growing up a few houses from each other.

Yuri was working on a Computer Science degree at our state university. I told him that I’d always be happy to help him if I could.

# The Arithmetic Evaluator Programming Assignment
A few weeks before COVID shut down the world, Yuri asked if I could help him with a programming assignment in his Data Structures course.

He dropped by the house one evening, and he pulled up this [assignment](https://www.cs.rutgers.edu/courses/112/classes/summer_2019_zhang/progs/prog2/prog2.html). This link is not from the semester when he took it, and it may not even be the same instructor.

I asked Yuri when it was due – two days. I asked him how long he had had it – two weeks. After I scolded him for procrastinating, I hoped I could give him enough direction so that he could complete some of it before the due date.

My first thought when reading the assignment was, “Those are [technical] words.” This remains one of the most poorly organized assignment descriptions I think I have ever seen. Maybe the instructor was trying to get the students prepared for a career of bad system requirements. For comparison, Princeton’s Data Structures course has great [assignment descriptions]( https://www.cs.princeton.edu/courses/archive/spring24/cos226/assignments.php).

> __Side Note:__ We received weekly programming assignments when I was in college. I recall receiving one assignment and completing it before the next class. In the next class a few students complained that it was too difficult. Our instructor changed the assignment on the spot. Several of us had completed the original assignment, and we argued that we shouldn’t be subjected to another assignment when we had completed the original one.
> 
> He looked at us and said, “Requirements change. Get used to it.”
>
> __This was probably the most valuable lesson I learned in that class.__ 

## The Gist of the Assignment
After about an hour of reading and rereading Yuri’s assignment and looking at the programming artifacts provided, I thought I had a good sense of what was needed.
Here’s the gist:
* The assignment evaluates an arithmetic expression represented as a `String`.
* It supports the basic arithmetic operations.
* It supports parentheses.
* It supports a read-only datastore of variables.
* It supports a read-only datastore of arrays. The array value initialization formatting looked like something you’d see in an [Advent of Code](https://adventofcode.com/) challenge.
* Some helper classes are provided. Some classes are incomplete, and the students need to provide implementations for some methods.

# Let’s Try Something Different
I still wasn’t 100% sure of my interpretation, and I needed to get Yuri started with something.
The assignment was in Java and his IDE was Eclipse, both the same technologies I was using at work at that time.

I suggested a quick experiment. I created the following method:
```java
    @Test
    public void test() throws Exception() {
    }
```

We ran the test in Eclipse, and it passed. Sweet. His IDE had [JUnit](https://en.wikipedia.org/wiki/JUnit). I was going to see if [Test-Driven Development](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) (TDD) would work for him.

# Test-Driven Development
I’ll recreate test and code examples of what we did for this blog, but this was four and a half years ago. I don’t have access to the assignment software artifacts for details. My memory is a bit fuzzy so this will be a memoir of that evening. More on the fuzziness in the [second epilog](#second-epilog).

The method signatures are not provided in the assignment. They are defined within the inaccessible artifacts package; however, the assignment hints at method signatures. For example, here's a glimpse of the `evaluate` method from the assignment:
```java
float res = evaluate(expr.substring(3,11), vars, arrays);
```

The assignment example indicates that the evaluation returns a `float`. It also indicates that `vars` and `arrays` datastore objects are arguments, which along with no object associated with `evaluate(...)` suggests that it is a static method. If I had access to its method signature, I'd know for sure.

Since `vars` and `arrays` are read-only datastores, I feel that a better design might have been an `Evaluator` class, which declared private field attributes for `vars` and `arrays` and initialized them in its constructor. That would have changed the method signature to:
```java
public float evaluate(String expression);
```

However, if doing this, then an `Evaluator` object would have been needed rather than a direct call to the static `evaluate(...)`. But this is neither here nor there for what Yuri needed.

I am going to declare that `evaluate(...)` returns an `int` in my examples. `int` equality is more specific than `float` equality. I’m also going to take some liberties with the read-only datastore. My `vars` datastore will be `Map<String, Integer>`. I will not provide an `arrays` datastore.

## The first test
Yuri and I followed the examples in the assignment, which recommended evaluating “3”. We wrote this test:
```java
    @Test
    public static void evaluate_Returns3_WhenEvaluating3() throws Exception {
        assertEquals(3, evaluate("3", null));
    }
```

The `null` is for the variables datastore, which is not accessed in this expression. It didn’t compile, because `evaluate(...)` didn’t exist.

I convinced Yuri to implement the method like this:
```java
    public static int evaluate(String expression, Map<String, Integer> variables) throws Exception {
        return 3;
    }

```

“See. It passes,” I exclaimed with delight. Yuri eyed me suspiciously. I asked him to be a little patient and have a little faith. I assured Yuri that this was just the first step. We’d have more.

## The second test
The next test:
```java
    @Test
    public static void evaluate_Returns4_WhenEvaluating4() throws Exception {
        assertEquals(4, evaluate("4", null));
    }
```

Of course, this failed. I had Yuri change the implementation to return `4`. The new test passed, but the previous one failed. We’re going to need a more generic implementation.

What could we do? How about this:
```java
    public static int evaluate(String expression, Map<String, Integer> variables) throws Exception {
        return Integer.valueOf(expression);
    }
```

Bingo. Now both tests pass.

## What about a variable?
```java
    @Test
    public void evaluate_Returns5_WhenEvaluatingVariableAassignedTo5() throws Exception {
        Map<String, Integer> variables = new HashMap<>();
        variables.put("A", 5);
        assertEquals(5, evaluate("A", variables));
    }
```

This failed with `java.lang.NumberFormatException: For input string: "A"`.

The implementation to cover the three existing tests seemed mostly obvious:
```java
    public static int evaluate(String expression, Map<String, Integer> variables) throws Exception {
        try {
            return Integer.valueOf(expression);
        } catch (NumberFormatException e) {
            return variables.get(expression);
        }
    }
```

And the tests passed.

## Let’s try addition
```java
    @Test
    public static void evaluate_Returns7_WhenEvaluating3Plus4() throws Exception {
        assertEquals(7, evaluate("3+4", null));
    }
```

This is a bit more challenging. I asked Yuri if he understood recursion. He did. Great.

Notice that `3` and `4`, while operands, are expressions as well. It took a few tries to get the substring boundaries correct, but we got them working:
```java
    public int evaluate(String expression, Map<String, Integer> variables) throws Exception {
        int plusIndex = expression.indexOf("+");
        if (plusIndex > 0) {
            return evaluate(expression.substring(0, plusIndex), variables) +
                   evaluate(expression.substring(plusIndex+1), variables);
        }

        try {
            return Integer.valueOf(expression);
        } catch (NumberFormatException e) {
            return variables.get(expression);
        }
    }
```

## A little off the top
The assignment states that the expression can have whitespaces. Let’s duplicate the passing tests and add whitespace:
```java
    public void evaluate_with_whitespaces() throws Exception {
        assertEquals(3, evaluate("          3               ", null));
        assertEquals(4, evaluate("  4       ", null));
        assertEquals(7, evaluate("  3   +   4 ", null));
        Map<String, Integer> variables = new HashMap<>();
        variables.put("A", 5);
        assertEquals(5, evaluate("  A       ", variables));
    }
```

They fail. But one quick String `trim()` cleaned them right up, and they passed.
```java
    public static int evaluate(String expression, Map<String, Integer> variables) throws Exception {
        expression = expression.trim();
        
        int plusIndex = expression.indexOf("+");
        if (plusIndex > 0) {
            return evaluate(expression.substring(0, plusIndex), variables) +
                   evaluate(expression.substring(plusIndex+1), variables);
        }

        try {
            return Integer.valueOf(expression);
        } catch (NumberFormatException e) {
            return variables.get(expression);
        }
    }

```

## This was going well... maybe too well.
We got multiplication to work. It was mostly a copy-and-paste of the addition implementation.

We were slicing through the assignment like a hot knife through butter. I felt that Yuri needed to work on the assignment on his own, and if I didn’t send him home soon, we would have finished most of the assignment in short order.

Yuri said that he felt confident enough to continue on his own.

We had not tried expressions with different multiple operators such as `3 + 4 * 5`. This must evaluate `4 * 5` first to get `20` and then add `3` to get `23`.

I suggested that he didn’t have to over think how to get this to work. Create a test that declared an expected evaluation of `23`, and then try it. We had already implemented addition and multiplication. If the test passed, then it evaluated addition and multiplication operations in the correct order returning `23`. If it failed and returned `35`, then the order of operator processing is in the wrong order. Swap the order of operator processing and test again.

Do this for the other operators too. Create a few more complicated expressions. Test and adjust takes less thinking effort and time than adjust and test.

I sent him on his way.

# That’s an Excellent Question
I ran into Yuri a week or so later. I asked if he got the assignment done. He said that he got it working except for the parentheses, which he stated were extra credit. Either my memory is faulting about this, or the assignment I found online is different from his, since it doesn’t list parentheses as extra credit.

Then he added, “___That testing technique you showed me was really useful. Why aren’t they teaching us that in class?___”

__Exactly! That is an excellent question.__

# TDD is Introduced Too Late
I don’t know why TDD is not presented in introductory programming classes. I suspect that academia doesn’t know or understand the practice. Some students might be exposed to it in a Software Engineering class, but by then, I think it’s too late.

By the time most developers are first exposed to TDD, they’ve spent years programming. It’s not part of their discipline. The window of receptiveness slams shut quickly. And then the practice might only progress slowly, one promotion to management, one retirement, or one funeral at a time.

The absence of TDD in introductory programming classes is the third reason I alluded to in the [previous blog]( https://jhumelsine.github.io/2024/07/15/tdd.html#why-are-developers-so-reluctant-to-write-tests-first) as to why developers are reluctant to try TDD. It was never taught to them during their formative years, and there’s a lot of reluctance for them to change how they’ve been doing things from the start.

I firmly believe that if TDD were taught in introductory programming, then the practice would be so second nature that developers would shudder at the thought of writing any code without writing a specifying test first.

I envision an introductory TDD curriculum along these lines:
* Provide all tests in introductory programming with the assignments where they are deactivated except the first one. Instruct students to focus upon getting one test to pass at a time. Activate a new test as the previously activated tests pass and get the new test to pass without breaking the previously activated tests.
* Educate them about the provided tests. Focus upon why they are useful and how they would go about designing their own.
* Wean students off tests by providing fewer and fewer as they learn more about creating their own tests. Maybe start by describing the behavior to test and let them write the test that specifies that behavior.
* Once students have learned how to create their own tests, then don’t provide any tests. Let them create their own tests.

# Summary
This story demonstrated the TDD process on a small scale. Yuri and I started with the most basic test cases that were simple to implement. We layered in more complex test scenarios and updated the implementation accordingly.

TDD allowed us to approach an intimidating problem one step at a time with the confidence that as we added new behaviors, we weren’t breaking existing ones.

TDD should be introduced as a tool early in a programmer’s education. Then it’s an informed choice as to whether they choose to use it or not.

# First Epilog
I asked Yuri if I could tell this story. He responded with:

> I’m perfectly okay with the story being told as I still use that test-driven development concept when I've had to write powershell scripts for work. Most recently, was my unit-testing for creating a script for doing a litigation hold across a client in 365 and exporting it into excel, as well as another one for creating a desktop shortcut by scraping it from a website and deploying it into the desktops of our client. Broken down into their smaller functions of course so I could build up on.

__His email made me very happy.__

# Second Epilog
I invited Yuri to review an early draft. I wanted to make sure that I didn't misrepresent our evening. He has slightly different memories. I don't think our individual memories are inconsistent, but maybe different parts of the evening impressed each of us differently.

Yuri remembers our struggles with the [StringTokenizer](https://docs.oracle.com/javase/8/docs/api/java/util/StringTokenizer.html). I have no memory of attempting to isolate tokens. We may have attempted to extract tokens from the expression. We may have also attempted to extract index values which are defined like sparse array.

I did not parse tokens in my recreation examples. I'm using only enough `String` processing to identify operator token locations and isolate substrings. I am leaning heavily upon recursion to handle the rest as described with more detail in [Third Epilog](#third-epilog). The assignment recommends recursion:
> While recursion is optional for this assignment, using it to evaluate subexpressions will make it a LOT easier to write working code. (This is a great opportunity to learn how to use recursion in a realistic situation!!)

I've been thinking about my recursion-based recreation implementation, and I'm pretty sure that it has a mistake. For now, I don't care. I will describe flaw, but I won't correct the code.

I'm near 100% sure that the addition and multiplication code will evaluate right-to-left rather than left-to-right. None of the test cases failed. And to be on the safe side, I added these additional tests with multiple additions and multiplications in the same expression to confirm this:
```java
    @Test
    public void evaluate_handlesMultipleAdditionAndMultiplications() throws Exception {
        assertEquals(10, evaluate("1+2+3+4", null));
        assertEquals(10, evaluate("4+3+2+1", null));
        assertEquals(24, evaluate("1*2*3*4", null));
        assertEquals(24, evaluate("4*3*2*1", null));
    }
```

These pass because additional and multiplication are commutative. The order of operations doesn't matter. Right-to-left is just as good as left-to-right.

The relative order of performing multiplication before addition is enforced in the implementation with the addition token being handled before the multiplication token. It feels like we're evaluating the addition before we're evaluating the multiplication, but that's not the case. And we don't need to over analyze it. The following tests confirm that the addition/multiplication evaluation is in the correct order:
```java
    @Test
    public void evaluate_Returns23_WhenEvaluating3Plus4Times5() throws Exception {
        assertEquals(23, evaluate("3 + 4 * 5", null));
        assertEquals(23, evaluate("5 * 4 + 3", null));
    }
```

Even if the implementation is not 100% correct with respect to the rules of arithmetic evaluation, it doesn't matter. The tests still pass. `evaluate(...)` is returning expected values.

However, my arithmetic evaluator code hasn't completed the assignment. It doesn't evaluate subtraction or division, and that's when I think that left-to-right evaluation will become important. Addition/multiplication are in different order of evaluation hierarchies, which the current implementation supports, whereas addition/subtraction are on the same evaluation classification, and left-to-right evaluation resolves any evaluation ambiguities.

If I were to continue with the assignment, I would do the following:
* Create tests for basic subtraction and division functionality.
* Implement subtraction and division code that mirrors the addition and multiplication implementations.
* Create more sophisticated tests that contain different arithmetic operations. I think these tests whould fail, since the order of operations won't be correct.
* Rather than looking for the operator tokens using `indexOf("+")`, etc., look for them using `lastIndexOf("+")`, etc. This will find the index of the last one rather than the first one. This should implement left-to-right evaluation, but I don't need to over analyze it. The tests would confirm when the implementation is correct.
* `lastIndexOf(...)` still won't be sufficient. Rather than looking for just the `lastIndexOf("+")`, the implementation will need to look for the last index of either `+` or `-`, and perform the appropriate operation between the two recursion calls. The same applies for multiplication/division. A regular expression implementation may work better. And even if this is a better solution, I should not implement it until I have a need to implement it. And I won't have that need until I have failing tests that force me to implement it.
* And while the code is being pulled apart and rebuilt to handle subtraction and division, the previous tests are still confirming that we're not breaking any previous functionality in the process.

# Third Epilog
As I was writing this blog and recreating the code, I became more interested in how to implement the parentheses behavior, which Yuri and I didn't attempt. It’s different than the other behaviors. It’s not a binary arithmetic operation or a variable look up. It’s about grouping and order of precedence. I decided to tackle it.

I started with simple parentheses infused expressions and worked my way toward more complex ones. I lumped them all into one test, which I normally wouldn’t do, but I felt it was okay for this exercise. _Learn the rules, and then break the rules_.
```java
    @Test
    public void evaluate_handlesParentheses() throws Exception {
        assertEquals(4, evaluate("(4)", null));
        assertEquals(4, evaluate("((4))", null));
        assertEquals(7, evaluate("((3+4))", null));
        assertEquals(14, evaluate("(3 + 4) * 2", null));
        assertEquals(14, evaluate("(3+4)*2", null));
        assertEquals(18, evaluate("3 * (2 + 4)", null));
        assertEquals(6, evaluate("(2+4)", null));
        assertEquals(18, evaluate("3*(2+4)", null));
        assertEquals(42, evaluate("3 * (2 + (4 * (1 + 2)))", null));
        assertEquals(42, evaluate("3*(2+(4*(1+2)))", null));
        assertEquals(21, evaluate("(1+2)*(3+4)", null));
        assertEquals(21, evaluate("  (   1 +    2   ) * (  3   + 4)     ", null));
        assertEquals(1626, evaluate("3 * (2+(4*(1+2) * (2 + 3)) * (4 + 5))", null));
    }
```

I got each assert to pass before adding a new one. I won’t provide evolution of the implementation but suffice it to say the first version assumed that the parentheses were the first and last characters in the expression. It returned the recursive `evaluate` of the characters between them. This worked for the first three tests but failed for the fourth when I needed to move toward a more generic solution.

Before I started any implementation, I thought the solution would mostly consist of String processing. But as I was layering in more tests and moving toward a more generic solution, I had time to think and experiment. There may be an elegant approach to this problem.

If I can identify the first opening and closing matching parentheses in the expression, then the expression will be of the form:
```
preParenthesesExpression ( parenthesesBoundedExpression ) postParenthesesExpression
```
A few notes about the form above:
* This is the original expression chopped into pieces using the parentheses as the delimiters.
* The opening delimiting parenthesis is the first one in the expression.
* The closing delimiting parenthesis is the pair match to the opening delimiter. It is not necessarily the first closing parameter in the expression. Nor is it necessarily the last closing parameter in the expression.
* The `preParenthesesExpression` may be an empty string. It will not contain an opening parenthesis, since the opening delimiting parentheses is the first one in the expression.
* The `parenthesesBoundedExpession` will not be empty. It may also contain additional parentheses.
* The `postParenthesesExpression` may be any empty string. I may also contain additional parentheses.

The evaluation of the parentheses delimited expression is doubly nested recursive calls to `evaluate(...)`, where `+` is String concatenation:
```
evaluate(preParenthesesExpression + evaluate(parenthesesBoundedExpression) + postParenthesesExpression)
```

As an example, if the original expression were `3 * (4 + 5) * 2`, then the inner recursive call would `evaluate("4 + 5")` returning `9`. Then the outer recursive call would `evaluate("3 * 9 * 2")` returning `54`. Since each recursive call is an expression shorter than the original expression, the recursion will always work.

My main challenge was off-by-one errors in the substring indexing. I even broke `(4)` as I was working on the more general implementation. But I knew I had broken the test immediately, and I adjusted accordingly.

The parentheses specific implementation took about an hour from start to finish. This would not have been my first approach if I had tried to implement all the behaviors at the start. However, upon seeing this implementation, it just feels right, possibly elegant.

```java
    public static int evaluate(String expression, Map<String, Integer> variables) throws Exception {
        expression = expression.trim(); // Remove pre and post whitespace

        // Parentheses Evaluation
        int openParenIndex = expression.indexOf("("); // First opening parenthesis index
        if (openParenIndex >= 0) {
            // Matching closing parenthesis index
            int closeParenIndex = openParenIndex + getClosingParen(expression.substring(openParenIndex));

            // Evaluate and return a shorter version of original expression
            return evaluate(
                    expression.substring(0, openParenIndex) +
                    // Evalate parentheses bounded expression
                    evaluate(expression.substring(openParenIndex+1, closeParenIndex), variables) +
                    expression.substring(closeParenIndex+1, expression.length()),
                variables);
        }

        // Addition Evaluation
        int plusIndex = expression.indexOf("+");
        if (plusIndex > 0) {
            return evaluate(expression.substring(0, plusIndex), variables) +
                   evaluate(expression.substring(plusIndex+1), variables);
        }

        // Multiplication Evaluation
        int timesIndex = expression.indexOf("*");
        if (timesIndex > 0) {
            return evaluate(expression.substring(0, timesIndex), variables) *
                   evaluate(expression.substring(timesIndex+1), variables);
        }

        try {
            // Integer Resolution
            return Integer.valueOf(expression);
        } catch (NumberFormatException e) {
            // Variable Resolution
            return variables.get(expression);
        }
    }

    private static int getClosingParen(String expression) {
        int counter = 0;
        for (int i = 0; i < expression.length(); i++) {
            if ("(".equals(expression.substring(i, i+1))) counter++;
            if (")".equals(expression.substring(i, i+1))) counter--;
            if (counter == 0) return i;
        }
        return -1;
    }

```

I haven’t attempted the arrays, but I think they would use the same techniques that were used for variables and the parentheses, except the `evaluate` value from the bracket bounded expression substring would be an index into the array.

And if I were to continue with the assignment and add subtraction and division, I would have to resove the right-to-left versus left-to-right evaluation order as I described in the [Second Epilog](#second-epilog).

# References
* Previous [blog references](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#references)
