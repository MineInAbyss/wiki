# NMS

```kt
implementation("com.mineinabyss:idofront-nms:<version>")
```

## Conversions

We provide `toBukkit` and `toNMS` extension functions for some common NMS types, as well as typealiases starting with NMS (ex `NMSItemStack`.)

Classes include: Entity, Player, World, Inventory, ItemStack

Example:
```kt
bukkitEntity.toNMS() // returns NMSEntity
nmsEntity.toBukkit() // returns BukkitEntity
```

## WrappedPDC

A persistent data container (PDC) implementation that wraps a `CompoundTag`.

### Fast Item PDC

We include an extension `ItemStack.fastPDC` which will bypass bukkit's meta cloning that adds a lot of overhead when dealing with big PDCs and takes the nbt directly from NMS.
