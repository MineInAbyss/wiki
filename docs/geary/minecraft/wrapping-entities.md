---
title: Wrapping Entities
parent: Minecraft Guide
grand_parent: Geary
nav_order: 1
---

# Minecraft entities

The vanilla server code is very inheritance based. In other words, it's just a hierarchy of classes, where we can instantiate one to spawn an entity into the world.

> Just to avoid confusion, we'll refer to vanilla entities as `BukkitEntity`. In fact, this is a typealias for Bukkit's `Entity` interface.

The server also has `EntityTypes` objects that store extra data for each mob type. These are namely used for setting attributes like max-health, or the width and height of mobs. Mobzy handles injecting these types into the server for custom entities.

Still, inheritance gets painful to work with, which is why we started this ECS in the first place. But inevitably, we will need to interact with Minecraft code. The rest of this page will explain some logic about how we link Geary entities to Bukkit entities.

# Wrapping Bukkit Entities

We often want to wrap around existing bukkit entities and attach our own components to them. To the end user converting between the two looks like:

```kotlin
geary(bukkitEntity) // 1
gearyOrNull(bukkitEntity) // 2
gearyEntity.get<BukkitEntity>() // 3
gearyEntity.get<Player>() // 4
```

1. Convert `Bukkit -> Geary` or create a `GearyEntity` associated with this `BukkitEntity`.

2. Convert `Bukkit -> Geary` or return `null`.

3. Convert `Geary -> Bukkit`.

   Will always work if we know this GearyEntity represents one in Bukkit.

4. Convert `Geary -> A specific entity type`.

   Geary will automatically add the Bukkit class specific to our entity type alongside a BukkitEntity. Remember, this does not add the whole inheritance hierarchy, so the entity needs to be *exactly* of this type!

## More in-depth look

### bukkit -> geary

Internally, `BukkitEntityAccess` manages a map of `UUID`s to `GearyEntity` objects. The plugin extension DSL allows us to register things that should happen to the ECS entity when a Bukkit entity gets added or removed.

There are listeners to add players into the ECS on login, and remove Geary entities from the ECS when the bukkit entity is removed. Currently, we `DO NOT` add all entities into the ECS when they are added to the world due to performance reasons.

To convert from Bukkit to Geary, we just use the UUID map.

### geary -> bukkit

When we create a wrapper for a Bukkit entity, we simply add it as a component. We're guaranteed this entity always exists because we know a listener will remove our GearyEntity when the Bukkit one gets removed from the world.
