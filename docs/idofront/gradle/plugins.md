[:simple-gradle: ![](https://img.shields.io/maven-metadata/v?label=idofront-gradle&metadataUrl=https://repo.mineinabyss.com/releases/com/mineinabyss/idofront-gradle/maven-metadata.xml){ style="vertical-align:middle" }](https://repo.mineinabyss.com/#/releases/com/mineinabyss/idofront-gradle)

We use Gradle plugins to reuse configuration code (ex. to easily publish to our Maven repo.) Under the hood, we use the [Kotlin DSL plugin](https://docs.gradle.org/current/userguide/kotlin_dsl.html#sec:kotlin-dsl_plugin) to write our plugins concisely. These sorts of plugins are called *conventions plugins*.

## Usage

Add the mineinabyss repo to `settings.gradle.kts`
```kotlin
pluginManagement {
    repositories {
        gradlePluginPortal()
        // Add our repository to be able to access the plugin
        maven("https://repo.mineinabyss.com/releases")
    }
}
```

Apply a plugin in your `plugins { }` block. All of them start with `com.mineinabyss.conventions`. We recommend you use aliases as described in the [catalog](catalog.md) page to add our plugins. Apply them using `libs.plugins.idofront.x.y.z`.

??? tip "Tip: If you don't wish to use plugin aliases"
    you can force a version for all plugins in `pluginManagement`, so you don't have to manually specify it each time:
    ```kotlin
    val idofrontConventions: String by settings
    resolutionStrategy {
        eachPlugin {
            if (requested.id.id.startsWith("com.mineinabyss.conventions"))
                useVersion(idofrontConventions)
        }
    }
    ```

    Be sure to specify the `idofrontConventions` version in `gradle.properties`:

    ```properties
    idofrontConventions=<x.y.z>
    ```

    See maven badge at the top for the latest version.


!!! note
    Some conventions have extra config options that may be specified in your gradle.properties, they are explained further down.

## Available plugins and options

!!! note
    Plugins with configurable options use an extension with the same name as the plugin.

### `com.mineinabyss.conventions.autoversion`

- Appends version number from `RELEASE_VERSION` env variable to this project and subprojects. Used together with GitHub workflows for versioning.

---

### `com.mineinabyss.conventions.copyjar`

- Copies output artifact from `reobfJar` or `shadowJar` to a specified path marked in the `plugin_path` gradle property.

---

### `com.mineinabyss.conventions.kotlin`

- Adds the Kotlin with a version shared across our organization.

---

### `com.mineinabyss.conventions.papermc`

- Adds Paper dependencies, process resources task which replaces `${plugin_version}` in plugin.yml with the project's `version`. Adds copyJar plugin.

---

### `com.mineinabyss.conventions.nms`

- Sets up Paper's paperweight-userdev environment to automatically deobfuscate Minecraft source code.

---

### `com.mineinabyss.conventions.publication`

- Publishes to our maven repo with sources.

---

### `com.mineinabyss.conventions.testing`

- Uses jUnit platform for testing, adds kotest and mockk dependencies.
