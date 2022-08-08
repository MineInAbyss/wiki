# Serialization

!!! info "Currently in main package, will be split later."

Idofront adds serializers for many data types

## Usage

Create a serializable class with kotlinx.serialization, then set the serializer for the property you need:

```kt
@Serializable
class Something(
    @Serializable(with = LocationSerializer::class)
    val location: Location
)
```

Or declare the serializer once at the top of the file:
```kt
@file:UseSerializers(UUIDSerializer::class)

...

@Serializable
class Data(
    val uuid: UUID
)
```

### List of supported classes

- Recipes (through wrapper classes `SerializableRecipe` and subclasses of `SerializableRecipeIngredients`)
- BossBar
- ItemStack (full support, not meant for user readability, see SerializableItemStack below)
- Location
- Component (via `MiniMessageSerializer`)
- PotionEffect
- UUID
- Vector
- World
- Kotlin Duration (see below)

## SerializableItemStack

Lets you define items cleanly in config, then convert them to a bukkit ItemStack. Also supports Looty prefabs.

## Recipes

Example YML from using `SerializableRecipe`
```yml
recipe:
  key: "plugin:some_recipe"
  ingredients:
    - !<shaped>
      items:
        G: # SerializableItemStack
          type: GLASS
        C:
          type: COMPASS
      configuration: |-
        | G |
        |GCG|
        | G |
  result: # SerializableItemStack
    type: COMPASS
    displayName: Fancy Compass
  # group: (optional) 
```

Many other ingredient types exist such as shapeless, cooking, smoking, campfire, etc...

## Range serializers

Ex. `IntRangeSerializer`, all of them let you define a range in the form of `start..end` or `start to end`, ex `1..4`. Writing a single number like `1` results in a range equivalent to `1..1`

## Duration

Durations are serialized as a number followed by a timescale (ex `1s` for one second.) We also add a extension functions for durations in ticks: `1.ticks` and `Duration.inWholeTicks`.

### Supported timescales:

```yml
t: Ticks
s: Seconds
m: Minutes
h: Hours
d: Days
w: Weeks
mo: Months
```
