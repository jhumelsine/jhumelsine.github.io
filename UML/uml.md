# What is UML?
**NOTE: Work In Progress**

[Unified Modeling Language](https://en.wikipedia.org/wiki/Unified_Modeling_Language) (UML) began when [Ivar Jacobsen](https://en.wikipedia.org/wiki/Ivar_Jacobson), [James Rumbaugh](https://en.wikipedia.org/wiki/James_Rumbaugh) and [Grady Booch](https://en.wikipedia.org/wiki/Grady_Booch), collectively known as "The Three Amigos," joined forces in the mid 1990s to unify their individual modeling approaches into one.

UML provides a means of modeling different aspects of the software development process, most closely associated with the Object Oriented Programming paradigm. UML specifies 17 types of [diagrams](https://en.wikipedia.org/wiki/Unified_Modeling_Language#Diagrams). It's intended to be informal enough so that diagrams can be easily drawn by hand on white boards and formal enough so UML specific software, such as modeling tools to draw diagrams, generating code from those diagrams, and generating diagrams from the code.

UML is a specification managed by the [Object Modeling Group](www.omg.org) (OMG).

# Its demise

UML flamed hot, then cooled. It's rarely taught in universities. It's not used often in industry. What happened?

It became too popular and bloated with features. The [UML 2.5.1 specification](https://www.omg.org/spec/UML/2.5.1/PDF) from late 2017 is almost 800 pages long. Some of the automated tools where not great. Too many software processes required its use too rigidly. Many of us were forced to create too many unnecessary UML documents with tools that could be too cumbersome to use. It left a bad  impression for many.

I was fortunate enough to have been exposed to UML before it was rammed down our throats. A little can go a long way. Of the 17 diagrams, I use one about 95% of the time. I use a few others, but only sparingly. I also don't use all of the notations. The specification may be 800 pages long, but I could probably document the parts I use in about 4 pages.

UML is a tool. It should work for us. We should not work for it. Use as much or as little of it to suit your needs.

# When I realized how to use them

My first experience was with one of UML's predicessors, the [Object Modeling Technique](https://en.wikipedia.org/wiki/Object-modeling_technique) (OMT), defined by Rumbaugh. It most closely resembles UML's Class Diagrams. The diagrams in the [Gang of Four's Design Pattern](https://learning.oreilly.com/library/view/design-patterns-elements/0201633612/) book use OMT.

Here's a sample diagram, just to provide a sense of what OMT looked like (Source: [CMU's Medevac Domain Object Model](https://www.cs.cmu.edu/afs/cs/project/ozone/www/DITOPS/publications/ditops-trac2es/node10.html)):

![Medevac Model OMT Diagram](https://www.cs.cmu.edu/afs/cs/project/ozone/www/DITOPS/publications/ditops-trac2es/_12156_figure524.gif) 

I had taken a rotation assignment as a system tester, what we'd now call QA, in the mid 1990s. I was assigned about 20 requirements to test. I could understand them individually, but I was having a problem understanding them as a cohesive unit. The result of one requirement seemed an event to trigger for another requirement. One event seemed to change the nature and the state of the system as a whole as its impact propagated.

I brought a copy of the requirements home. I cut my requirements into separate pieces of paper and spread them across the living room floor. I clustered requirement that seemed to relate to another another, but I still wasn't seeing the entire picture.

Then one Sunday morning in church, I was getting a little bored during the sermon and my mind started to wonder; sorry, Pastor Nichols. I was thinking about the requirements, and I thought somethink like: **X IS-A Y**, and then **X HAS-A Z**. These were basic relationships defined in the requirements. I started to envision my requirements as a diagram where the requirements formed relationship lines among the entities defined in the requirement.

I grabbed my set of requirements, a blank sheet of paper and a pen. As I read each requirement, I drew the relationship it described on the paper in a diagram similar to the one above. Each requirement was a new line connecting elements in the system.

When I was done, the entire set of 20 requirements fit on one sheet of paper. It was a roadmap. I could see its cohesiveness. I could follow the propagation flow of one trigger as it propagated through the system. It all made sense.

UML is often used as a development tool. That may not be the right focus. I was not a developer for that assignment. I was not using OMT (UML's predecessor) to implement a solution. I was using OMT to understand the problem.

**The value of UML is not to define the solution. The value of UML is helping understand the domain of the problem.** This work often leads to an implementation, but I don't think that's it's main value.

# My Favorite Diagrams

## Class Diagrams

## Use Case Diagrams

## State Diagrams

## Sequence Diagrams

# Ideas

My personal design process.

If a design diagram is messy, such as lots of crisscrossing lines, then the implementation will be messy. Fix the design.

Circular dependencies?

Stable/Unstable elements?

Law of Demeter.

Use interfaces or classes as connectors.

