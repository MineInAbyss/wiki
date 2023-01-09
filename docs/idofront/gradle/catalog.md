[:simple-gradle: ![](https://img.shields.io/maven-metadata/v?label=catalog&metadataUrl=https://repo.mineinabyss.com/releases/com/mineinabyss/catalog/maven-metadata.xml){ style="vertical-align:middle" }](https://repo.mineinabyss.com/#/releases/com/mineinabyss/catalog)

Gradle [versions catalog](https://docs.gradle.org/current/userguide/platforms.html) for our plugins. `idofront-catalog-shaded` creates a packaged release that shades all these dependencies, which we use for our [platform](../platforms/index.md).

!!! tip
    You can see all our defined versions [in this file](https://github.com/MineInAbyss/Idofront/blob/master/gradle/libs.versions.toml)

## Importing our catalog

=== "settings.gradle.kts"

```kotlin
dependencyResolutionManagement {
    repositories {
        maven("https://repo.mineinabyss.com/releases")
    }

    versionCatalogs {
        create("libs") {
            from("com.mineinabyss:catalog:<version>")
        }
    }
}
```
!!! note
    Our catalog needs to be called `libs` for some of our plugins to work. If you use your own, we recommend [loading it from a file](https://docs.gradle.org/current/userguide/platforms.html#sec:importing-catalog-from-file) and using the name `myLibs`.

=== "build.gradle.kts"

```kotlin
dependencies {
    compileOnly(libs.kotlinx.coroutines) // You will get autocomplete here!
    implementation(libs.kotlinx.serialization)
    // etc...
}
```

## Using plugins from the catalog

```kotlin
@Suppress("DSL_SCOPE_VIOLATION") // The IDE shows an error without this that doesn't actually cause problems
plugins {
    alias(libs.plugins...)
}
```

## Common errors

??? failure "Plugin is already on the classpath"
    Error resolving plugin <name>

    The request for this plugin could not be satisfied because the plugin is already on the classpath with an unknown version, so compatibility cannot be checked.

This error happens when the plugin has been added by a parent project, since `alias` always tries to force a specific version, rather than just applying the plugin without version.

To fix this, apply the plugin by name instead of using `alias`:
```kotlin
plugins {
    id(libs.plugins.x.y.z.get().pluginId)
}
```

---

!!! failure "Expecting an expression"

You may get an error if no configuration comes after the `plugins` block. In this case add an empty run statement like so:

```kotlin
@Suppress("DSL_SCOPE_VIOLATION")
plugins { ... }

run {}
```

---

??? failure "LibrariesForLibs does not exist"
    org.gradle.api.UnknownDomainObjectException: Extension of type 'LibrariesForLibs' does not exist.

It is likely that one of our conventions plugins tried to read versions from the catalog via the `libs` variable, but it either hasn't been registered, or has been registered under a different name. Follow the instructions in [Using plugins from the catalog](#using-plugins-from-the-catalog).

