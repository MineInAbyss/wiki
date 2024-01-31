# Mobs

Any mob prefab must have a component that provides a Minecraft entity.

## Vanilla type
The simplest component, just picks a type registered in Minecraft. This includes any custom mobs that may be registered via NMS.
```yaml
geary:set.entityType: minecraft:slime
```

## MythicMobs

```yaml
geary:set.mythic_mob: <mythicMobKey>
```

Where `<mythicMobKey>` is the root key in your MythicMob mob file, ex:

```yaml
myCustomPig:
  Type: PIG
  Health: 20
  Options:
    ...
```
