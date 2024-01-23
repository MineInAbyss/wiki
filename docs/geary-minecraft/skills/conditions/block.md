# Block conditions

All of these require a location input, for which variables are allowed. Defaults to Location on event, if present.

### Block type

Checks the block type against allowed and denied materials.

```yaml
check.block_type:
  allow: # Material must match one of these if specified
    - minecraft:stone
    - minecraft:dirt
  deny: # Material must not match any of these if specified
    - minecraft:air
  at: <location input>
```

### Height

```yaml
check.height:
  range: 0-256 # Range of allowed heights
  at: <location input>
```

### Light level

```yaml
check.light:
  range: 0-15 # Range of allowed light levels
  at: <location input>
```

### Time

```yaml
check.time:
  min: 0 # min time in ticks
  max: 10000 # max time in ticks
  at: <location input> # Will use this location's world to check time
```
