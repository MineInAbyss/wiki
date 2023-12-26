# Setup

## Gradle

```kotlin
repositories {
    maven("https://repo.mineinabyss.com/releases")
}

dependencies {
    implementation("com.mineinabyss:geary-papermc:$gearyVersion")
}
```

!!! note
    Depend on `geary-papermc` to get all supported addons automatically, or `geary-papermc-core` for just the API.


## Initialize your plugin
The Geary plugin initializes Geary with some useful addons. Read the [general setup guide](/geary/guide/setup) for more info.

The engine will start after the first server tick, which gives plugins time to install and configure their own addons in `onEnable()` like so:

```kotlin
geary {
    autoscan(classLoader, "my.plugin.package") {
        // Register all serializable classes for use in prefabs/persisting data
        components()
        
        // Automatically add systems with @AutoScan
        all()
    }
    
    // Alternatively
    on(GearyPhase.INIT_SYSTEMS) {
        geary.pipeline.addSystems(
            // Your systems here
        )
    }

    on(GearyPhase.ENABLE) {
        // Your startup logic
    }
}
```
