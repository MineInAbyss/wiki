A component integrating with ModelEngine 4 is provided in Mobzy:

```yaml
mobzy:set.modelengineModel:
    modelId: "mymob" # (1)!
    invisible: true # (2)!
    damageTint: true  # (3)!
    nametag: true # (4)!
    stepHeight: 0.0 # (5)!
    scale: 1 # (6)!
    verticalCull: # (7)!
      type: <CullType>
      distance: <Double>
      ignoreRadius: <Double>
    backCull: # (8)!
      type: <CullType>
      angle: <Double>
      ignoreRadius: <Double>
    blockedCull: # (9)!
      type: <CullType>
      angle: <Double>
      ignoreRadius: <Double>
```

1. **String**, The name of your model in MEG
2. **Boolean**, default `true`<br>Whether the base entity should be visible
3. **Boolean**, default `true`<br>Whether to tint the model red on hit
4. **Boolean**, default `true`<br>
5. a
6. **DoubleRange**, default `1`<br>How much to scale this entity by, randomly picks in range (ex. `1..1.5`)
7. **VerticalCull**, default `null`<br>Override default MEG vertical culling settings
8. **BackCull**, default `null`<br>Override default MEG back cull settings
9. **BlockedCull**, default `null`<br>Override default MEG blocked cull settings

| Data type | Allowed values                   |
|-----------|----------------------------------|
| CullType  | `NO_CULL, MOVEMENT_ONLY, CULLED` |

