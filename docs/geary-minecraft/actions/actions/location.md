# Location actions

These actions take a location parameter that supports variables.

```yaml
namespaces: [ geary ]
```

### Spawn entity

```yaml
spawn:
  at: <location input>
  entity: <entity reference> #Entity may be referenced by prefab key or defined in-place as a list of components.
```

### Particles

```yaml
particle:
  at: <location input>
  particle: minecraft:cloud # Minecraft particle name
  # Double range of offsets, defaults to 0
  offsetX: 0
  offsetY: 0.5-1.0
  offsetZ: 0
  color: red # Particle color if applicable
  count: 2-4
  radius: 32 # Radius of players to send particle to, defaults to 32
  speed: 0.1-0.2 # Speed range of particle, defaults to 0
```


### Explode

```yaml
explode:
  at: <location input>
  power: 4.0 # Explosion power
  setFire: false # Should blocks around have fire set to them?
  breakBlocks: false # Should blocks be broken by the explosion?
  fuseTicks: 0 # How long should the fuse be (0 means instant explosion)?
```

### Knockback

```yaml
knockback:
  center: <location input> # Center of knockback
  power: 1.0 # Knockback power
  yAngle: 45 # Knockback angle in degrees (positive means all knockback has an upwards component)
  scaleWithDistance: true # Should power reduce with distance?
  cancelCurrentVelocity: true # Should velocity be appended or overridden?
```
