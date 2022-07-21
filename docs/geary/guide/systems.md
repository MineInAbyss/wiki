# Systems

We have a modular way to store data thanks to entities and components. Now, we want to actually do something with it. Before jumping into syntax, let's think about all the ways we could accomplish this goal.

## Component matching

The first thing we need to work with our data is a way to match the right entities. Otherwise, we might try to modify the `Health` component on an entity that doesn't have one! 

!!! info "Definition: Family"
    A family is a list of rules applied to a set of components. An entity is said to match a family iff its components match the rules set out by the family.

For instance, a family could match all entities with `Health` set, but not `Invulnerable`.

!!! info "Definition: System"
    A system is a piece of code in charge of handling one behavior for entities matching some family.

## Types of systems

Geary supports three types of systems. Let's examine how we can use them before diving into syntax.

### [`Query`](https://mineinabyss.com/Geary/geary-core/com.mineinabyss.geary.systems.query/-geary-query/) quickly gets all entities matching a family

Queries are useful for finding information on the fly, for example a query for nearby entities.

### [`Repeating system`](https://mineinabyss.com/Geary/geary-core/com.mineinabyss.geary.systems/-ticking-system/) runs a query at a fixed interval

Repeating systems are useful for information that changes with time. For instance, a repeating system could update locations of entities based on their velocity.

### [`Listener`](https://mineinabyss.com/Geary/geary-core/com.mineinabyss.geary.systems/-geary-listener/) reacts to changes in an entity's components

For many tasks, listeners are far more efficient than repeating systems because they only run when something changes. For example, a listener can react to updating the health component on entities, to remove them when it reaches zero.

