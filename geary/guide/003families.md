---
title: Families
parent: Guide
grand_parent: Geary
nav_order: 3
permalink: geary/guide/families
---

# What is a Family

Families define some pattern to match entities against. For example, a family may specify that an entity should have components A, B, and C.

Once created, a family may either check if it contains an entity type, or request to get all entities matching itself. However, these operations are NOT cached. Instead, `Queries` and `Systems` make use of families and will more efficiently keep track of appropriate entities.

Internally, families are made up of different selector classes like AND, OR, NOT. Each are an instance of the `Family` class and contain a list of other selectors. It's a bit hard to explain in text, so just see how families are defined below.

## Defining a Family

Geary has both mutable and immutable family classes. The mutable family builder classes come with a DSL for cleaner code. Use the `family {}` block to access it:

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

This creates an immutable family which looks for entities that have `A and (B or not(C and D))`.

This may look weird at first, but it allows us to add onto the DSL with other scope-dependent functions.

# Using Families Manually

Internally, queries use these functions, but there may be special cases where you want to use them yourself:

```kotlin
entity.type in family //1: Boolean
QueryManager.getEntitiesMatching(family) //2: List<GearyEntity>
```
1. Check whether a family contains an entity
2. Request a list of all entities matching this family.
