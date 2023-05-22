# Setup

Geary uses an addon system to let you load only what you need.

## Gradle setup

Add the dependencies you want, namely `geary-core` and any addons that can be found [here](https://github.com/MineInAbyss/Geary/tree/master/addons).

```kotlin
repositories {
    maven("https://repo.mineinabyss.com/releases")
}

dependencies {
    val gearyVersion = "x.y.z"
    implementation("com.mineinabyss:geary-core:$gearyVersion")
    implementation("com.mineinabyss:geary-<addon-name>:$gearyVersion")
}
```

## Initialize Geary

!!! note 
    Currently, we use global dependency injection, so only one instance of the engine can exist at a time, but it may be reset using `DI.clear()`. This will be revamped once Kotlin stabilizes context receivers.

Geary provides a helper DSL to get set up, choose an engine implementation, and install your chosen addons. 

```kotlin
geary(ArchetypeEngineModule) {

    // Install addons without configuration

    install(FileSystemAddon, FileSystem.SYSTEM)
    install(UUIDTracking)

    // Some addons provide their own DSL. It will install the addon if it's not already installed

    serialization {
        format("yml", ::YamlFormat)

        components {
            component(UUID::class, UUIDSerializer.withSerialName("geary:uuid"))
        }
    }

    autoscan(classLoader, "com.mineinabyss.geary") {
        components()
    }

    // Run custom logic on different load phases

    on(GearyPhase.INIT_SYSTEMS) {
        // custom logic to add your systems
    }

    on(GearyPhase.ENABLE) {
        println("Everything is done loading!")
    }
}
```

Both the engine module, and addons define all the data that needs to be implemented, and install logic in a `companion object`. Some provide a default module, but it can always be extended to change implementation details as needed.

The engine will be started as soon as the DSL block ends, but this behaviour can be changed by the engine module. Once started, access all implementation details using provided global variables (ex. `geary.pipeline.addSystem()`)

## Testing

Geary provides a testing module which does not start any pipeline tasks automatically and has some extra options.

```kotlin
// Simple use
geary(TestEngineModule)

// Configure some options for tests
geary(TestEngineModule, TestEngineModule(
    reuseIDsAfterRemoval = false
))

// Reset everything for a new test
DI.clear()
```
