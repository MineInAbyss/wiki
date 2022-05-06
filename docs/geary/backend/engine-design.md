---
title: Engine Design
parent: Backend
grand_parent: Geary
nav_order: 1
---

# Engine Design

This page outlines the basic design of this ECS and future plans for improvements.

# Good reads

[ECS Back and Forth](https://skypjack.github.io/2019-02-14-ecs-baf-part-1/) is a nice intro, though the ECS discussed in the series uses sparse sets which we decided not to go with.

Flecs' [Building an ECS](https://ajmmertens.medium.com/building-an-ecs-1-types-hierarchies-and-prefabs-9f07666a1e9d) series is a very interesting read and has impacted this project a lot. Flecs introduces some ideas like components being entities, or type roles, which allow for very clean parent/child relationships, prefabs, and more.

# Evolution of our engine

This ECS started off very simple, so it's a quite nice way to introduce how we got to the current implementation.

### Rudimentary bitsets implementation
- Have a map of component type to list of components of that type.
- Each entity is an index for each list.
- Have bitsets to accompany each component array that define whether or not something a component is present
- When looking for a family of components, do AND or ANDNOT operations on the bitsets to get a list of entities with matching components.

### Archetypes
Split these huge arrays with empty spaces into archetypes which only contain entities that all share the same exact component types. Now you only need to check that an archetype matches all the requested components! When we create a new archetype, we can assign it to all the appropriate systems and from there iteration has very little overhead.

The flecs blog goes more in depth into how these are implemented more specifically, and we use very similar concepts, so have a read!


