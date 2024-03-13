# Quickstart

!!! warning "WIP, in-development syntax"
    This page is a work in progress and includes syntax currently in development versions of Geary. Continue to [Entities](entities.md) for the rest of the guide.

This page gives an overview of Geary's syntax and features, the rest of the guide will dive more in depth and assume less knowledge about ecs. Think of this as a *Geary by example* guide.

## Setup

```kotlin
geary(ArchetypeEngineModule) {
    // install wanted addons here
}
```

- Geary currently creates one global instance, support for 'worlds' with separate entities coming later
- Most of Geary is prepared for Kotlin multiplatform but currently only the JVM target is fully implemented

## Define components

``` kotlin
data class Position(var x: Double, var y: Double)
data class Velocity(var x: Double, var y: Double)
sealed class Alive
```

- Any class can act as a component
- Data classes recommended
- Mutable vars are allowed but won't automatically trigger component changed events unless manually calling `set`
- Use sealed class for 'marker' components that don't have data attached to them.

## Manage entities

```kotlin
// create
val entity = entity()

// remove
entity.removeEntity()

val entity2 = entity {
    ...
} // equivalent to entity().apply { ... }
```

### Set data

```kotlin
val exampleEntity = entity {
    set(Position(0.0, 0.0)) // Set Position component
    set(Position(1.0, 0.0)) // Override previous Position component with new data
    set<Velocity>(Velocity(1.0, 1.0)) // Explicitly define type
    remove<Velocity>() // Unset Velocity component
    add<Alive>() // Adds component without data
}
```

### Read data

```kotlin
exampleEntity.apply {
    get<Position>() // return type is Position? (in this case, Position(1.0, 0.0))
    get<Alive> // return type is Alive? (in this case, null)
    has<Alive> // returns true since we added earlier
    has<Velocity> // returns false since we removed the component
}
```

## Queries

Queries are used to operate on a list of entities with specific data at runtime.

```kotlin
// We construct delegates for our data which will automatically match the correct entities
// Pointer represents the location of an entity delegates can read data from
class MyQuery: GearyQuery() {
    var position by get<Position>()
    val velocity by get<Velocity>()
}

// This syntax tracks the query for fast matching
val query = geary.cachedQuery(MyQuery())

// get all entities with both position and velocity components
val matchedEntities: List<Entity> = query.entities()

// Extract data from our query
val dataFromMatched: List<Pair<Position, Velocity>> = query.map { position to velocity }

// Run directly on query
query.forEach {
    // we can modify position directly because we declared its delegate as var
    position.x += velocity.x
    position.y += velocity.y
}
```

- Delegates have little overhead, they point almost directly to data in memory.
- Collecting to a list is much slower than iterating directly with forEach since memory needs to be allocated for the list.

### Inline queries

If you don't need query data to be publicly accessible, you can use anonymous objects in-place. We'll be using this syntax from now on

```kotlin
// We need to manually erase the type or make query private, Kotlin doesn't allow public anonymous objects
val query: CachedQueryRunner<*> = geary.cachedQuery(object: Query() {
    val position by get<Position>()
    val velocity by get<Velocity>()
})

query.entities()
```

### Ensure block

We can also match arbitrary families, in this case `this` refers to our queried entity, so we use the following syntax:

```kotlin
object: Query() {
    override fun ensure() = this { or { has<Position>(); has<Velocity>() } }
}
```

## Systems

Systems are cached queries with an `exec` block attached, they can also be repeating.

```kotlin
// We define a function to create our system, note we use GearyModule as an extension to pollute tab-completions less
fun GearyModule.createVelocitySystem() = system(object: Query() {
    val position by get<Position>()
    val velocity by get<Velocity>()
}).exec {
    position.x += velocity.x
    position.y += velocity.y
}

// Run exec on all entities matching the query
geary.createVelocitySystem().tick()
```

### Repeating systems

```kotlin
// We prefer defining systems in functions
fun GearyModule.createVelocitySystem() = system(object: Query() { ... })
    .every(1.seconds) // Duration used to calculate every n engine ticks the system should exec
    .exec { ... }

geary.createVelocitySystem()
geary.tick() // ticks all registered repeating systems
```

### Deferred systems

Systems cannot safely or quickly perform entity type modifications (i.e. component add or remove calls, we can only modify already set data, and even this can't call component modify events). Instead, we can iterate over all matched entities, gather data, and then perform modifications once the system completes:

```kotlin
fun GearyModule.createDeferredSystem() = system(object : Query() {
    val string by get<String>()
}).defer {
    // This could be a heavier calculation that benefits from memory being close together!
    return string.length > 10
}.onFinish { result: Boolean, entity: GearyEntity ->
    // We can safely do entity modifications here, everything runs in sync!
    if (result) {
        entity.add<TooLong>()
        entity.remove<String>()
    }
}
```

## Events

Events are entities, which can be called on other entities:

```kotlin
val console = entity {
    set<PrintStream>(System.out)
}

data class SendMessage(val message: String)

val event = entity {
    set(SendMessage("Hello world!"))
}

// We call console the target entity, and event the event entity
console.callEvent(event) // can optionally pass a source entity too
```

## Event listeners

Event listeners can handle event calls with appropriate data (note these aren't nearly as optimized as query iteration):

```kotlin
fun GearyModule.createMessageListener() = listener(object : ListenerQuery() {
    // notice we get access to three entities here, the target (`this`), event, and source
    val printStream by get<PrintStream>
    val sendMessage by event.get<SendMessage>
}).exec {
    printStream.println(sendMessage.message)
}

geary.createMessageListener()

// now using the call from earlier will print out "Hello world!"
console.callEvent(event)
```

- Events can modify entities live since they run synchronously, `this.entity` or `event.entity` can be called to do modifications.

## Data change listeners

Most data modifications fire an event with an appropriate component to describe what data changed. Geary provides syntax for listening to these events:

```kotlin
fun GearyModule.createListener() = listener(object : ListenerQuery() {
    val position by get<Position>()
    val velocity by get<Velocity>()
    override fun ensure() = event.anySet(::position, ::velocity)
}).exec {
    println("Position or Velocity changed on an entity that has both!")
}

geary.createListener()
val entity = entity()

// Nothing gets fired since we match BOTH position AND velocity AND a change in (position OR velocity)
entity.set(Position(0.0, 0.0))
entity.set(Velocity(0.0, 0.0)) // our println fires
entity.set(Position(0.0, 0.0)) // our println fires again
```

- If we wish to listen to modification of components we're not already querying for, we can match against a family on the event entity: `event { onSet<SomeComponent>() }`
- There are some extra `any...` functions available for the event entity for different types of modifications! (likewise `on...`)
