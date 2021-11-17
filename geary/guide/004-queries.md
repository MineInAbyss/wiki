---
title: Queries
parent: Guide
grand_parent: Geary
nav_order: 4
permalink: geary/guide/queries
---

# Queries

- Queries request a certain `Family` of entities.
- They provide `Accessors` ([delegated properties](https://kotlinlang.org/docs/reference/delegated-properties.html)) to efficiently read data from matched entities.
- Queries track all matched archetypes, which makes iterating over all matched entities very fast.
- The query class is an iterator so `forEach` can be used directly on one.

# Creating a Query

## Extend the `Query` class

Since it is very uncommon to want more than one instance of a Query, using singleton `object`s is recommended:

```kotlin
object MyQuery : Query()
```

## Request components

### Family operations

You can use exactly the same operations described in [Families](families) in the Query's init block:

```kotlin
object : Query {
    init {
        or {
            has<A>()
            has<B>()
        }
    }
}
```


### Accessors
Accessor delegates allow you to request data and read it within a `ResultScope`:

```kotlin
class Health(var amount: Int)

object HealthQuery : Query() {
    val ResultScope.health by get<Health>()
}
```

You can find other accessors in the `AccessorHolder` class.

# Using a query

## Iteration

Every Query is an `Iterator` of `ResultScope`s, so you can call `forEach`, `asSequence`, `map`, etc... on them.

However, to use an Accessor, you must be in the scope of the query. Use `apply` to do so:

```kotlin
HealthQuery.apply {
    forEach { result ->
        result.health.amount += 1
    }
}
```

> If we simply did `HealthQuery.forEach`, we would not be able to call the `health` extension function on our result!

### Accessing the entity

`ResultScope` also gives the `entity` being iterated. You are free to add or remove any components from it concurrently, however data in accessors is immutable and **will not** update on its own.

### Data combinations

When accessors return a list of data, all possible combinations of that data will be iterated over, even if some iterations include the same entity several times.

For example, if a Query has a relation accessor, `relation<A>()`, there may be several components related to `A` and each will get an iteration.

## Registering

The query will register itself upon its first iteration request, but you may register it manually using:

```kotlin
QueryManager.trackQuery(MyQuery)
```
