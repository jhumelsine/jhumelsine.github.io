---
title: DRAFT - Yuri, the Programming Assignment and Me
description: The TDD indoctrination of youthful programmers via testing propaganda, OR just show them it’s beneficial
unlisted: true
---

# Introduction
Yuri lives in my neighborhood. He’s roughly my son’s age, and they were good friends growing up a few houses from each other.

Yuri was working on a Computer Science degree at our state university. I told him that I’d always be happy to help him if I could.

# The Arithmetic Evaluator Programming Assignment
A few weeks before COVID shut down the world, Yuri asked if I could help him with a programming assignment in his Data Structures course.

He dropped by the house one evening, and he pulled up this [assignment](https://www.cs.rutgers.edu/courses/112/classes/summer_2019_zhang/progs/prog2/prog2.html). This link is not from the semester when he took it, and it may not even be the same instructor.

I asked Yuri when it was due – two days. I asked him how long he had had it – two weeks. After I scolded him for procrastinating, I hoped I could give him enough pointers so that he could complete some of it before the due date.

My first thought when reading the assignment was, “Those are [technical] words.” This remains the most poorly organized assignment description I think I have ever read. Maybe the instructor was trying to get the students prepared for a career of bad system requirements. For comparison, Princeton’s Data Structures course has great [assignment descriptions]( https://www.cs.princeton.edu/courses/archive/spring24/cos226/assignments.php).

> __Side Note:__ We got weekly programming assignments when I was in college. I recall receiving one assignment and completing it before the next class. In the next class a few students complained that it was too difficult. Our instructor changed the assignment on the spot. Several of us had completed the original assignment, and we argued that we shouldn’t be subjected to another assignment when we had completed the original one.
> 
> He looked at us and said, “Requirements change. Get used to it.”

__This was probably the most valuable lesson I learned in that class.__ 

## The Gist of the Assignment
After about an hour of reading and rereading Yuri’s assignment and looking at the programming artifacts provided, I thought I had a good sense of what was needed.
Here’s the gist:
* The assignment evaluates an arithmetic expression string.
* It supports the basic arithmetic operations.
* It supports parentheses.
* It supports a read-only datastore of variables.
* It supports a read-only datastore of arrays. The array value initialization formatting looked like something you’d see in an [Advent of Code](adventofcode.com) challenge.

# Let’s Try Something Different
I still wasn’t 100% sure of my interpretation, and I needed to get Yuri started with something.
The assignment was in Java and his IDE was Eclipse, both the same technologies I was using at work at that time.

I suggested a quick experiment. I created the following method:
```java
    @Test
    public void test() throws Exception() {
    }
```

We ran the test in Eclipse, and it passed. Sweet. His IDE had JUnit. I was going to see if [Test-Driven Development TBD](https://jhumelsine.github.io/2024/07/15/tdd.html#test-driven-development) (TDD) would work for him.

# Test-Driven Development
I’ll recreate test and code examples of what we did for this blog, but this was four and a half years ago. I don’t have access to the software artifacts for details. My memory is a bit fuzzy so this will be a memoir of that evening. More on the fuzziness in the [second epilog](#second-epilog).

The assignment suggests that the evaluation returns a `float`, but the examples suggest an `int`. I’ll stick with `int` in my examples here. I’m also going to take some liberties with the read-only datastore. My examples will not include the arrays.

## The first test
We followed the examples in the assignment, which recommended evaluating “3”. We wrote this test:
```java
    @Test
    public static void evaluate_Returns3_WhenEvaluating3() throws Exception {
        assertEquals(3, evaluate("3", null));
    }
```

The `null` is for the variables datastore. It didn’t compile, because `evaluate` didn’t exist.

I convinced him to implement the method like this:
```java
    public static int evaluate(String expression, Map<String, Integer> variables) throws Exception {
        return 3;
    }

```

“See. It passes,” I said. Yuri eyed me suspiciously. I asked him to be a little patient and have a little faith.

## The second test
I assured Yuri that this was just the first step. We’d have more. The next test:
```java
    @Test
    public static void evaluate_Returns4_WhenEvaluating4() throws Exception {
        assertEquals(4, evaluate("4", null));
    }
```

Of course, this failed. I had Yuri change the implementation to return `4`. The new test passed, but it broke the previous one. We’re going to need a more general implementation.

We could make both pass with a `switch` statement, but that won’t scale to all possible integers. Hmmm. What else could we use? How about this:
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

This failed with a `java.lang.NumberFormatException: For input string: "A"`.

This time we’re not going to make it pass with a hard-coded value. The implementation to cover the three existing tests was obvious:
```java
    public static int evaluate(String expression, Map<String, Integer> variables) throws Exception {
        try {
            return Integer.valueOf(expression);
        } catch (NumberFormatException e) {
            return variables.get(expression);
        }
    }
```

## Let’s try addition
```java
    public static void evaluate_Returns7_WhenEvaluating3Plus4() throws Exception {
        assertEquals(7, evaluate("3+4", null));
    }
```

This is a bit more challenging. I asked Yuri if he understood recursion. He did. Great.

Notice that `3` and `4`, while operands, are expressions as well. It took a few tries to get the substring boundaries correct, but we got them working:
```java
    @Test
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
The assignment states that the expression can have whitespaces. Let’s redo some of the tests with whitespace:
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

They fail. But one quick String `trim()` will clean them right up.
```java
    public static int evaluate(String expression, Map<String, Integer> variables) throws Exception {
        expression = expression.trim();
        
        int plusIndex = expression.indexOf("+");
        if (plusIndex > 0) {
            return evaluate(expression.substring(0, plusIndex), variables) + evaluate(expression.substring(plusIndex+1), variables);
        }

        try {
            return Integer.valueOf(expression);
        } catch (NumberFormatException e) {
            return variables.get(expression);
        }
    }

```

## This was going well... maybe too well.
This was going well. We were slicing through the assignment like a hot knife through butter.

We got multiplication to work. It was mostly a copy-and-paste of the addition implementation.

Maybe it was going too well. I felt that Yuri needed to work on the assignment on his own, and if I didn’t send him home soon, we would have finished in short order.

Yuri said that he felt confident enough to complete it on his own.

We had not tried expressions with different multiple operators such as `3 + 4 * 5`. This must evaluate `4 * 5` first to get `20` and then add `3` to get `23`.

I suggested that he didn’t have to overthink how to get this to work. Create a test that declared an expected evaluation of `23`, and then try it. We had already implemented addition and multiplication. If the test passed, then it evaluated in the correct order returning `23`.

If it failed and returned `35`, then the order of operator processing is in the wrong order. Swap them and test again.

Do this for the other operators too. Create a few more complicated expressions. Test and adjust takes less thinking effort and time than adjust and test.

I sent him on his way.

# That’s an Excellent Question
I ran into Yuri a week or so later. I asked if he got the assignment done. He said that he got it working except for the parentheses, which he stated were extra credit. Either my memory is faulting about this, or the assignment I found online is different from his, since it doesn’t list parentheses as extra credit.

Then he added, “That testing technique you showed me was really useful. Why aren’t they teaching us that in class?”

__Exactly! That is an excellent question.__

# TDD is Introduced Too Late
I don’t know why TDD is not introduced in introductory programming classes. I suspect that academia doesn’t know or understand the practice. Some students might be exposed to it in a Software Engineering class, but by then, I think it’s too late.

By the time most developers are first exposed to TDD, they’ve spent years programming. It’s not part of their discipline. The window of receptiveness slams shut quickly. And then the practice progresses slowly one promotion to management, one retirement or one funeral at a time.

This is the third reason I alluded to in the [previous blog]( https://jhumelsine.github.io/2024/07/15/tdd.html#why-are-developers-so-reluctant-to-write-tests-first) as to why developers are reluctant to try TDD. It was never taught to them during their formative years, and there’s a lot of reluctance for them to change how they’ve been doing things for years.

I firmly believe that if TDD were taught in introductory programming, then the practice would be so second nature that developers would shutter at the thought of writing any code without writing a specifying test first.

I envision an introductory TDD curriculum along these lines:
* Provide all tests in introductory programming with the assignments. Deactivate them except the first one. Instruct students to focus upon getting one test to pass at a time. Activate a new test as the previously activated tests pass and get the new test to pass without breaking the previously activated tests.
* Educate them about the provided tests. Focus upon why they are useful and how they would go about designing their own.
* Ween students off tests by providing fewer and fewer as they learn more about creating their own tests.
* Once students have learned how to create their own tests, then don’t provide any tests. Let them create their own tests.

# Summary
This story demonstrated the TDD process on a small scale. Yuri and I started with the most basic test cases that were simple to implement. We layered in more complex test scenarios and updated the implementation accordingly.

TDD allowed us to approach an intimidating problem one step at a time with the confidence that as we added new behaviors, we weren’t breaking existing ones.

TDD should be introduced as a tool early in a programmer’s education. Then it’s an informed choice as to whether to use it or not.

# First Epilog
I asked Yuri if I could tell this story. He responded with:

> I’m perfectly okay with the story being told as I still use that test-driven development concept when I've had to write powershell scripts for work. Most recently, was my unit-testing for creating a script for doing a litigation hold across a client in 365 and exporting it into excel, as well as another one for creating a desktop shortcut by scraping it from a website and deploying it into the desktops of our client. Broken down into their smaller functions of course so I could build up on.

__His email reply made me very happy.__

# Second Epilog
After I had posted a draft of this blog, I invited Yuri to review it. I wanted to make sure that I didn't misrepresent our evening. He has slightly different memories. I don't think they're inconsistent, but maybe different parts of the evening impressed each of us differently.

Yuri remembers our struggles with the [StringTokenizer](https://docs.oracle.com/javase/8/docs/api/java/util/StringTokenizer.html), which the assignment warned is a legacy class retained only for backward compatability. New implementations should not use it. But for the purpose of this assignment, it could be used along with several other suggestions, such as [Pattern](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Pattern.html) and [Matcher](https://docs.oracle.com/javase/8/docs/api/java/util/regex/Matcher.html). Though not listed in the assignment, [Scanner](https://docs.oracle.com/javase/8/docs/api/java/util/Scanner.html) might be a reasonable option as well.

I have no memory of attempting to isolate tokens. We may have attempted to extract tokens from the expression. We may have also attempted to extract index values which are defined more like sparse array, with zero values omitted, or a `Map` of __Integer => Integer__ where the `key` is the index into the array and the `value` is the array's index value.

Here's the assignment's description of variable and array values:
> Since the expression has a variable, a, the evaluator needs to be supplied with a file that has a value for it. Here's what etest1.txt looks like, and in this example lower-case letters are variable declarations and defintions and upper-case letters are array declarations and definitions:
> * a 3
> * b 2
> * A 5 (2,3) (4,5)
> * B 3 (2,1)
> * d 56

Yuri doesn't recall the use of a `Map` for the variable resolution as I have implemented in my reconstruction. This is because the assignment provided helper classes for the datastore. It mentions a `Variable` class and `Array` class are provided for the variable and array datastores respectively. Their definitions are in the assignment package, which are inaccessible unless registered in the class. I'm sure we used the provided classes back then, but I used a `Map` for the recreation here.

I don't remember using tokens for expression processing, nor did I use it in the recreation. I'm using just enough `String` processing to identify operator token locations and isolate substrings. Then I leaned heavily upon recursion to handle the rest as described with more detail in [Third Epilog](#third-epilog). The assignment recomments recursion:
> While recursion is optional for this assignment, using it to evaluate subexpressions will make it a LOT easier to write working code. (This is a great opportunity to learn how to use recursion in a realistic situation!!)

I was thinking about my recursion based recreation implemenation, and I'm pretty sure that it has a mistake. But guess what? For right now, I don't care.

I'm near 100% sure that the addition and multiplicate code will evaluate right-to-left rather than left-to-right. But none of the test cases fail. And just to be on the safe side, I added these additional tests to confirm this:
```java
    @Test
    public void evaluate_handlesMultipleAdditionAndMultiplications() throws Exception {
        assertEquals(10, evaluate("1+2+3+4", null));
        assertEquals(10, evaluate("4+3+2+1", null));
        assertEquals(24, evaluate("1*2*3*4", null));
        assertEquals(24, evaluate("4*3*2*1", null));
    }
```

These pass because additional and multiplication are communative. The order of operations doesn't matter. Right-to-left is just as good as left-to-right is just as good as outer to inner or inner to outer.

The relative order of performing multiplication before addition is enforced in the implementation with the addition token being processed before the multiplication token. This feels backwards, since it feels like we're performing the addition before we're performing the multiplication, but that's not the case. And we don't to over analyze it. The following tests confirm it:
```java
    @Test
    public void evaluate_Returns23_WhenEvaluating3Plus4Times5() throws Exception {
        assertEquals(23, evaluate("3 + 4 * 5", null));
        assertEquals(23, evaluate("5 * 4 + 3", null));
    }
```

The point is that even if the implementation is not 100% correct with respect to the rules of arithmetic evaluation, it doesn't matter. All of the tests still work. It will evaluate the expected result.

However, my arithmetic evaluator code hasn't completed the assignment. It doesn't evaluate subtraction or division, and that's when I think that left-to-right evaluation will become important. That's because addition/multiplication are in different order of evaluation hierarchies, which the current implementation supports, whereas addition/subtraction are on the same evaluation classification, and left-to-right evaluation resolves any evaluation ambiguities.

If I were to continue with the assignment, I would do the following:
* Create tests for basic subtraction and division functionality.
* Implement subtraction and division code that mirros the addition and multiplication code.
* Create more sophisticated tests that contain different arithmetic operations.
* They will fail, since order of operations won't be correct.
* Rather than looking for the operator tokens using `indexOf("+")`, etc., look for them using `lastIndexOf("+")`, etc. This will find the index of the last one rather than the first one. This should implement left-to-right evaluation.
* But that still won't be enough. Rather than looking from just the `lastIndexOf("+")`, the implementation will need to look for the last index of either `+` or `-`, and perform the appropriate operation between the two recursion calls. The same applies for multiplication/division.
* And while the code is being pulled apart and rebuilt to handle subtraction and division, the previous tests still be confirming that we're not breaking any previous functionality in the process.

# Third Epilog
As I was writing this blog and recreating the code, I became more interested in how to implement the parentheses behavior. It’s different than the other behaviors. It’s not a binary arithmetic operation or a variable look up. It’s about grouping and order of precedence. I decided to tackle it.

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

I got each assert to pass before adding a new one. I won’t provide evolution of the implementation but suffice it to say the first version assumed that the parentheses were the first and last characters in the expression. It returned the recursive `evaluate` of the characters between them. This worked for the first three tests but failed for the fourth when I needed to move toward a more general solution.

Before I started any implementation, I thought the solution would mostly consist of String processing. But as I was layering in more tests and moving toward a more general solution, I had some time to think and experiment. There may be an elegant approach to this problem.

If I can identify the first opening and closing matching parentheses in the expression, then the expression will be of the form:
```
preParenthesesExpression ( parenthesesBoundedExpression ) postParenthesesExpression
```
A few notes about the form above:
* This is the original expression chopped into pieces using the parentheses as the delimiters.
* The opening delimiting parenthesis is the first one in the expression.
* The closing delimiting parenthesis is the pair match to the opening delimiter. It is not necessarily the first closing parameter in the expression. Nor is it necessarily the last closing parameter in the expression.
* The `preParenthesesExpression` may be an empy string. It will not contain an opening parenthesis, since the opening delimiting parentheses is the first one in the expression.
* The `parenthesesBoundedExpession` will not be empty. It may also contain additional parentheses.
* The `postParenthesesExpression` may be any empty string. I may also contain additional parentheses.

The evaluation of the parentheses delimited expression is doubly nested recursive calls to `evaluate(...)`, where `+` is concatination:
```
evaluate(preParenthesesExpression + evaluate(parenthesesBoundedExpression) + postParenthesesExpression)
```

As an example, if the original expression were `3 * (4 + 5) * 2`, then the inner recursive call would `evaluate("4 + 5")` returning `9`. Then the outer recursive call would `evaluate("3 * 9 * 2")` returning `54`. Since each recursive call is an expression shorter than the original expresion, the recursion will always work.

My main challenge was off-by-one errors in the substring indexing. I even broke `(4)` as I was working on the more general implementation. But I knew I had broken the test immediately, and I adjusted accordingly.

The parentheses specific implementation took about an hour from start to finish to complete. This would not have been my first approach if I had tried to implement all the behaviors at the start. However, upon seeing this implementation, it just feels right, possibly elegant.

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

# References
* Previous [blog references](https://jhumelsine.github.io/2024/06/07/unit-test-convert.html#references)