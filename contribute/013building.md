---
title: Building
parent: Contribute
nav_order: 13
---

# Building

We use Gradle for all our Kotlin/Java projects. You do not need to install anything to build, however, we recommend building through IntelliJ if you wish to contribute.

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

When working on multiple projects at once, use our [composite master project](https://github.com/MineInAbyss/CompositeMegaproject9000).
