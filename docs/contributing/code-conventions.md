# Coding conventions

## Code style guide

We follow the [Android Kotlin Style Guide](https://developer.android.com/kotlin/style-guide).

IntelliJ has an autoformatter that follows this style guide, use `Ctrl+Alt+L` to run it. If the formatter makes no changes, your code style is good to go.


??? note "If you need to tell IntelliJ to use the new code style, go here"
    <img src="https://user-images.githubusercontent.com/16233018/115119043-6d144600-9f74-11eb-9ec2-59d1d5bcb42c.png" width="800">

## Idofront: our shared utils library

Idofront is a library we share between most of our plugins that adds helper functions, a command DSL, and more. Be sure to check out its [wiki](../idofront/index.md)!

## Gradle conventions

We reuse gradle configuration code with [idofront-gradle](https://github.com/MineInAbyss/Idofront/tree/master/idofront-gradle).

## Publishing

We use GitHub workflows for CI and publishing to our maven repo: [repo.mineinabyss.com](https://repo.mineinabyss.com). Please always use `com.mineinabyss` as the group id for new projects.
