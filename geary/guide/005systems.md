---
title: Systems
parent: Guide
grand_parent: Geary
nav_order: 5
permalink: geary/guide/systems
---

# Systems

Systems are just queries which iterate over their matched entities at a given interval. It is very important to learn how to properly design systems, but this is a big topic that applies to ECS as a whole.

> Over time, I recommend looking around for devlogs or talks where people go into detail about what exactly their systems do. If I had to pick one, this super in-depth GDC talk sticks out in my head: [Overwatch Gameplay Architecture and Netcode](https://www.youtube.com/watch?v=W3aieHjyNvw)

# Your first system

Let's start simple and create a theoretical system that switches textures for entities when they are moving. Here's our data:

```kotlin
class Texture { ... }

class Textures(
    val idle: Texture,
    val walking: Texture
)

class Render(
    var activeTexture: Texture
)

class Velocity(...) {
    fun isNotZero(): Boolean
}
```

## Create the system class

```kotlin
object WalkingAnimationSystem : TickingSystem(interval = 10)
```

We extend the `TickingSystem` class, which lets us define how often this system should run in ticks. We'll say 10 (every 0.5 seconds).

## Add accessors just like a Query
```kotlin
object WalkingAnimationSystem : TickingSystem(interval = 10) {
    private val QueryResult.textures by get<Textures>()
    private val QueryResult.render by get<Render>()
    private val QueryResult.velocity by get<Velocity>()
}
```

## Implement a tick function

`QueryResult.tick()` simply gets called on every matched entity every time the system runs.

However, sometimes we might want to evaluate something once instead of running it per entity. In this case, override `tick` and call `forEach` as needed. Keep in mind, this overrides the default implementation responsible for calling the previous function.

Let's override the first:

```kotlin
override fun QueryResult.tick() {
    render.activeTexture = when(velocity.isNotZero()) {
        true -> textures.walking
        false -> textures.idle
    }
}
```

## Register the System

Either call `Engine.addSystem(WalkingAnimationSystem)` or add it in Geary's plugin extension DSL. That's all!

# Extras

## Async & concurrent modification

Geary supports removing components while the entity is being iterated over in a system. For instance, you may want to "process" a component by iterating over all entities with it, then removing it after that system completes its job.

In the future we will also look into supporting async system iteration, where systems that iterate over non-overlapping families of components may run simultaneously.

## System pipeline

It is common to want to have fine control over what systems should run well. In the future, Geary will do this with a pipeline of steps, where each system can specify what step it relates to. 

This is currently not implemented, but is of high interest.
