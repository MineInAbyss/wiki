---
title: Entities
parent: Guide
grand_parent: Geary
nav_order: 0
permalink: geary/guide/entities
---

# Entities

Think of entities as bundles of data with a 64bit identifier id with the typealias `GearyEntityId = ULong`.

To avoid confusion with Minecraft entities, we use the typealias `BukkitEntity`.

## The GearyEntity class

While it is useful to represent entities as just an id internally, we often want to call functions on an entity, or store references to it.

Geary provides an [inline class](https://kotlinlang.org/docs/inline-classes.html) `GearyEntity` which will usually get compiled into a long (the one big exception is in lists and generic types).

### Converting to a GearyEntity

The convention is to make an extension function `toGeary()` to convert something to a `GearyEntity`, for example:

```kotlin
entityId.toGeary() // Long -> GearyEntity
bukkitEntity.toGeary() // Bukkit Entity -> GearyEntity
```

## Reading and manipulating data

Once we have a `GearyEntity` reference, there are many helpful functions we can use for reading or manipulating its data.

### set

Sets a component with specific data for the entity:

```kotlin
entity.set(SomeData())
```

This function takes a type parameter which you should think of as a key. We can use any supertype of our data as the component key:

```kotlin
entity.set<BukkitEntity>(player)
```

The player's type is Player, so normally the key used would be `Player`, but here we force it to use a superclass `BukkitEntity`.

> Note: You are discouraged from using inheritance, this is intended to ease interaction with code you cannot control.

### add

Add a component to an entity with a type key, without any data attached:

```kotlin
entity.add<Alive>()
```

This is commonly used for marker components that don't need to store data. Later we will explore how this feature lets us reference other entities in our entities.

### has

Checks whether an entity has a component (regardless of whether it holds data):

```kotlin
entity.has<SomeComponent>() // returns Boolean
```

### get

Reads a component of a certain type from the entity:

```kotlin
val data = entity.get<SomeData>() // returns SomeData?
```

#### Null safety

Kotlin's [null safety](https://kotlinlang.org/docs/null-safety.html) is extremely handy when trying to access components, because we are usually not aware of all the components an entity *could* have.

Null safety ensures we know what to do when a component isn't present. Here are some common use cases:

```kotlin
entity.get<A>() ?: return // 1
entity.get<B>() ?: B() // 2
entity.getOrSet<C> { C() } //3
```

1. Tries to get `A` or stops if not present.
2. Tries to get `B` or uses a default value.
3. Tries to get `C` or sets and returns a default value.


### remove

Removes any component of a certain type off of an entity:

```kotlin
entity.remove<SomeComponent>()
```

## Extra

### Can we `add` and `set` the same component
Yes, `set` takes precedence, so if data is set, it will stay set.

### Type erasure
Because of type erasure on the JVM, it's highly discouraged to use generic types with your components, because we can't
actually verify those types when getting a component. For example:

```kotlin
Engine.entity {
    set(listOf("strings")) // sets a list of strings as a component
    get<List<Int>>() // will succeed!
}
```

`get<List<Int>>()` succeeds because there is no way for us to know the generic type of the list during runtime. However, an error
will be thrown when trying to access elements of the list which we expect to be integers, but are actually strings.
