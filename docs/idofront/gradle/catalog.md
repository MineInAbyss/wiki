!!! info "Package [![](https://img.shields.io/maven-metadata/v?label=catalog&metadataUrl=https://repo.mineinabyss.com/releases/com/mineinabyss/catalog/maven-metadata.xml){ style="vertical-align:middle" }](https://repo.mineinabyss.com/#/releases/com/mineinabyss/catalog-loader)"

Gradle [versions catalog](https://docs.gradle.org/current/userguide/platforms.html) for our plugins. `idofront-catalog-shaded` creates a packaged release that shades all these dependencies, which we use for our [platform](../platforms/index.md).

!!! tip
    You can see all our defined versions [in this file](https://github.com/MineInAbyss/Idofront/blob/master/gradle/libs.versions.toml)

# Usage

## Dependencies

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
        
        // See more info section to add your own deps
        create("myLibs") {
            alias("groovy-core").to("org.codehaus.groovy:groovy:3.0.5")
        }
    }
}
```
!!! note
    Our catalog needs to be called `libs` for some of our plugins to work.

=== "build.gradle.kts"

```kotlin
dependencies {
    compileOnly(libs.kotlinx.coroutines) // You will get autocomplete here!
    implementation(libs.kotlinx.serialization)
    // etc...
}
```

## Plugin aliases

We have aliases for all Idofront plugins, which also lets you remove the `resolutionStrategy` block shown in [plugins](plugins.md):

```kotlin
@Suppress("DSL_SCOPE_VALIDATION") // The IDE shows an error without this that doesn't actually cause problems
plugins {
    alias(libs.plugins...)
}
```
