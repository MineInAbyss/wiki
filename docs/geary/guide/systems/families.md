# Defining families

!!! warning
    The current system is convoluted, this page describes the planned rewrite

Recall that families define rules regarding components. Geary lets us define those rules using the `family {}` builder.

Families have operations for selecting entities, which can be joined by three connectives, `and, not, or`, evaluated left to right.

## Operations

Families have a counterpart for three entity operations: `has`, `hasSet`, and `hasRelation` which match against all entities where those operations would return `#!kotlin true`.

## Usage

Let's look at an example family made of several operations:

```kotlin
val family = family { (has<A> and not(has<B>)) or has<C> }
```

Once created, a family can check if an entity matches it with `#!kotlin entity in family // Boolean`. More importantly, we can now use them in our systems for fast pattern matching.
