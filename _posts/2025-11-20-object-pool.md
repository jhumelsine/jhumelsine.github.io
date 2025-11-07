---
title: WORK IN PROGRESS – Object Pool Design Pattern
description: TBD
unlisted: true
---

# Introduction

# Summary

# References

+++++++++++++++++++++++++++++++++++++
# NOTES

Not in the GoF, but probably should have been.

I consider it a creational pattern even if the objects are not being created. The client is still acquiring them.

Feels similar to Flyweight structurally, but it's different. Both involve collections of objects. Flyweight objects are shared. Object Pool objects are not shared.

Object Pools are used when an object is resource intensive. It make take a lot of time to instantiate, or it may be tied to a limited resource.

Even if most haven't implemented an Object Pool, most have used one. A Thread Pool is an object pool.

Real world object pools:
* Secretarial Pool - think Mad Men.
* Bowling Shoe rentals
* Library books

Objects need to be returned to the pool otherwise you'll end up with a drained pool. I don't think that work references will work. Need a means to release the object. Maybe something like close() might help.

Objects need to be cleaned of any intrinsic state, otherwise, you could end up with a cess pool.

What to do when an object is requested, but the pool is empty?
* Block wait.
* Block wait with timeout.
* Exception immediately.
* Create a new object on the spot.

As for an implementation, consider using a Queue. May need an `initialize(arguments)` method as well.

Still have to deal with concurrency and state. Memory leaks is a different topic. This pattern will always leak in that objects are added to the pool at start up and they remain as long as the process is running.
