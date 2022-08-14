# Queries

!!! info "Definition: Accessor"
    A special property that can be defined in systems to automatically build families for us.

---

!!! info "Definition: Accessor Scope"
    A bundle of data relating to a queried entity, which an accessor can quickly read from.

Queries and repeating systems only have a `TargetScope`, because they query one entity at a time. Listeners work with more entities at once, so they have several scopes. Each accessor must specify the scope it belongs to, let's see what that looks like.

## Syntax

Let's build a query step by step. We'll find all entities with set `Health`, and added `Alive` components, and add 1 to their health.

```kotlin
class Health(val amount: Int)
class Alive
```

=== ":fontawesome-solid-1:"

    Extend the `GearyQuery` class.

    ```kotlin
    class HealthQuery : GearyQuery()
    ```

=== ":fontawesome-solid-2:"

    Add an accessor for the target which ensures we match a certain family.

    ```kotlin
    class HealthQuery : GearyQuery() {
        val TargetScope.alive by family { has<Alive>() }
    }
    ```
    
    !!! note
        If we added more accessors that specify more families, the query uses an and operation to ensure all the conditions for its accessors are met. 

=== ":fontawesome-solid-3:"

    Matching families is useful, but accessors also let us automatically read components, for example, with the `get` accessor.

    ```kotlin
    class HealthQuery : GearyQuery() {
        val TargetScope.health by get<Health>()
        val TargetScope.alive by family { has<Alive>() }
    }
    ```

    Now our query will match against entities that have `Health` set, and have `Alive`  added or set. Let's see how we can read data from the query now.

---

## Accessor types

Geary has the following accessors:

- `#!kotlin get<T>()` matches entities with components of type `T`
- `#!kotlin getOrNull/getOrDefault<T>()` gets a component of type `T`, or uses the default value

### List accessors

List accessors will run your system once for every matched item. For instance, `getRelations<ChildOf?, Any?>` will run your system for each parent.

- `#!kotlin getRelations<K, T>()` individually matches all relation combinations

### Transformers

- `#!kotlin accessor.flatten()` makes list accessors return a list instead of running many times
- `#!kotlin accessor.map()` maps any matched data to some new result

## Iteration

Every Query is an `Iterator` of `TargetScope`s, so you can call `forEach`, `asSequence`, `map`, etc... on them. However, when possible use `fastForEach`.

To read our accessors' data, we first enter the scope of the query, then run one of these operations like so:

```kotlin
HealthQuery.run {
    fastForEach { target ->
        // Increase every matched entity's health by 1
        target.entity.set(Health(target.health.amount + 1))
    }
}
```

!!! note
    If we simply did `HealthQuery.fastForEach { ... }`, we would not be able to call the `health` property on our target!
