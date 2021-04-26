---
title: Introduction
parent: Minecraft Guide
grand_parent: Geary
nav_order: 0
---

# Geary's goal for Minecraft

Currently, we only support PaperMC (because of useful events for keeping track of all entities), but the plan is to expand to more JVM server platforms once things settle more.

Geary's main goal for Minecraft is to act as a link between existing concepts in the server and its own ECS entities. Sometimes this behaviour is complex enough to be moved into its own project (see [Other Plugins](#other-plugins).)

We want to encourage using Geary alongside existing server APIs. For example, using familiar classes from that API, or integration with existing events.

This guide will focus on using Geary with the Bukkit api (Specifically its fork PaperMC).

## Other plugins

### Looty

Looty's goal is to provide an ECS entity representation for Bukkit ItemStacks. Currently, it can only keep track of entities within a player's inventory.

### Mobzy

Mobzy provides extra components and actions specifically related to mobs. It also aids with creating custom NMS entities that use Geary to add some configurable mob behaviours.

In the future, Mobzy's goal will be to provide a completely decoupled alternative to Minecraft's entities. It will still interact with a server API, but will handle mobs entirely through packets. The goal of this is to allow for more powerful, and more efficient custom entities.

## Future plans

*(Feel free to move on with the guide and ignore this)*

As the project expands, and more platforms are supported, we might want to reuse a lot of components and make them multiplatform in a sense. Since we want to encourage using the strengths of each individual server platform, the goal won't be to allow code written for plugins on one platform to work on another (Ex. literally using the Bukkit API on a Sponge server).

Instead, Geary will need to provide its own sort of multiplatform API, with component classes that provide data which represents entities. We would then have projects for each platform that ensure the data in those components matches with what's on the server.

This is obviously a huge amount of work. However, it would be extremely helpful for providing one API that works with both fully custom ECS entities or that can wrap around Bukkit/Sponge/etc... ones.
