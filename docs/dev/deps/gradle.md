We have a big version catalog in Idofront that all other projects depend on. It includes versions for Kotlin, Minecraft, common gradle plugins, other Paper plugins, and more. These can also be overridden for specific projects when importing the catalog.

## Updating/adding dependencies

To update versions everywhere, or add a new dependency that may be useful to other plugins, change the [`libs.versions.toml`](https://github.com/MineInAbyss/Idofront/blob/master/gradle/libs.versions.toml) file in Idofront.

Only deps in the `platform` bundle will be shaded into the Idofront server jar. A lot of dependencies are other plugins, or compiler plugins that don't need to be in the plugin jar.

### Automatically update dependencies in Idofront

Run `gradle versionCatalogUpdate` in Idofront to automatically update the `libs.versions.toml` file, by default it ignores prereleases.

## Update blockers

[Compose Multiplatform](https://github.com/JetBrains/compose-jb) is usually the biggest blocker for updating Kotlin version. You may mitigate this by changing the compiler version used by compose, but this can be buggy and generally it's better to wait for it to update.

## Update frequency

We've had many problems in the past with updating too much too fast. Don't make breaking version changes more than like once a quarter, updating all the plugins is a pain :sob:.
