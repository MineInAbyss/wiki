# Repeating systems

- A repeating system is a query that runs on a defined interval.

## Your first system

Let's start simple and create a theoretical system that switches textures for entities when they are moving.

### Given data

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

### Extend `TickingSystem`

```kotlin
object WalkingAnimationSystem : TickingSystem(interval = 1.seconds) {
```

### Add accessors just like a Query
```kotlin
...
    private val TargetScope.textures by get<Textures>()
    private val TargetScope.render by get<Render>()
    private val TargetScope.velocity by get<Velocity>()
...
```

### Implement a tick function

Ticking systems provide a `TargetScope.tick()` function to avoid calling `forEach` every time.

```kotlin
...
    override fun TargetScope.tick() {
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
    fastForEach { result ->
        result.entity.doSomething(data)
    }
}
```

## Extras

### System pipeline

It is common to want fine control over the order systems execute in. In the future, Geary will do this with a pipeline, where each system can specify what step it relates to. 

This is currently not implemented, but is of high interest.
