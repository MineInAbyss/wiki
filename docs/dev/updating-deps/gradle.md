## Centralized dependency management

We manage all our shared project dependencies in Idofront. It also includes Kotlin version, Minecraft server version, and versions for other gradle plugins. This lets us quickly update our plugins just by changing the Idofront version in `gradle.properties`.

## Updating versions

To update versions everywhere, or add a new dependency that may be useful to other plugins, change the [`libs.versions.toml`](https://github.com/MineInAbyss/Idofront/blob/master/gradle/libs.versions.toml) file in Idofront.

To add the dependency to our shared platform, also add it in [idofront-catalog-shaded](https://github.com/MineInAbyss/Idofront/blob/master/idofront-catalog-shaded/build.gradle.kts). We don't include all dependencies automatically since some are meant to be shaded into each plugin, or have separate api and implementation dependencies.

!!! note
    Certain plugins shade their own dependencies (ex. guiy adds jetpack compose), but we generally discourage this unless a plugin will likely be used outside of Mine in Abyss.

### Finding all outdated dependencies in Idofront

Run `gradle dependencyUpdates` to get a list of outdated dependencies (this task comes from a gradle plugin we use in Idofront.)

## Update blockers

[Compose Multiplatform](https://github.com/JetBrains/compose-jb) is usually the biggest blocker for updating Kotlin version. You may mitigate this by changing the compiler version used by compose, but this can be buggy and generally it's better to wait for it to update.

## Update frequency

We've had many problems in the past with updating too much too fast. Aim for quarterly breaking version changes (this goes for both external and internal dependencies.) This practice should minimize how often we need to go through every repo and make changes to get some new feature live on the server.
