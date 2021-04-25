---
title: Entities
parent: Guide
grand_parent: Geary
nav_order: 0
---

# Entities

At its core, an ECS centers around entities which act like bundles of data called components. 

Every entity has a 64-bit id associated with it, or more specifically, this 64-bit entity id is like an index in an array, and the entity itself is never an actual instantiated object.

## The GearyEntity class

While it is useful to represent entities as just an id internally, we often want to call functions on an entity, or store references to it.

Geary provides an [inline class](https://kotlinlang.org/docs/inline-classes.html) `GearyEntity` which in most cases will be compiled down into a regular old long (the one big exception is in lists, or anything else where you would use it as a generic type). 

### Converting to a GearyEntity

Wrapping `geary(something)` is the pattern for converting a long or any other object into a `GearyEntity`. Namely, this is also used for getting a Geary entity for some `BukkitEntity` in Minecraft.

## Reading & Manipulating Data

Once we have a `GearyEntity` reference, there are many helpful functions we may use for reading or manipulating its data. Let's say `entity` is a reference to an entity.

### Entity type vs stored data

Geary treats components as entities, and as such each component has some entity id associated with it. Every entity has a `type`, a list of all the entity ids this entity holds. A side effect of this is that we may add entities to entities, and Geary allows for this, but we cover this later!

An entity type, however, isn't enough for storing data. We only know that an entity has a component, not what it actually is. When we want to set that data, Geary must go through an internal system (archetypes) to actually store the component somewhere.

Sometimes however, we just want to add a component, but not set any data. Geary allows for this and distinguishes between components added to an entity which hold data, and those that do not.

### has

Checks whether an entity has a component (regardless of whether it holds data):

```kotlin
entity.has<SomeComponent>() // returns Boolean
```

### get

Reads data of a certain type stored as a component on an entity:

```kotlin
val data = entity.get<SomeData>()
```

#### Null safety

Kotlin's [null safety](https://kotlinlang.org/docs/null-safety.html) is extremely handy when trying to access components
off entities, because we normally aren't sure of the components on an entity. Null safety ensures we know what to do when a component isn't present. Here are some common use cases:

```kotlin
entity.get<A>() ?: return // 1
entity.get<B>() ?: B() // 2
entity.getOrSet<C> { C() } //3
```

1. Tries to get `A` or stops if not present.
2. Tries to get `B` or uses a default value.
3. Tries to get `C` or sets and returns a default value.

### add

Add a component to an entity without any data attached:

```kotlin
entity.add<SomeComponent>()
```

### set

Sets a component with specific data for the entity:

```kotlin
entity.set(SomeData())
```

#### Polymorphism

When setting a component, you may set it under any superclass of your component. For instance, if we have a class Pig,
which is a subclass of BukkitEntity:

```kotlin
val pig: Pig
entity.set<BukkitEntity>(pig)
entity.get<BukkitEntity>() // returns pig
entity.get<Pig>() // fails
```

### remove

Removes any component of a certain type off of an entity, regardless of whether it holds data or not:

```kotlin
entity.remove<SomeComponent>()
```

## Extra

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
