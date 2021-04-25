---
title: Serialization
parent: Guide
grand_parent: Geary
nav_order: 3
---

Systems iterate over `families` of components. Essentially, they make a query for all entities that match a certain description regarding the data they must hold. 

> In the future, we'd like to make an extensive system for complex queries (for example, all entities whose parent has a child with a certain component).

# Your first system

Let's create a system that gives our custom mobs a walking animation. To do this, we'll read the idle and walking model items from our `Model` component. We'll change between these models by changing the item in the head slot of a `BukkitEntity`.

## Create the system class

```kotlin
object WalkingAnimationSystem : TickingSystem(interval = 10) { // 1
    override fun GearyEntity.tick() { // 2
        
    }
}
```

1. We extend the `TickingSystem` class, which lets us define how often this system should run in ticks. We'll say 10 (every 0.5 seconds).
2. We must implement an extension function on `GearyEntity` which will individually run on all entities we query when the system ticks.

## Specify what components we want

An inefficient approach to doing this would be to simply get a component from every entity we tick over, and skip it if it doesn't have one:

```kotlin
override fun GearyEntity.tick() {
    val model = get<Model>() ?: return
    val mob = get<BukkitEntity>() ?: return
}
```

While this would work, this code is extremely inefficient, since we must iterate through *all* entities and check one by one whether they have the components we need!

A key part of a good ECS engine is being able to efficiently know all entities which match the components we need. Geary provides an idiomatic way of doing so:

```kotlin
object WalkingAnimationSystem : TickingSystem(interval = 10) {
    private val model by get<Model>()
    private val mob by get<BukkitEntity>()
    
    override fun GearyEntity.tick() {
    }
}
```

We use [delegated properties](https://kotlinlang.org/docs/reference/delegated-properties.html) to hide a lot of logic this system will now use to efficiently find all entities with these components.

The system will ensure that if we access `model` or `mob` within `GearyEntity.tick()` while the system iterates, it references the actual component on that entity.

## Implement the tick function

Once we have our component query set up, we may now access `model` or `mob` within the `tick` function. You can probably imagine reading data off our `Model` and changing the head slot on our `BukkitEntity` based off its velocity. That's about it!

## has check

You may also check whether an entity `has` a component by using:

```kotlin
private val mob = has<BukkitEntity>()
```

Currently, this ensures the entity has something `added` to it, **but not `set`** - meaning there is no actual data present. Since no data is present, it simply returns the entity id of that component, which you may use to remove it from the entity.

A notable example of this being used is for the `Held` component in Looty, which is present on items held by the player.

# Extras

## Async & Concurrent modification

Geary supports removing components while the entity is being iterated over in a system. For instance, you may want to "process" a component by iterating over all entities with it, then removing it after that system completes its job.

In the future we will also look into supporting async system iteration, where systems that iterate over non-overlapping families of components may run simultaneously.

## System pipeline

It is common to want to have fine control over what systems should run well. In the future, Geary will do this with a pipeline of steps, where each system can specify what step it relates to. 

This is currently not implemented, but is of high interest.
