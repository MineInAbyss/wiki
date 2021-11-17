---
title: Ticking systems
parent: Guide
grand_parent: Geary
nav_order: 5
permalink: geary/guide/ticking-systems
---

# Ticking systems

- A ticking system is a query that runs on a defined interval.

# Your first system

Let's start simple and create a theoretical system that switches textures for entities when they are moving.

## Given data

```kotlin
data class Texture { ... }

data class Textures(
    val idle: Texture,
    val walking: Texture
)

data class Render(
    var activeTexture: Texture
)

data class Velocity(...) {
    fun isNotZero(): Boolean
}
```

## Extend `TickingSystem`

```kotlin
object WalkingAnimationSystem : TickingSystem(interval = 10) { // measured in ticks
```

## Add accessors just like a Query
```kotlin
...
    private val ResultScope.textures by get<Textures>()
    private val ResultScope.render by get<Render>()
    private val ResultScope.velocity by get<Velocity>()
...
```

## Implement a tick function

Ticking systems provide a `ResultScope.tick()` function to avoid calling `forEach` every time.

```kotlin
...
    override fun ResultScope.tick() {
        render.activeTexture = when(velocity.isNotZero()) {
            true -> textures.walking
            false -> textures.idle
        }
    }
}
```

Sometimes we want to evaluate code once instead of running it per entity. In this case, override `tick` and call `forEach` as needed:

```kotlin
override fun tick() {
    val data = complexOperation()
    forEach { result ->
        result.entity.doSomething(data)
    }
}
```

## Register the System

Either call `Engine.addSystem(WalkingAnimationSystem)` or add it in Geary's plugin extension DSL. That's all!

# Extras

## System pipeline

It is common to want fine control over the order systems execute in. In the future, Geary will do this with a pipeline, where each system can specify what step it relates to. 

This is currently not implemented, but is of high interest.
