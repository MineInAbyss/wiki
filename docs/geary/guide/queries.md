# Queries

Recall that queries find all entities matching a family.

!!! info "Definition: Accessor"
    A special property that can be defined in systems to automatically build families for us.

## Syntax

Let's build a query step by step. We'll find all entities with set `Health`, and added `Alive` components.

```kotlin
class Health(val amount: Int)
class Alive
```

=== ":fontawesome-solid-1:"

    Extend the `Query` class.

    ```kotlin
    class HealthQuery : GearyQuery()
    ```

=== ":fontawesome-solid-2:"

    Add an accessor that gets the `Health` component for us.

    ```kotlin
    class HealthQuery : GearyQuery() {
        val TargetScope.alive by family { has<Alive>() }
    }
    ```

=== ":fontawesome-solid-3:"

    Add an accessor that matches our desired family for the `Alive` component.

    ```kotlin
    class HealthQuery : GearyQuery() {
        val TargetScope.health by get<Health>()
        val TargetScope.alive by family { has<Alive>() }
    }
    ```

### Iteration

Every Query is an `Iterator` of `TargetScope`s, so you can call `forEach`, `asSequence`, `map`, etc... on them.

However, to use an Accessor, you must be in the scope of the query. Use `apply` to do so:

```kotlin
HealthQuery.run {
    forEach { result ->
        // Increase every matched entity's health by 1
        entity.set(Health(result.health.amount + 1))
    }
}
```

!!! note
    If we simply did `HealthQuery.forEach`, we would not be able to call the `health` property on our result!

#### Accessing the entity

`ResultScope` also gives the `entity` being iterated. You are free to add or remove any components from it concurrently, however data in accessors is immutable and **will not** update on its own.

!!! warning "Data combinations"

    When accessors return a list of data, all possible combinations of that data will be iterated over, even if some iterations include the same entity several times.
    
    For example, if a Query has a relation accessor, `relation<A>()`, there may be several components related to `A` and each will get an iteration.

### Registering

The query will register itself upon its first iteration request, but you may register it manually using:

```kotlin
QueryManager.trackQuery(MyQuery)
```
