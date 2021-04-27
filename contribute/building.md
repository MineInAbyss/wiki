---
title: Building
parent: Contribute
nav_order: 3
---

# Building

We use Gradle for all our Kotlin/Java projects. You do not need to install gradle, everything will be set up for you in one command! However, we highly recommend using IntelliJ as your IDE if you wish to contribute, as it's made by the same company makes Kotlin.

- Clone the repository in question.
- Enter the project directory.
- Build the project with gradle:
    * Linux/OSX: `./gradlew build`
    * Windows: `gradlew.bat build`
    * IntelliJ: Nice in-depth guide [here](https://www.jetbrains.com/help/idea/getting-started-with-gradle.html).
- Copy the jar from `build/libs` to the server plugin directory.
- Update the config files based on the samples to work for your use case.

## Automatic copyJar

Most of our projects automatically copy the generated jar to a specified `plugin_path`. To specify it globally:

- Create and open `~/.gradle/gradle.properties` (`~` being your user directory).
- Add `plugin_path=path/to/my/server/plugins`.
- Ensure you use `/` and not `\`.

## Composite builds

We use Gradle's composite builds to work on two projects at once. Most of our projects have config options in the `gradle.properties` to include certain local projects. This makes working on two projects at once super convenient. The projects must be cloned to the same folder, i.e.:

```
Projects
└───DeeperWorld
└───MineInAbyss
```
