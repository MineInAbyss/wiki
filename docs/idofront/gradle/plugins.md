
!!! info "Package [![](https://img.shields.io/maven-metadata/v?label=idofront-gradle&metadataUrl=https://repo.mineinabyss.com/releases/com/mineinabyss/idofront-gradle/maven-metadata.xml){ style="vertical-align:middle" }](https://repo.mineinabyss.com/#/releases/com/mineinabyss/idofront-gradle-loader)"

We use Gradle plugins to reuse configuration code (ex. to easily publish to our Maven repo.) Under the hood, we use the [Kotlin DSL plugin](https://docs.gradle.org/current/userguide/kotlin_dsl.html#sec:kotlin-dsl_plugin) to write our plugins concisely. These sorts of plugins are called 'conventions plugins', presumably because they set conventions to be used across projects.

## Usage

Add the mineinabyss repo to `settings.gradle.kts`
```kotlin
pluginManagement {
    repositories {
        gradlePluginPortal()
        // Add our repository to be able to access the plugin
        maven("https://repo.mineinabyss.com/releases")
    }    
    
    //Use same version across all plugins
    val idofrontConventions: String by settings
    resolutionStrategy {
        eachPlugin {
            if (requested.id.id.startsWith("com.mineinabyss.conventions"))
                useVersion(idofrontConventions)
        }
    }
}
```

Apply a plugin in your `plugins { }` block. All of them start with `com.mineinabyss.conventions`

```kotlin
plugins {
  id("com.mineinabyss.conventions.SOMETHING")
}
```

!!! note
    Some conventions have extra config options that may be specified in your gradle.properties, they are explained further down.

If you're using the `resolutionStrategy` block, be sure to specify the `idofrontConventions` version in `gradle.properties`:

```properties
idofrontConventions=<x.y.z>
```

See maven badge at the top for the latest version.

## Conventions

### `com.mineinabyss.conventions.copyjar`

- Copies a generated `shadowJar` artifact to a specified path.

```kotlin
plugin_path: String // The path to copy the jar to. (should be set in global gradle.properties.)
copyJar: Boolean = true // if false, will not run.
```

---

### `com.mineinabyss.conventions.kotlin`

- Adds Kotlin and shadowjar plugins. Applies a Java platform of our common dependencies.
- Adds a `kotlinVersion` property to the project and warns if the project already has such a property that doesn't match.

---

### `com.mineinabyss.conventions.papermc`

Adds paper dependencies, process resources task which replaces `${plugin_version}` in plugin.yml with the project's `version`. Adds copyJar plugin.

```kotlin
serverVersion: String = <current server version we use> // the full Minecraft server version name.
```

---

### `com.mineinabyss.conventions.nms`

- Sets up Paper's paperweight-userdev environment to automatically deobfuscate Minecraft source code.

---

### `com.mineinabyss.conventions.publication`

- Publishes to our maven repo with sources. Adds GitHub run number to the end of version.
```kotlin
runNumberDelimiter: String = "." // the characters to put in between the version and run number.
addRunNumber: String = true // if false, will not add run number.
publishComponentName: String = "java" // the name of the component to be published.
mineinabyssMavenUsername: String
mineinabyssMavenPassword: String
```

---

### `com.mineinabyss.conventions.testing`

- Uses jUnit platform for testing, adds kotest and mockk dependencies.
