---
title: Families
parent: Guide
grand_parent: Geary
nav_order: 3
permalink: geary/guide/families
---

# Families

Families define a pattern to match entities against. For example, a family may specify that an entity should have components A, B, and C.

## Defining a family
Use the `family {}` dsl:

```kotlin
family {
    has<A>()
    or {
        has<B>()
        not {
            has<C>()
            has<D>()
        }
    }
}
```

This creates an immutable family which looks for entities that have components `A and (B or not(C and D))`.

## Usage

Once created, a family can check if an entity matches it, or get a list of all entities matching the family:

> However, these operations are NOT efficient. Instead, `Queries` and `Systems` make use of families and will more efficiently keep track of appropriate entities.

```kotlin
entity in family //1: Boolean
QueryManager.getEntitiesMatching(family) //2: List<GearyEntity>
```

1. Check whether a family contains an entity
2. Request a list of all entities matching this family.
