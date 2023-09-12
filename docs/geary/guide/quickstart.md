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
data class Postion(var x: Double, var y: Double)
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

## Set data

```kotlin
val exampleEntity = entity {
    set(Position(0.0, 0.0)) // Set Position component
    set(Position(1.0, 0.0)) // Override previous Position component with new data
    set<Velocity>(Velocity(1.0, 1.0)) // Explicitly define type
    remove<Velocity>() // Unset Velocity component
    add<Alive>() // Adds component without data
}
```

## Read data

```kotlin
exampleEntity.apply {
    get<Position>() // return type is Position? (in this case, Position(1.0, 0.0))
    get<Alive> // return type is Alive? (in this case, null)
    has<Alive> // returns true since we added earlier
    has<Velocity> // returns false since we removed the component
}
```

## Query

Queries are used to operate on a list of entities with specific data at runtime.

```kotlin
class MyQuery: GearyQuery() {
    // We construct delegates for our data which will automatically match the correct entities
    // Pointer represents the location of an entity delegates can read data from
    var Pointer.position by get<Position>()
    val Pointer.velocity by get<Velocity>()
}

val query = MyQuery()

// get all entities with both position and velocity components
val matchedEntities: List<Entity> = query.toList()

// Extract data
val dataFromMatched: List<Pair<Position, Velocity>> = query.run { // run allows us to use the delegates we made
    toList { position to velocity }
}

// Run directly on query
query.run {
    forEach {
        // we can modify position directly because we declared its delegate as var
        position.x += velocity.x
        position.y += velocity.y
    }
}
```

- Delegates have little overhead, they point almost directly to data in memory.
- Collecting to a list is much slower than iterating directly with forEach

## Repeating system

```kotlin
class VelocitySystem : GearyRepeatingSystem() {
    private val Pointer.velocity by get<Velocity>()
    private var Pointer.position by get<Position>()

    // Will run on each matched entity
    override fun Pointer.tick() {
        position.x += velocity.x
        position.y += velocity.y
    }
}

// Register our system
val system = VelocitySystem()
geary.pipeline.addSystem(system)

// Tick engine
geary.tick() // matches all registered systems to appropriate entities and runs the `tick` function
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
class MessageListener: GearyListener() {
    // pointers holds pointers for target and event entities (optionally source too)
    // notice we specify which entity to get data on
    val Pointers.printStream by get<PrintStream>.on(target)
    val Pointers.sendMessage by get<SendMessage>.on(event)
    
    fun Pointers.handle() {
        printStream.println(sendMessage.message)
    }
}

geary.pipeline.addSystem(MessageListener())
// now using the call from earlier will print out "Hello world!"
console.callEvent(event)
```

- For events that process data, consider using `get<...>().removable()` and setting to null to remove a component after being processed.

## Data change listeners
