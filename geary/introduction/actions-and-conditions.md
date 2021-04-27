---
title: Actions & Conditions
parent: Introduction
grand_parent: Geary
nav_order: 3
---

# Actions and Conditions

Actions and conditions are used everywhere when making custom prefabs. Combined with events, they're what actually drive custom items in Looty, or interesting mob behaviours in Mobzy.

## Actions

Actions do *something* on an entity if it has the right components. They can also be configured. For example, a `DealDamageAction` would have a parameter for the amount of damage to deal. When we run the action on an entity, it will try to deal that amount of damage to it.

## Conditions

Conditions are really similar to actions in how they are configurable, but their purpose is only to run a check, not to actually do anything. For example, the `LightCondition` ensures an entity with a Location component has a certain light level.
