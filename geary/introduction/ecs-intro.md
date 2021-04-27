---
title: What is an ECS?
parent: Introduction
grand_parent: Geary
nav_order: 1
---

# What is an Entity Component System (ECS)?

If you've coded a bit, consider reading, [Understanding Component-Entity-Systems](https://www.gamedev.net/tutorials/_/technical/game-programming/understanding-component-entity-systems-r3013/), an excellent article outlining basic concepts of an Entity Component System.

If not, here's a quick summary of the three main parts of an Entity Component System:

## Components

Components store some information. For example, a `Location` component might store x, y, z coordinates. 

Some components can be configured in a text file. Converting a component to and from this text file is called serialization.

## Entities

An entity is a *thing* which just holds several components. For example, an entity might be a monster in a game which might have `Location`, `Health`, or `Sprite` components.

## Systems

Systems run code on entities with specific components. For example, a `PhysicsSystem` might look for all entities with a `Position` and `Velocity`, and adjust the position based on velocity.

These are never configurable themselves, only the components may be. Because of this, if you are only planning on creating entities in a config, you will essentially never see a system.
