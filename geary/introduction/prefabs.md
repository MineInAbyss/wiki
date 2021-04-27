---
title: Prefabs
parent: Introduction
grand_parent: Geary
nav_order: 2
---

# Prefabs and Configuration

A core part of Geary is being able to create a *type* of entity, which can then be reused many times in-game. We call these entity types `Prefabs`.

The entities we create from a prefab are called `instances`. A prefab instance is the actual item or mob you see in a game, while the prefab is like a blueprint for it.

Interestingly, a prefab itself is a regular old entity (with the exception that no systems ever run on it.) Therefore, we can think of a prefab as a list of components - and if all those components are serializable - we can actually configure prefabs by hand in a file!


## Prefab Files

Geary lets plugins specify a prefab folder. Every file inside will be loaded as a prefab. You can even organize files into more folders inside. The only limitation is that no two prefab files should have the same name.

Geary will automatically figure out what format to use based on the file extension. Currently, we encourage YAML, so files ending with `.yml`.

The configuration guide will go into detail on creating prefabs for custom items with Looty, and custom mobs with Mobzy this way.
