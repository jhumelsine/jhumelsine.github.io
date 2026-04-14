---
title: WORK IN PROGRESS – Communication Patterns
description: TBD
unlisted: true
---

<img src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExb2ZzcWdvbXQ1ajM0dWFjY2xiZjN4dHBjcDdjZG45NW13bTFlaG4wNyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3oEjHLFk88eqsmJ5F6/giphy.gif" alt="Cool Hand Luke Failure To Communicate" title="Image Source: https://giphy.com/gifs/3oEjHLFk88eqsmJ5F6" width = "60%" align="center" style="padding-right: 20px;">

# Introduction
About three decades ago, I worked on a project and someone asked me, "Can you call my function when this thing happens in your code?"

"Sure, I can do that," I replied. So I made the change.

A few days later, he stopped by and said, "Turns out, I don't need you to call me function. You can take it out." So I removed the call to his function.

A bit later, he returned and sheepishly said, "I know I asked you to remove that call to my function, but it turns out, I do need it. Can you add it back in?" Geesh, make up your mind, but I added it, since he needed it.

A few years later, I had moved to another company. This time I was the requester. I wanted to know when something happened in another developer's code. I made the same request, and his response was, "No. I can't do that. You should have asked for that during the design review." This happened a year or two after the [Agile Manifesto](https://agilemanifesto.org/) had been published. We were still deep into the [Waterfall Model](https://en.wikipedia.org/wiki/Waterfall_model). If I had suggested that we should be ___Responding to change___ _over following a plan_, I'm sure I would have been laughed out of the room

I kindly reminded him that that the design review happened about three months before I had joined the company. Then thinking about the previous job, I thought to myself, "Hey! I could have used the same excuse to deny the notification requests on my previous job!?"

One of the __Communication Design Patterns__ would have solve my communication issues smoothly regardless of whether I was providing information or requesting it.

# Communication Design Patterns
Unlike the Captain in [___Cool Hand Luke___](https://en.wikipedia.org/wiki/Cool_Hand_Luke), who blames the inmates for their inability to follow his rigid authority as communication failure, the Communication Design Patterns foster communication, but in a decoupled way. Coupling was the problem I encountered decades ago as described above. Had I known these patterns, I could have saved myself and others a lot of headaches.

I only classify two Gang of Four (GoF) design patterns as communication patterns:
* __Observer__ (TBD), which allows any number of interested parties to receive update notifications
* __Mediator__ (TBD), which describes a communication network of Domain Events (TBD)

One would think that there would be more than two communication patterns. One can also argue that many if not all of the GoF's design patterns are about communication. However, up to this point, that communication has been a side-effect of delegation and propagation. Services define a [contract](https://jhumelsine.github.io/2025/06/10/contracts.html) and client code calls it. The service is passive. In the communication patterns, the service takes on a more active role.

I will briefly introduce the two Communication Design Patterns here and provide details in subsequent blog entries.

## Observer
<img src="https://media4.giphy.com/media/v1.Y2lkPTc5MGI3NjExdGt5bnBrbWw3N20xamtsOXFocmozbWo4NHhkcDZhenMzNGJobDc3aCZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/4vIfVmbQonId4YTpjI/giphy.gif" alt="Observing with Binoculars on a Stakeout" title="Image Source: https://giphy.com/gifs/potterythrow-spy-spying-people-watching-4vIfVmbQonId4YTpjI" width = "30%" align="right" style="padding-right: 20px;">

__Observer__ would have solve both problems I described in the [Introduction](#introduction). In Observer, any number of interested parties will receive notifications when the information they're interested has been updated. It requires a minor enhancment made by the developer whose code is reponsible for that information, and then that developer is out of the loop.

The interested parties can subscribe to and unsubscribe from update notifications as needed without an impact upon the code that manages the updates. The may only be several subscribers, thousands of them or zero. It's of no consequence to the managing code.

The Observer is like a private investigator on a stakeout.

There are several variations to consider when implementing Observer, and I'll illustrate them in subsequent blog entries.

This pattern is also called [___Pub/Sub___](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) because the information manager publishes notifications to subscribers.

Twitter/X is an example of this communication paradigm. People subscribe to the posts of a specific account. When that account's owner publishes a tweet, it will appear in the feed of all subscribers.

## Mediator
<img src="https://upload.wikimedia.org/wikipedia/commons/2/23/Teletype_Model_15_printing_news_report.jpg?20121221042756" alt="News Service Teletype" title="Image Source: https://commons.wikimedia.org/wiki/File:Teletype_Model_15_printing_news_report.jpg" width = "30%" align="right" style="padding-right: 20px;">

Consider how people get their news. Some subscribe to their favorite influencers receiving only what that person posts. Others may subscribe to a news service, where their feed could include stories written by any reporter within the news service.

Observer is laser focused upon a subscribed source and any updates to that source. Mediator is interested in events in general in the system. An event is an immutable fact that occurred in the past. Returning to the news service, it would contain a story that specifies _what_ happened, _when_ it happened, _where_ it happened and possibly _how_ it happened.

Observer follows a one-to-many model. Mediator is tends to follow a many-to-many model. For example, many reporters might submit stories that that dissemanted to many subscribers.

Mediator can be viewed as a special case of Observer. One can even implement Mediator using Observer; however, its intent and paradigm is different enough from Observer to warrent it being its own design pattern.

### Orchestration or Choreography
TBD. Need some references.

# Summary
TBD

# References
TBD

# Complete Demo Code
Here’s the entire implementation up to this point as one file. Copy and paste it into a Java environment and execute it. If you don’t have Java, try this [Online Java Environment](https://www.programiz.com/java-programming/online-compiler/). Play with the implementation. Copy and paste the code into Generative AI for analysis and comments.

# NOTES
* Observer: If using threads and sending content, need to ensure that notifications are not processed in the wrong order. Need a queue of some type.
* Observer: If Observer is storing state, then need to address the possibility of stale state information:
    * Query then Subscribe and run the risk of an update in between.
    * Subscribe then Query and run the risk of ... not sure. Need to think about this to remember the risk.
* POLA - Pub/Sub. Sub with Critical, Major and Minor and how it listed with the subscribe but it applied to all subscribers. 
* Mediator: Event Message order so definitely need a queue.
* Mediator: Event Message guarantee:
    * If not guaranteed, such as with UDP, then there's a risk of the it being lost.
    * If guaranteed, then there's the risk of multiple occurrences of the same Event Message so the receiver has to be idempotent.  
