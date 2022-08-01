# Queries

!!! info "Definition: Accessor"
    A special property that can be defined in systems to automatically build families for us.

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

### Iteration

Every Query is an `Iterator` of `TargetScope`s, so you can call `forEach`, `asSequence`, `map`, etc... on them. However, when possible use `fastForEach`.

To read our accessors' data, we must enter the scope of the query, then run one of these operations like so:

```kotlin
HealthQuery.run {
    fastForEach { result ->
        // Increase every matched entity's health by 1
        result.entity.set(Health(result.health.amount + 1))
    }
}
```

!!! note
    If we simply did `HealthQuery.fastForEach { ... }`, we would not be able to call the `health` property on our result!

!!! warning "Data combinations"

    When accessors return a list of data, all possible combinations of that data will be iterated over, even if some iterations include the same entity several times.
    
    For example, if a Query has a relation accessor, `relation<A>()`, there may be several components related to `A` and each will get an iteration.
