---
title: Serialization
parent: Guide
grand_parent: Geary
nav_order: 10
permalink: geary/guide/serialization
---

# Persisting Components and Serialization

Geary works closely with [kotlinx.serialization](https://github.com/Kotlin/kotlinx.serialization) to allow components to be encoded, stored, and persisted.

## What do we encode for?

ktx.serialization allows us to easily encode complex structures for our components into [many formats](https://github.com/Kotlin/kotlinx.serialization/tree/master/formats).

We use this flexibility to encode both into formats like JSON or YAML that are configurable by humans, and binary formats like CBOR for persisting components.

We'll explore this more when we look at configurable prefabs, (and persistent data containers in Minecraft).

## Making your data serializable

Components, actions, and conditions don't need to be serializable. However, you are heavily encouraged to make them serializable, so they can be used within configs.

Be sure to read more about ktx.serialization, but here's a quick explanation to get you started.

Annotate a class as `@Serializable` and the properties in its constructor and inside it will become serializable so long as they themselves are also marked as `@Serializable`. This includes complex structures like lists, maps, and other custom classes.

Use `@Transient` or a delegate to avoid serializing a property in a class.

## Polymorphic serialization

ktx.serialization allows you to serialize an abstract class or interface, and let users specify which subclass of it to actually create using a `SerialName`.

Since components can be Any object, when you want to serialize any component, use `@Polymorphic GearyComponent`, ex:

```kotlin
@Serializable
class SomeData(
    // Only need @Polymorphic for components!
    val components: List<@Polymorphic GearyComponent>
    val actions: List<GearyAction>
)
```

## Autoscanning

Within the Geary extension DSL, you can ask to autoscan and register components, actions, conditions, and any custom serializable abstract class or interface. This will allow them to be used with any of the formats stored in the `Formats` class.

The autoscanner will look through classes at runtime and automatically register all subclasses of the requested interfaces.

> However, because `GearyComponent` is not an interface, and we don't just want to register ALL objects, components *must* be annotated with `@AutoscanComponent` for them to be found!

## Namespaces and SerialName

Be sure to give your components a descriptive serial name and **always specify a namespace** to avoid clashes:

```kotlin
@Serializable
@SerialName("myplugin:mydata")
class MyData(...)
```
