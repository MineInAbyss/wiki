---
title: Components 
parent: Guide 
grand_parent: Geary 
nav_order: 1
permalink: geary/guide/components
---

# Components

Geary does not have an interface that components must extend. Any object can be added to an entity as a component. In
general, components should only contain data, or some helper methods to access/mutate that data.

## Useful Kotlin language features

[Data classes](https://kotlinlang.org/docs/data-classes.html) are often useful for auto-generating a lot of code for
your components, including destructure functions that allow you to do something like this:

```kotlin
val (x, y, z) = entity.get<Location>()
```

Additionally, try to use immutability where it makes sense.
