Components that override vanilla behaviour, provided by Mobzy. Most of these will listen to events and cancel them.

## Any entity

### Interaction

Prevents different types of interaction. If passed list is empty, will prevent all.

```yaml
prevent.interaction: [ ATTACK ]
```

!!! info "Available options: `ATTACK`, `RIGHT_CLICK`"

### Regeneration

Prevents different types of regeneration events. If passed list is empty, will prevent all.

```yaml
prevent.regeneration: [ REGEN, MAGIC ]
```

!!! info "Available options: any
of [EntityRegainHealthEvent.RegainReason](https://jd.papermc.io/paper/1.20/org/bukkit/event/entity/EntityRegainHealthEvent.RegainReason.html)"

### Prevent riding

Cancels VehicleEnterEvent on this entity.

```yaml
prevent.riding: true
```

## Animals

### Breeding

Cancels EntityEnterLoveModeEvent and EntityBreedEvent.

```yaml
prevent.breeding: { }
```
