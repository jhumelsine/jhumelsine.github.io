---
title: Design Process
description: My design process from concept to implementation
unlisted: true
---
<img src="/assets/ConstructionProcess.jpeg" alt="Constructor Workers Reviewing a Design"  width = "50%" align="center" style="padding-right: 35px;">

# Introduction
I am taking a break from specific Design Patterns for a while to focus upon other Software Engineering topics. I’ll start with my design process, which will still reference design patterns, but I won’t focus upon them.

I have provided glimpses of my process in previous blog posts. I’ll be more intentional here. This process worked best for me when scoped to a functional component, such as a [bounded context](https://martinfowler.com/bliki/BoundedContext.html). I wouldn’t use this process for an entire architecture, nor would I use it for an individual class.

This is my personal process, which won’t apply to everyone in all situations. Developers must find a process that works for them. Hopefully, some readers may find some elements of my process helpful for their own processes.

__Caveat:__ I developed this process before I knew about [Test-Driven Development](https://en.wikipedia.org/wiki/Test-driven_development) (TDD). I have since incorporated elements of TDD into my process, but had I known TDD before I developed this process, I’m sure my process would have been different.

# My Process

## Where To Start
I posted several quotes a few months ago in [Bumper Sticker Computer Science and Software Engineering](https://jhumelsine.github.io/2023/12/15/bumper-sticker-computer-science-software-engineering.html#planning). My favorite planning quotes are:
* _The sooner you start to code, the longer the program will take._ — Roy Carlson
* _Weeks of coding can save you hours of planning._ — Unknown

I don’t jump into the code immediately. I first need to have a sense of the desired behavior. This usually comes from requirements, use cases, user stories, acceptance criteria, etc. Even the [Package Routing PowerPoint Slide Requirements](https://jhumelsine.github.io/2024/05/14/interpreter-design-pattern-production.html#the-requirements) provided me with a starting point.

If desired behavior isn’t described, then I’m probably wasting my time working on a design or implementation.

## Visual Thinker
<img src="https://upload.wikimedia.org/wikipedia/commons/e/e3/Allgemeiner-baum.png" alt="Data Structure" title="Image Source: https://upload.wikimedia.org/wikipedia/commons/e/e3/Allgemeiner-baum.png" width = "25%" align="right" style="padding-right: 35px;">

I’m a visual thinker. I didn’t understand data structures in college until our professor drew them on the whiteboard using rectangles and lines representing pointers.

[UML Class Diagrams](https://jhumelsine.github.io/2021/06/22/uml.html#class-diagrams) is how I visualize object-oriented (OO) code. But it’s more than that. It’s how I visualize the domain model. UML Class Diagrams do a great job of representing __IS-A__ and __HAS-A__ relationships between domain concepts. This occurred to me [When I realized how to use them]( https://jhumelsine.github.io/2021/06/22/uml.html#when-i-realized-how-to-use-them) to model behavior from requirements without concern of an implementation. I was using [Object-Modeling Technique](https://en.wikipedia.org/wiki/Object-modeling_technique) (OMT) at the time, the predecessor to [Unified Modeling Language](https://en.wikipedia.org/wiki/Unified_Modeling_Language) (UML). The concepts are consistent in both with their main differences being notation.

I start my process with UML Class Diagrams to understand the domain model, its domain elements, and their relationships. The design transform almost seamlessly into a design of OO classes.

## The Process Should Work for You, Not You for the Process
I worked on a massive waterfall-based project many years ago where we were required to create complete designs including:
* [Class Diagrams](https://en.wikipedia.org/wiki/Class_diagram)
* [Use Case Diagrams](https://en.wikipedia.org/wiki/Use_case_diagram)
* [Sequence Diagrams](https://en.wikipedia.org/wiki/Sequence_diagram)
* [State Machine Diagrams](https://en.wikipedia.org/wiki/UML_state_machine), if apropos
* Etc.

It was a massive waste of resources. We had to provide so much detail that we were practically _coding_ the entire implementation in the UML documentation tool without the benefit of being able to test the code, knowing external APIs, etc. Then once we started the implementation, we’d find flaws in our design. We would correct them in the code, but we never updated our design documents. No one ever read them anyhow.

The UML documentation tool forced upon us was a bit buggy. My sequence diagrams would sporadically go haywire, rendering the diagram useless. There was not way to undo the damage. My only option was to terminate the tool, then start it using the most recently saved version. All subsequent work would be lost. I got into the habit of saving after every step so that lost work would be kept to a minimum on a restart.

Its only benefit I remember was its completeness and consistency. If you added a class, method, attribute, etc. in one diagram, then it would be accessible in other diagrams that referenced it. Sometimes this accessibility was useful. Sometimes it cluttered the design with more information than needed.

While I like visual thinking via UML, this level of detail was overkill. We were working for the process. The process was not working for us. 

## Pen to Paper
I use just enough UML detail to keep my thoughts organized. I tend to use UML Class Diagrams for the vast majority of my design, but sometimes I may also create Use Case Diagrams, State Machine Diagrams and Sequence/Communication Diagrams as needed.

Each design requires a different level of detail. Each designer will use their own level of detail.

I often start with pen and paper to jot down domain elements and their __IS-A__/__HAS-A__ relationships. I tend to create several hand drawn diagrams. Quite often I’ll spot issues in the current diagram before I’ve even finished it. I’ll complete the diagram and then I’ll grab a blank sheet of paper and start a fresh one making any necessary adjustments.

Each iteration gives me time to think. I’ll add more detail in each iteration. I’ll add contract methods to interfaces. I may add implementation ideas and even code snippets for a few classes.

I usually don’t include administrative details, such as constructors or accessor methods. I usually don’t include attributes, since UML relationship lines tend to indicate the important ones. I usually don’t include private methods, mostly because I consider them implementation details more than design details.

If the design becomes cluttered, especially with many cross-crossing lines, then this is an indication that the implementation will be cluttered as well.
I rethink cluttered designs and try to create a less cluttered one.

Eventually a design emerges for the domain elements often structured around design patterns concepts.

## PowerPoint
Once I’m reasonably satisfied with a design on pen and paper, I’ll recreate it in PowerPoint. I’ve used PowerPoint for all the UML Class Diagrams that I’ve presented previously in my blogs.

There are many UML tools that will assist. Developers are free to choose whatever drawing mechanism works best for them.

Moving to a drawing tool cleans up the rough edges. It gives me more opportunities to think about the design. When I need to make changes, I don’t need to redraw the entire diagram from scratch. I can often move the existing elements around in the diagram as needed with minimal clean up.

## One to Many
When a hand drawn design won’t easily fit on a single slide (or page) in PowerPoint, I’ll split the design across pages. Sometimes I split the design while I’m still using pen and paper.

I’ll use a class, often an interface or an abstract class, as the connecting elements among pages. The connecting element will appear on multiple pages. 

Most interface and class elements have two aspects in a design:
* Classes that access their public methods. I.e., highlighting __WHAT__ they do.
* Classes that implement or extend them. I.e., highlighing __HOW__ they do it.

The page connecting classes tend to be on the edges of a design when modeling __what__ they provide. They are supporting elements of that page.

The page connecting classes tend to be at the top center of a design when modeling __how__ they are implemented. They are primary elements of that page.

## Implementation
Once I’m reasonably satisfied with a design in PowerPoint, I’ll implement the elements in the design.

I tend to follow TDD practices with its Red-Green-Refactor cycle. I also try to adhere to Bob Martin’s [Three Rules of TDD](http://butunclebob.com/ArticleS.UncleBob.TheThreeRulesOfTdd).
I only write enough test and code to confirm behavior as it's being incrementially implemented.
I refactor in each cycle as needed.

The implementation tends to go quickly and usually without too many issues.

Since the design tends to already have code snippets, I’ve technically gotten a little ahead of myself with respect to TDD practices. I rationalize this with the second _D_ in _TDD_ standing for _Development_ whereas I have been focusing upon _Design_ up to this point. __LOL__.

Even though some code snippets already exist in the design, I still find TDD practices useful, since:
* The tests declare intent and ensure that the code always adheres to that intent.
* The tests are the first clients to use the API via the public methods. I’m the first person to encounter anything that’s janky with the API.
* The tests are the first user documentation, and it’s guaranteed to work.
* The tests reveal any previously unforeseen issues in the design. Design is theory. Implementation is the confirmation of that theory.

# Why this process works for me
Here are some benefits I get from hand drawn and PowerPoint rendered UML Class Diagrams for my designs.

## Time to Think
It slows me down. This friction gives me time to think about my design, consider alternatives, etc. It’s easy to make changes, try different designs and run scenarios through my head before writing any code.

I don’t feel that developers are given enough time or take enough time to think about their designs.

## Complete Control
I have complete control. I can show as much or as little detail as I desire. I can use different colors and fonts in PowerPoint easily.

I’m not shackled by a process or tool that forces me to provide details that I don’t really need or doesn’t have the ability to present details that I do need.

## Size Matters
There’s a limit to how much I can fit on a page. This constraint helps to prevent the design from growing too large and complicated. The constrained design usually adheres to the [Law of Demeter](https://en.wikipedia.org/wiki/Law_of_Demeter).

When the design is too large for a single page, segregating the design across multiple pages using interfaces and abstract classes as connecting elements provides a nice mechanism to expand the design while retaining modularity.

## Natural Boundaries
Segregating a design across several pages with interfaces and abstract classes as the connecting elements is more than just a convenient decomposition mechanism. Interfaces and abstract classes are [stable/fixed](https://jhumelsine.github.io/2023/11/03/hexagonal-architecture-dependencies-knowledge.html#stable-or-fixed-design-elements) elements. They don’t depend upon other parts of the design. Dependency/knowledge arrows point toward them. 

The design elements on one page don’t depend upon nor know about the design elements on other pages. The stable/fixed connecting elements don’t allow information to flow through them. There are exceptions to this, especially with [Leaky Abstractions](https://en.wikipedia.org/wiki/Leaky_abstraction), but leaks should be kept to a minimum.

Design elements on the same page tend to be cohesive without being coupled to design elements on other pages. If the stable/fixed connecting elements are reasonablely mature, implementation can proceed with the design elements across the pages in parallel.

Stable/fixed connecting elements become natural boundaries for the design and implementation. If a design results in three separate pages, then three different developers/teams should be able to implement them in parallel without depending upon or knowing each other’s details.

# Example
I’m working on the [Advent of Code](https://adventofcode.com/) backlog between blog posts. The [Advent of Code 2015 Day 7 Challenge](https://adventofcode.com/2015/day/7) is based upon [Logic Gates](https://en.wikipedia.org/wiki/Logic_gate). The assignment defines 16-bit logic gates for AND, OR, LSHIFT, RSHIFT and NOT operations. The assignment data defines a layout of over 300 logic gates. The challenge is to calculate the output value of gate `a`.

__Spoiler Alert!__: I’m going to present my solution. After almost nine years to statute of limitations for revealing a solution has passed. This is your last chance to solve it yourself before seeing my solution.

## Pen and Paper
Here’s my pen and paper design:

<img src="/assets/HandDrawnDesign.jpg" alt="Hand Drawn Design"  width = "80%" align="center" style="padding-right: 35px;">

I think it was the third or fourth iteration. It’s messy, but since it’s only for my use, that’s okay. It was enough for me to organize my initial thoughts.

It’s similar to the [Rational Expression Evaluator](https://jhumelsine.github.io/2024/04/30/interpreter-design-pattern-parser-implementation.html) design. The [Composite Design Pattern](https://jhumelsine.github.io/2024/02/27/composite-design-pattern.html) is the core design element. The `Parser` reads the assignment data and creates the objects that correspond to it.

There is a wrinkle in the logic gate assignment data. The logic gate layout elements are not presented in any order. Each gate and its input and output _wires_ labels are presented in any order in the layout specification. I can’t easily build a traditional composite object tree, since I don’t have an ordered layout, and I don’t feel like implementing a [topological sort](https://en.wikipedia.org/wiki/Topological_sorting).

Instead, I decided to store each gate in a `Map` with its output wire as the key. `Composite` will look up the gate in the `Map` based upon the wire name and get its signal value. This worked fine with the test data, but I had a performance issue with the assignment data, which I’ll address later.

## PointPoint
This design won’t fit on one PowerPoint slide. I split it into two slides with the abstract `Composite` as the connecting element.
`Composite` isn’t a traditional Composite by reference. It doesn’t delegate to `Expression` directly. It doesn’t implement `getSignal()`. The `Composite` to `Expression` reference in the hand drawn design has been removed in the PowerPoint design once I realized that it was not 100% correct. 

__NOTE:__ In hindsight, I should have used `Gate` rather than `Expression` and `getOutputWire()` rather than `getSignal()`. And `Composite`’s protected attributes should have probably been named `inputWireA` and `inputWireB`. And `Composite` should have probably been named `GateOperation`. If the assignment had more longevity than the typical Advent of Code assignment, I would have considered using more domain relevant names.

`Composite` is a Composite by name. It acquires an `Expression` from `Map<String, Expression>` by name and delegates to it.

<img src="/assets/LogicGates1.png" alt="Logic Gates Design 1"  width = "80%" align="center" style="padding-right: 35px;">

The second diagram details the different Logic Gates each of which supports a logic operation.

<img src="/assets/LogicGates2.png" alt="Logic Gates Design 2"  width = "80%" align="center" style="padding-right: 35px;">

I realized that I could use Java’s shift operations for LShift and RShift rather than multiplying or dividing by a power of 2 as shown in the hand drawn diagram, so I made the update in the PowerPoint.
 
## Implementation
The implementation was straight forward, but I ran into two issues not anticipated in the design.

`Not` was not returning correct results in the test case examples. The assignment is for 16-bit logic gates. My implementation is using Java `int`, which is 32 bits. Flipping the 16 high order bits in the `Not` implementation was giving me incorrect results. I addressed it by masking the 16 high order bits to zeros.

My unit tests worked, once I fixed `Not`. I tried the assignment dataset, and my program didn’t terminate. The problem was in `Composite`. It was evaluating `Expression` in the `Map` regardless of how many times it had previously been evaluated. I surmised that the same set of gets were being evaluated repeatedly. I added [memoization](https://en.wikipedia.org/wiki/Memoization) with a few new lines on `Composite` to remember previously evaluated `Expression` values, and the performance issues disappeared.

See the complete implementation in the References section.

# Summary
My process gives me time to think. It only contains the level of detail that I need to organize my thoughts. The implementation tends to emerge from the design and work.

This process has always worked well for me. It might work for you or not. Feel free to adopt or ignore any parts of what I’ve presented.

# References
My Logic Gate implementation:
```java
import java.util.*;
import java.io.BufferedReader;
import java.io.FileReader;
import java.io.IOException;

public class CircuitDiagram {
    public static void main(String []args) {
        Test.test();


        Map<String, Expression> map = new HashMap<>();
        Parser parser = new Parser(map);
        for (String instruction : Reader.getData("data.txt")) 
            parser.parse(instruction);
        System.out.println(map.get("a").getSignal());
    }
}

class Parser {
    private final Map<String, Expression> map;

    public Parser(Map map) {
        this.map = map;
    }

    public final void parse(String instruction) {
        String[] tokens = instruction.split("->");
        map.put(tokens[1].trim(), getExpression(tokens[0]));
    }

    private Expression getExpression(String instruction) {
        instruction = instruction.trim();

        String[] tokens = instruction.split(" ");
        if (instruction.contains("NOT")) return new Not(map, tokens[1]);
        if (instruction.contains("AND")) return new And(map, tokens[0], tokens[2]);
        if (instruction.contains("OR")) return new Or(map, tokens[0], tokens[2]);
        if (instruction.contains("LSHIFT")) return new LShift(map, tokens[0], Integer.parseInt(tokens[2].trim()));
        if (instruction.contains("RSHIFT")) return new RShift(map, tokens[0], Integer.parseInt(tokens[2].trim()));

        try {
            return new Constant(Integer.parseInt(instruction));
        } catch (NumberFormatException e) {
            return new Variable(map, instruction);
        }
    }
}

interface Expression {
    int getSignal();
}

class Constant implements Expression {
    private final int value;

    public Constant(int value) {
        this.value = value;
    }

    @Override
    public final int getSignal() {
        return value;
    }
}

abstract class Composite implements Expression {
    private final Map<String, Expression> map;
    private final Map<String, Integer> calculatedMap = new HashMap<>();
    protected final String expressionA;
    protected final String expressionB;

    public Composite(Map<String, Expression> map, String expressionA, String expressionB) {
        this.map = map;
        this.expressionA = expressionA;
        this.expressionB = expressionB;
        addConstant(expressionA);
        addConstant(expressionB);
    }

    private void addConstant(String value) {
        try {
            if (!map.containsKey(value)) map.put(value, new Constant(Integer.parseInt(value)));
        } catch (NumberFormatException e) {}
    }

    protected final int getSignal(String name) {
        if (calculatedMap.containsKey(name)) return calculatedMap.get(name);
        int calculatedValue = map.get(name).getSignal();
        calculatedMap.put(name, calculatedValue);
        return calculatedValue;
    }
}

class Variable extends Composite {
    public Variable(Map<String, Expression> map, String expressionA) {
        super(map, expressionA, null);
    }

    @Override
    public final int getSignal() {
        return getSignal(expressionA);
    }
}

class Not extends Composite {
    public Not(Map<String, Expression> map, String expressionA) {
        super(map, expressionA, null);
    }

    @Override
    public final int getSignal() {
        return 0x0000FFFF & ~getSignal(expressionA);
    }
}

class And extends Composite {
    public And(Map<String, Expression> map, String expressionA, String expressionB) {
        super(map, expressionA, expressionB);
    }

    @Override
    public final int getSignal() {
        return getSignal(expressionA) & getSignal(expressionB);
    }
}

class Or extends Composite {
    public Or(Map<String, Expression> map, String expressionA, String expressionB) {
        super(map, expressionA, expressionB);
    }

    @Override
    public final int getSignal() {
        return getSignal(expressionA) | getSignal(expressionB);
    }
}

abstract class Shift extends Composite {
    protected final int shift;

    public Shift(Map<String, Expression> map, String expressionA, int shift) {
        super(map, expressionA, null);
        this.shift = shift;
    }
}

class LShift extends Shift {
    public LShift(Map<String, Expression> map, String expressionA, int shift) {
        super(map, expressionA, shift);
    }
    
    @Override
    public final int getSignal() {
        return getSignal(expressionA) << shift;
    }
}

class RShift extends Shift {
    public RShift(Map<String, Expression> map, String expressionA, int shift) {
        super(map, expressionA, shift);
    }

    @Override
    public final int getSignal() {
        return getSignal(expressionA) >>> shift;
    }
}

class Reader {
    public static List<String> getData(String fileName) {
        List<String> data = new LinkedList<>();
        BufferedReader reader;
        try {
            reader = new BufferedReader(new FileReader(fileName));
            String line = reader.readLine();
            while (line != null) {
                data.add(line);
                line = reader.readLine();
            }
            reader.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
        return data;
    }
}

class Test {
    public static void test() {
        constantReturnsItsValue();

        variableReturnsItsValue();

        testParser();

        System.out.println("END OF TESTS");
    }

    private static void constantReturnsItsValue() {
        Constant constant = new Constant(123);
        assertEquals(123, constant.getSignal());
    }

    private static void variableReturnsItsValue() {
        Map<String, Expression> map = new HashMap<>();
        map.put("123", new Constant(123));

        Variable variable = new Variable(map, "123");
        
        assertEquals(123, variable.getSignal());
    }

    private static void testParser() {
        Map<String, Expression> map = new HashMap<>();
        Parser parser = new Parser(map);

        parser.parse("123 -> x");
        parser.parse("456 -> y");
        parser.parse("y -> z");
        parser.parse("x AND y -> d");
        parser.parse("x OR y -> e");
        parser.parse("x LSHIFT 2 -> f");
        parser.parse("y RSHIFT 2 -> g");
        parser.parse("NOT x -> h");
        parser.parse("NOT y -> i");
        parser.parse("1 OR 2 -> j");
        parser.parse("1 AND 3 -> k");

        assertEquals(123, map.get("x").getSignal());
        assertEquals(456, map.get("y").getSignal());
        assertEquals(456, map.get("z").getSignal());
        assertEquals(72, map.get("d").getSignal());
        assertEquals(507, map.get("e").getSignal());
        assertEquals(492, map.get("f").getSignal());
        assertEquals(114, map.get("g").getSignal());
        assertEquals(65412, map.get("h").getSignal());
        assertEquals(65079, map.get("i").getSignal());
        assertEquals(3, map.get("j").getSignal());
        assertEquals(1, map.get("k").getSignal());
    }

    private static void assertEquals(int expected, int actual) {
        if (expected != actual) {
            System.out.format("expected=%d, actual=%d\n", expected, actual);
        }
    }
}
```

Here is the circuit pack layout assignment defined by Advent of Code:
```
af AND ah -> ai
NOT lk -> ll
hz RSHIFT 1 -> is
NOT go -> gp
du OR dt -> dv
x RSHIFT 5 -> aa
at OR az -> ba
eo LSHIFT 15 -> es
ci OR ct -> cu
b RSHIFT 5 -> f
fm OR fn -> fo
NOT ag -> ah
v OR w -> x
g AND i -> j
an LSHIFT 15 -> ar
1 AND cx -> cy
jq AND jw -> jy
iu RSHIFT 5 -> ix
gl AND gm -> go
NOT bw -> bx
jp RSHIFT 3 -> jr
hg AND hh -> hj
bv AND bx -> by
er OR es -> et
kl OR kr -> ks
et RSHIFT 1 -> fm
e AND f -> h
u LSHIFT 1 -> ao
he RSHIFT 1 -> hx
eg AND ei -> ej
bo AND bu -> bw
dz OR ef -> eg
dy RSHIFT 3 -> ea
gl OR gm -> gn
da LSHIFT 1 -> du
au OR av -> aw
gj OR gu -> gv
eu OR fa -> fb
lg OR lm -> ln
e OR f -> g
NOT dm -> dn
NOT l -> m
aq OR ar -> as
gj RSHIFT 5 -> gm
hm AND ho -> hp
ge LSHIFT 15 -> gi
jp RSHIFT 1 -> ki
hg OR hh -> hi
lc LSHIFT 1 -> lw
km OR kn -> ko
eq LSHIFT 1 -> fk
1 AND am -> an
gj RSHIFT 1 -> hc
aj AND al -> am
gj AND gu -> gw
ko AND kq -> kr
ha OR gz -> hb
bn OR by -> bz
iv OR jb -> jc
NOT ac -> ad
bo OR bu -> bv
d AND j -> l
bk LSHIFT 1 -> ce
de OR dk -> dl
dd RSHIFT 1 -> dw
hz AND ik -> im
NOT jd -> je
fo RSHIFT 2 -> fp
hb LSHIFT 1 -> hv
lf RSHIFT 2 -> lg
gj RSHIFT 3 -> gl
ki OR kj -> kk
NOT ak -> al
ld OR le -> lf
ci RSHIFT 3 -> ck
1 AND cc -> cd
NOT kx -> ky
fp OR fv -> fw
ev AND ew -> ey
dt LSHIFT 15 -> dx
NOT ax -> ay
bp AND bq -> bs
NOT ii -> ij
ci AND ct -> cv
iq OR ip -> ir
x RSHIFT 2 -> y
fq OR fr -> fs
bn RSHIFT 5 -> bq
0 -> c
14146 -> b
d OR j -> k
z OR aa -> ab
gf OR ge -> gg
df OR dg -> dh
NOT hj -> hk
NOT di -> dj
fj LSHIFT 15 -> fn
lf RSHIFT 1 -> ly
b AND n -> p
jq OR jw -> jx
gn AND gp -> gq
x RSHIFT 1 -> aq
ex AND ez -> fa
NOT fc -> fd
bj OR bi -> bk
as RSHIFT 5 -> av
hu LSHIFT 15 -> hy
NOT gs -> gt
fs AND fu -> fv
dh AND dj -> dk
bz AND cb -> cc
dy RSHIFT 1 -> er
hc OR hd -> he
fo OR fz -> ga
t OR s -> u
b RSHIFT 2 -> d
NOT jy -> jz
hz RSHIFT 2 -> ia
kk AND kv -> kx
ga AND gc -> gd
fl LSHIFT 1 -> gf
bn AND by -> ca
NOT hr -> hs
NOT bs -> bt
lf RSHIFT 3 -> lh
au AND av -> ax
1 AND gd -> ge
jr OR js -> jt
fw AND fy -> fz
NOT iz -> ja
c LSHIFT 1 -> t
dy RSHIFT 5 -> eb
bp OR bq -> br
NOT h -> i
1 AND ds -> dt
ab AND ad -> ae
ap LSHIFT 1 -> bj
br AND bt -> bu
NOT ca -> cb
NOT el -> em
s LSHIFT 15 -> w
gk OR gq -> gr
ff AND fh -> fi
kf LSHIFT 15 -> kj
fp AND fv -> fx
lh OR li -> lj
bn RSHIFT 3 -> bp
jp OR ka -> kb
lw OR lv -> lx
iy AND ja -> jb
dy OR ej -> ek
1 AND bh -> bi
NOT kt -> ku
ao OR an -> ap
ia AND ig -> ii
NOT ey -> ez
bn RSHIFT 1 -> cg
fk OR fj -> fl
ce OR cd -> cf
eu AND fa -> fc
kg OR kf -> kh
jr AND js -> ju
iu RSHIFT 3 -> iw
df AND dg -> di
dl AND dn -> do
la LSHIFT 15 -> le
fo RSHIFT 1 -> gh
NOT gw -> gx
NOT gb -> gc
ir LSHIFT 1 -> jl
x AND ai -> ak
he RSHIFT 5 -> hh
1 AND lu -> lv
NOT ft -> fu
gh OR gi -> gj
lf RSHIFT 5 -> li
x RSHIFT 3 -> z
b RSHIFT 3 -> e
he RSHIFT 2 -> hf
NOT fx -> fy
jt AND jv -> jw
hx OR hy -> hz
jp AND ka -> kc
fb AND fd -> fe
hz OR ik -> il
ci RSHIFT 1 -> db
fo AND fz -> gb
fq AND fr -> ft
gj RSHIFT 2 -> gk
cg OR ch -> ci
cd LSHIFT 15 -> ch
jm LSHIFT 1 -> kg
ih AND ij -> ik
fo RSHIFT 3 -> fq
fo RSHIFT 5 -> fr
1 AND fi -> fj
1 AND kz -> la
iu AND jf -> jh
cq AND cs -> ct
dv LSHIFT 1 -> ep
hf OR hl -> hm
km AND kn -> kp
de AND dk -> dm
dd RSHIFT 5 -> dg
NOT lo -> lp
NOT ju -> jv
NOT fg -> fh
cm AND co -> cp
ea AND eb -> ed
dd RSHIFT 3 -> df
gr AND gt -> gu
ep OR eo -> eq
cj AND cp -> cr
lf OR lq -> lr
gg LSHIFT 1 -> ha
et RSHIFT 2 -> eu
NOT jh -> ji
ek AND em -> en
jk LSHIFT 15 -> jo
ia OR ig -> ih
gv AND gx -> gy
et AND fe -> fg
lh AND li -> lk
1 AND io -> ip
kb AND kd -> ke
kk RSHIFT 5 -> kn
id AND if -> ig
NOT ls -> lt
dw OR dx -> dy
dd AND do -> dq
lf AND lq -> ls
NOT kc -> kd
dy AND ej -> el
1 AND ke -> kf
et OR fe -> ff
hz RSHIFT 5 -> ic
dd OR do -> dp
cj OR cp -> cq
NOT dq -> dr
kk RSHIFT 1 -> ld
jg AND ji -> jj
he OR hp -> hq
hi AND hk -> hl
dp AND dr -> ds
dz AND ef -> eh
hz RSHIFT 3 -> ib
db OR dc -> dd
hw LSHIFT 1 -> iq
he AND hp -> hr
NOT cr -> cs
lg AND lm -> lo
hv OR hu -> hw
il AND in -> io
NOT eh -> ei
gz LSHIFT 15 -> hd
gk AND gq -> gs
1 AND en -> eo
NOT kp -> kq
et RSHIFT 5 -> ew
lj AND ll -> lm
he RSHIFT 3 -> hg
et RSHIFT 3 -> ev
as AND bd -> bf
cu AND cw -> cx
jx AND jz -> ka
b OR n -> o
be AND bg -> bh
1 AND ht -> hu
1 AND gy -> gz
NOT hn -> ho
ck OR cl -> cm
ec AND ee -> ef
lv LSHIFT 15 -> lz
ks AND ku -> kv
NOT ie -> if
hf AND hl -> hn
1 AND r -> s
ib AND ic -> ie
hq AND hs -> ht
y AND ae -> ag
NOT ed -> ee
bi LSHIFT 15 -> bm
dy RSHIFT 2 -> dz
ci RSHIFT 2 -> cj
NOT bf -> bg
NOT im -> in
ev OR ew -> ex
ib OR ic -> id
bn RSHIFT 2 -> bo
dd RSHIFT 2 -> de
bl OR bm -> bn
as RSHIFT 1 -> bl
ea OR eb -> ec
ln AND lp -> lq
kk RSHIFT 3 -> km
is OR it -> iu
iu RSHIFT 2 -> iv
as OR bd -> be
ip LSHIFT 15 -> it
iw OR ix -> iy
kk RSHIFT 2 -> kl
NOT bb -> bc
ci RSHIFT 5 -> cl
ly OR lz -> ma
z AND aa -> ac
iu RSHIFT 1 -> jn
cy LSHIFT 15 -> dc
cf LSHIFT 1 -> cz
as RSHIFT 3 -> au
cz OR cy -> da
kw AND ky -> kz
lx -> a
iw AND ix -> iz
lr AND lt -> lu
jp RSHIFT 5 -> js
aw AND ay -> az
jc AND je -> jf
lb OR la -> lc
NOT cn -> co
kh LSHIFT 1 -> lb
1 AND jj -> jk
y OR ae -> af
ck AND cl -> cn
kk OR kv -> kw
NOT cv -> cw
kl AND kr -> kt
iu OR jf -> jg
at AND az -> bb
jp RSHIFT 2 -> jq
iv AND jb -> jd
jn OR jo -> jp
x OR ai -> aj
ba AND bc -> bd
jl OR jk -> jm
b RSHIFT 1 -> v
o AND q -> r
NOT p -> q
k AND m -> n
as RSHIFT 2 -> at
```
