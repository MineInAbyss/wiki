!!! tip
    Setters are all actions that run on entity load or death, but can also be used in [[Skills]].

```yaml
namespaces: [ mobzy ]
```

## Any entity

### Always show name

```yaml
set.alwaysShowName: true
```

## Living entities

### Attributes

Attributes available in vanilla Minecraft, see list on the [Minecraft wiki](https://minecraft.wiki/w/Attribute#Attributes_available_on_all_living_entities). Also includes hitbox width and height (set via internal NMS methods).

```yaml
set.attributes:
  width: 1.0
  height: 1.0
  armor: 2.0
  armorToughness: 1.0
  attackDamage: 2.0
  attackKnockback: 1.0
  attackSpeed: 1.0
  flyingSpeed: 1.0
  followRange: 32.0
  jumpStrength: 1.0
  knockbackResistance: 1.0
  luck: 1.0
  maxHealth: 30.0
  movementSpeed: 1.2
  spawnReinforcements: 0.0
```

### Potion effects

```yaml
set.potion_effects:
  - type: minecraft:speed
    duration: 7s
    amplifier: 10
  - type: minecraft:jump_boost
    duration: 7s
    amplifier: 3
```

### Can pick up items

```yaml
set.canPickupItems: false
```

### Collidable

Set if this entity will be subject to collisions with other entities.

```yaml
set.collidable: false
```
### Equipment

Sets equipment on the entity, all slots are a [[Serializable Item]]
```yaml
set.equipment:
  helmet: ...
  chestplate: ...
  leggings: ...
  boots: ...
```

### Invisible

```yaml
set.invisible: true
```

### Remove when far away

Should the entity be removed when it gets far from players

```yaml
set.removeWhenFarAway: true
```

## Monsters

### Break down doors

```yaml
set.breakDownDoor: false
```

## Sheep

### Wool color

Sets the sheep's wool color. 

```yaml
set.woolColor: RED # (1)!
```

1. Can be any value in [DyeColor](https://jd.papermc.io/paper/1.20/org/bukkit/DyeColor.html)


## Projectiles

### Display item

Sets the [[Serializable Item]] a projectile like a snowball should display.

```yaml
set.projectileItem:
  item: ...
```
