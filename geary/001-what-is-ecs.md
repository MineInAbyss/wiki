---
title: What is an ECS?
parent: Geary
nav_order: 1
permalink: geary/what-is-ecs
---

# What is an Entity Component System (ECS)?
At its core ECS is a way of splitting data and code. `Components` are just data

> `Location` might be a component that stores x, y, z coordinates.

We create `entities` that hold components, and nothing else.

> The player, a monster, or an item would all be entities, each with their own sets of components.

`Systems` then look for this data in different ways (Geary does this mainly through `queries`), and run code based on it.

> `PhysicsSystem` might be a system that looks for all entities with a `Location` and `Velocity` and updates both according to some newtonian physics.

This is a very broad level explanation, so here are more thorough recommendations for you to read:

- [Understanding Component-Entity-Systems](https://www.gamedev.net/tutorials/_/technical/game-programming/understanding-component-entity-systems-r3013/), an excellent article outlining basic concepts of an Entity Component System.
- [ECS Wikipedia page](https://en.wikipedia.org/wiki/Entity_component_system)
