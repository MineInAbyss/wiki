# Entity actions

Entity actions require the target entity (defined by `using`) to be a Minecraft entity.

```yaml
namespaces: [ geary ]
```

### Damage

```yaml
damage:
  damage: 5-10 # Random amount of damage to deal
  minHealth: 0.0 # Minimum amount of health the damaged entity should end up with
  ignoreArmor: true # Ignores armor damage protection
```

