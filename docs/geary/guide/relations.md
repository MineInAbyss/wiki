# Relations

Earlier we saw how Geary uses component types (classes like `Health, Location`) as a sort of key for setting or adding components. Internally, Geary assigns an entity id for each key. This means component types are entities within Geary.

Relations take advantage of this fact, and that we will almost never need the full 64 bits in our entity id. They combine two entity ids together to relate two entities, or an entity-component pair.

!!! info "Definition: Relation"

    A relation is a component type that is made up of two entity or component type ids in any combination.

## Component-component relation

A relation can be made from a `Persisting` component type and any other component type to indicate a component should be persisted. Let's see this in action:

```kotlin
class Persisting
class Health(val amount: Int)

val entity = entity {
    set(Health(20))
    set(Persisting::class to Health::class)
}

entity.get(Persisting::class to Any::class)
// Returns listOf(
entity.relations
```
