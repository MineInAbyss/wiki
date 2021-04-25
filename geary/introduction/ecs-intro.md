---
title: What is an ECS?
parent: Introduction
grand_parent: Geary
nav_order: 1
---

# What is an Entity Component System?

Please read, [Understanding Component-Entity-Systems](https://www.gamedev.net/tutorials/_/technical/game-programming/understanding-component-entity-systems-r3013/), an excellent article outlining basic concepts of an Entity Components System.

# Features specific to us

The engine backend is inspired heavily by [flecs](https://github.com/SanderMertens/flecs/). You can read more about its design in the [backend](../backend) section.

As an end user, most of the technical details might not be too important to you however, so here's a quick outline.

## Serializable components

We make it easy to create serializable components with the help of kotlinx.serialization. For more info see [Serialization](/geary/guide/serialization).
