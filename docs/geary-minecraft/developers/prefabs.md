# Prefabs

Geary will load entities from supported formats (currently `yml`) inside `plugins/Geary/<namespace>/.../prefabname.yml`

It's also possible to load manually using `prefabs.loader.loadFromPath()` and add new formats in the addon dsl and an implementation of the `Format` class:

```kotlin
geary {
    serialization {
        format("json") { module -> ... }
    }
}
```

## Configuring prefabs

Geary comes with `geary:set.entity_type` and `geary:set.item` components which allow a prefab to be instantiated. Here are example config files:

!!! tip
    We have plugins that provide many extra components to help you configure prefabs, see our [Readme on GitHub](https://github.com/MineInAbyss/Geary-papermc#plugins-using-geary)

### Mobs
```yaml
- !<geary:set.entity_type>
  key: minecraft:iron_golem
```

### Items


#### Player-instanced items

Items with one prefab that has a `geary:player_instanced_item` component will get one instance created per player. This instance won't be removed until all items of that instance are removed from the inventory

#### Example
```yaml
- !<geary:player_instanced_item>
- !<geary:set.item>
  item:
    type: PAPER
    displayName: Chiseled Bookshelf Empty
    customModelData: 2032
```


## Instantiation in code

```kotlin
val prefabKey: PrefabKey

// Mobs
val location: Location
location.spawnFromPrefab(prefabKey)

// Items
val itemStack = itemTracking.provider.serializePrefabToItemStack(prefabKey)
```
