# Conditions

## General conditions

### Chance

Specifies a percent chance for this skill to run on each trigger

```yaml
chance: 0.5
```

### Cooldown

Specifies a cooldown for this skill, the same target will not be able to run it again until the cooldown is over. Optionally, can specify a display name for players to see in hotbars, etc...

```yaml
cooldown:
  length: 2s
  displayName: <red>My cooldown! # MiniMessage component
```
