# Coding conventions

## Code style guide

We follow the [Android Kotlin Style Guide](https://developer.android.com/kotlin/style-guide).

IntelliJ has an autoformatter that follows this style guide, use `Ctrl+Alt+L` to run it. If the formatter makes no changes, your code style is good to go.


??? note "If you need to tell IntelliJ to use the new code style, go here"
    <img src="https://user-images.githubusercontent.com/16233018/115119043-6d144600-9f74-11eb-9ec2-59d1d5bcb42c.png" width="800">

## Use conventional commits

We use [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) to format our commit messages. You can see tags you can use that will show up in changelogs [here](https://github.com/MineInAbyss/publish-action/blob/develop/mineinabyss-cliff.toml). You can use multi-line messages to add more than one tag at a time. Here's an example valid commit

```plaintext
feat!: I made breaking changes!
fix: I fixed a bug

Here's a longer explanation

chore: I did some boring stuff
```

## Idofront

Idofront is a library we share between most of our plugins that adds helper functions, a command DSL, and more. You can find documentation for it in the [Idofront](/idofront) tab.

## Publishing

Some of our projects are libraries that other projects need to depend on. For example, MineInAbyss depends on DeeperWorld. We have our own maven repository ([repo.mineinabyss.com](https://repo.mineinabyss.com)) which lets us add these dependencies in Gradle.

Please use `com.mineinabyss` as the group id for new projects for consistency.
