# Platforms

Platforms make it easy to reuse dependencies across plugins, while preventing interference with other plugins.

## Mine in Abyss platform

Our plugins share many libraries, for simplicity we provide one platform for everything.

### Installation

- Download the latest platform from [GitHub](https://github.com/MineInAbyss/Idofront/releases/latest)
- Place the `.platform` file into your plugins folder

## Developer usage

```kt
implementation("com.mineinabyss:idofront-platform-loader:<version>")
```

### What problems do platforms solve?

#### Limitations of spigot's library loader

Spigot has a built in system for downloading dependencies, but it only allows libraries hosted on the maven central repository for security reasons. Since platforms are packaged ahead of time, you may include anything you like in them safely.

#### Version conflicts

When each plugin handles dependencies on its own, you must be careful to update all dependencies in your suite of plugins to avoid odd errors (ex two of your plugins accidentally depend on different Kotlin versions that cannot interact with each other.)

Since platforms force all your plugins to use one file, version errors are a lot more obvious. They will usually come up on startup and force users to update their platform, rather than showing up as mysterious bugs across plugin interactions.

#### Isolation

Shading dependencies into your plugin leaks them to other plugins, and certain dependency loading solutions may give you someone else's shaded dependencies.

Idofront platforms hook into Spigot's classloader to ensure only platform dependencies have access to the platform, and that those classes are always loaded before other plugins.

### Downsides

Outside plugins that wish to interact with you may need to load your platform to function too. We also don't have a good way to resolve conflicts when two platforms shade the same dependency.

### Creating platforms

Generate a jar file that contains all the dependencies you need (ex, with the [shadowjar](https://imperceptiblethoughts.com/shadow/getting-started/) gradle plugin.)

If you want to place the file inside the plugins folder, it must not end in `.jar`, `.platform` is recommended instead.

### Loading platforms

Inside the onLoad function, define:

```kt
IdofrontPlatforms.load(this, "platformName")
```

This will load `platformName.platform` inside the plugins folder. You may load several in one plugin.

#### Custom load logic

Perhaps you have extra logic that automatically downloads your platform files and wish to load them in a custom folder, in this case you may manually pass a file to load.

