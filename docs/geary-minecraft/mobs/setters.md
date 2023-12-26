Setters, will try to update Minecraft data whenever the entity is added to world. For example, when the mob spawns, or when a chunk loads. Plugins or vanilla interactions can change the size after, but it will always return to the set value when the chunk is reloaded. We look at a common list provided by Mobzy.

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

Sets equipment on the entity, all items defined by [SerializableItemStack]()
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

Sets the sheep's wool color. Can be any value for [DyeColor](https://jd.papermc.io/paper/1.20/org/bukkit/DyeColor.html)

```yaml
set.woolColor: RED
```


## Projectiles

### Display item

```yaml
set.projectileItem
```
