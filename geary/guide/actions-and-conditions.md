---
title: Actions & Conditions
parent: Guide
grand_parent: Geary
nav_order: 2
---

# Actions and Conditions

There are many cases where we want to check something about an entity, or run some action on it. Because we don't know the components of entities ahead of time, we can't really write functions that take certain components, so they need to take a `GearyEntity` as a parameter and individually `get` all of the components required.

`GearyAction` and `GearyCondition` allow us to be slightly more efficient when getting components, as well as letting us serialize these classes to be used in configs. 

They also work great for following composition. For example, we can ask for a `List<GearyAction>` and try running them one by one on an entity. This encourages composition, which works quite nicely as an alternative to inheritance in an ECS.

## Creating an Action or Condition

First, extend one of the classes and add all the properties you want to be configurable into its constructor. 

This is a great place to use composition. For example, you could use other existing actions/conditions in here, or use classes designed to be configurable. We'll take a look at these later.

### Conditions

Let's create a condition that checks whether the Location an entity is at has a certain light level:

```kotlin
class LightCondition(
    val range: IntRange = 0..15,
) : GearyCondition() {
    override fun GearyEntity.check(): Boolean = TODO()
}
```

Location is a component on this entity. To access it in the `check` function, we *could* just do something like: 

```kotlin
val location = get<Location>() ?: return false
```

However, this syntax gets pretty ugly and can be more efficient when getting many components. Actions and Conditions provide delegates that will automatically and efficiently ensure an entity has all the components required:

```kotlin
class LightCondition(
    val range: IntRange = 0..15,
) : GearyCondition() {
    val GearyEntity.location by get<Location>()

    override fun GearyEntity.check(): Boolean =
        location.block.lightLevel in range
}
```

We specify an extension property `GearyEntity.location`. The extension is needed to provide async support.

Finally, to check whether a condition was met for an entity, we instantiate this class and call the `metFor(entity: GearyEntity)` function:

```
LightCondition(5..10).metFor(entity)
```

### Actions

Actions follow the same format, but you must implement the function `GearyEntity.run(): Boolean`, where the boolean returned indicates whether the action did anything.

And then to run an action on an entity, `runOn(entity: GearyEntity)`.
