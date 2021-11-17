---
title: Components 
parent: Guide 
grand_parent: Geary 
nav_order: 1
permalink: geary/guide/components
---

# Components

- Geary treats components as entities
- A typealias `GearyComponentId = GearyEntityId` exists for component IDs.
- Every entity has a `type`, a set of all the entity/component ids it holds. 
- By treating components as entities, we gain the ability to add entities to entities easily.
- Try to keep your components immutable.

# Entity type

The type is used for the `add` operation you saw earlier, but it is not enough to store data. When we want to `set` data, Geary must go through an internal system of archetypes to put it.

# Component conventions

## Use `data` classes

[Data classes](https://kotlinlang.org/docs/data-classes.html) auto-generate useful methods like `copy`, `equals`, and `hashCode`. Try to use data classes for all your components, ex:

```kotlin
data class Location(val x: Int, val y: Int, val z: Int)
```

## Immutability

Try to keep component properties immutable (`val`) unless they must be changed very frequently (ex every tick, or more.)

Immutability allows us to broadcast changes to data as events and helps with peace of mind as programs grow and becomes more concurrent.
