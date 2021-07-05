---
title: Queries
parent: Guide
grand_parent: Geary
nav_order: 4
permalink: geary/guide/queries
---

# Queries

Queries are like fancy families. In fact, a query literally is a family builder like the `family {}` block.

The main benefits of queries over families are:
- Queries cache all matched archetypes, which makes iterating over all matched entities very fast.
- Queries have `Accessor`s: [delegated properties](https://kotlinlang.org/docs/reference/delegated-properties.html) which allow you not only to select for entities, but also efficiently read data off them.
- (FUTURE) Subqueries that more efficiently find matching entities.

## Creating a Query

Because queries have accessible properties through accessors, to actually use those properties, they must be defined as a class.

Since it is very uncommon to want more than one instance of a Query, using singleton `object`s is recommended.

```kotlin
object MyQuery : Query()
```

Currently, queries also need to be registered to track matched archetypes:

```kotlin
QueryManager.trackQuery(MyQuery)
```

Future
{: .label .label-yellow }
In the future this will be done automatically and you will have some options about what to prioritize (namely memory or performance) while extending `Query`.

## Accessors

Queries provide many accessor delegates, as well as a small API for creating custom ones. These accessors add onto the family and efficiently read data when iterating entities.

They also must be defined as extension properties of `QueryResult`, since they read data from that context:

```kotlin
class Health(var amount: Int)

object HealthQuery : Query() {
    private val QueryResult.health by get<Health>()
}
```

Some other accessors include:
- `relation<T>()`
- `relationWithData<T>()`

## Iteration

Queries act as `Iterator`s. You can simply call `forEach`, `asSequence`, `map`, etc... on them. This will iterate over `QueryResult`s, but to access data from them, you need to be in the right context, like so:

```kotlin
HealthQuery.apply {
    forEach { result ->
        result.health.amount += 1
    }
}
```

If we simply did `HealthQuery.forEach`, we would not be able to call the `health` extension function on our result!

### Accessing the entity

The result also contains the `entity` that it corresponds to. You are free to add or remove any components from it concurrently.

## Regular family operations

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

Remember these operations are done *in addition* to those requested by accessors. This means you can only further narrow down what entities you want.

## Data combinations

When multiple accessors returns a list of data, all possible combinations of that data will be iterated.

For example, if a Query has two `relation` accessors: `relation<A>()` and `relation<B>()`, running `forEach` will individually iterate over every relation in `A` matched up with every relation in `B`.

> P.S. I was SO gonna find how to write this out with proper set notation, but I can't Google properly.
