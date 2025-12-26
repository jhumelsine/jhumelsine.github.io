---
title: WORK IN PROGRESS – Communication Patterns
description: TBD
unlisted: true
---

<img src="https://media0.giphy.com/media/v1.Y2lkPTc5MGI3NjExb2ZzcWdvbXQ1ajM0dWFjY2xiZjN4dHBjcDdjZG45NW13bTFlaG4wNyZlcD12MV9pbnRlcm5hbF9naWZfYnlfaWQmY3Q9Zw/3oEjHLFk88eqsmJ5F6/giphy.gif" alt="Cool Hand Luke Failure To Communicate" title="Image Source: https://giphy.com/gifs/3oEjHLFk88eqsmJ5F6" width = "45%" align="center" style="padding-right: 20px;">

# Introduction
About three decades ago, I worked on a project and someone asked me, "Can you call my function when this thing happens in your code?"

"Sure, I can do that," I replied. So I made the change.

A few days later, he stopped by and said, "Turns out, I don't need you to call me function. You can take it out." So I removed the call to his function.

A bit later, he returned and sheepishly said, "I know I asked you to remove that call to my function, but it turns out, I do need it. Can you add it back in?" Geesh, make up your mind, but I added it, since he needed it.

A few years later, I had moved to another company. This time I was the requester. I wanted to know when something happened in another developer's code. I made the same request, and his response was, "No. I can't do that. You should have asked for that during the design review." I kindly reminded him that that the design review happened about three months before I had joined the company. Then thinking about the previous job, I thought to myself, "Hey! I could have denied the request with that response back then!?"

One of the __Communication Design Patterns__ would have solve my communication issues smoothly regardless of whether I was providing information or requesting it.

# Communication Design Patterns
Unlike the Captain in [___Cool Hand Luke___](https://en.wikipedia.org/wiki/Cool_Hand_Luke), who blames the inmates for their inability to follow his rigid authority as communication failure, the Communication Design Patterns foster communication, but in a decoupled way. Coupling was the problem I encountered decades ago as described above. Had I known these patterns, I could have saved myself and others a lot of headaches.

I only classify two Gang of Four (GoF) design patterns as communication patterns:
* __Observer__ (TBD), which allows any number of interested parties to receive update notifications
* __Mediator__ (TBD), which describes a communication network of Domain Events (TBD)

One would think that there would be more than two communication patterns. One can also argue that many if not all of the GoF's design patterns are about communication. However, up to this point, that communication has been a side-effect of delegation and propagation. Services define a [contract](https://jhumelsine.github.io/2025/06/10/contracts.html) and client code calls it. The service is passive. In the communication patterns, the service takes on a more active role.

I will briefly introduce the two Communication Design Patterns here and provide details in subsequent blog entries.

## Observer
__Observer__ would have solve both problems I described in the [Introduction](#introduction). In Observer, any number of interested parties will receive notifications when the information they're interested has been updated. It requires a minor enhancment made by the developer whose code is reponsible for that information, and then that developer is out of the loop.

The interested parties can subscribe to and unsubscribe from update notifications as needed without an impact upon the code that manages the updates. The may only be several subscribers, thousands of them or zero. It's of no consequence to the managing code.

There are several variations to consider when implementing Observer, and I'll illustrate them in subsequent blog entries.

This pattern is also called [___Pub/Sub___](https://en.wikipedia.org/wiki/Publish%E2%80%93subscribe_pattern) because the information manager publishes notifications to subscribers.

## Mediator

# Summary
TBD

# References
TBD

# Complete Demo Code
Here’s the entire implementation up to this point as one file. Copy and paste it into a Java environment and execute it. If you don’t have Java, try this [Online Java Environment](https://www.programiz.com/java-programming/online-compiler/). Play with the implementation. Copy and paste the code into Generative AI for analysis and comments.
