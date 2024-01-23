# Location actions

These actions take a location parameter that supports variables.

### Spawn entity

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

