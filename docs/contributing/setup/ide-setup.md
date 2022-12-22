# IDE/Project Setup

!!! note "Recommended IDE: [:simple-intellijidea: IntelliJ](https://www.jetbrains.com/idea/download)"
    It has solid Kotlin support and we will only be able to help you resolve IDE problems in IntelliJ.

Once installed, [Set up your JDK](https://www.jetbrains.com/help/idea/sdk.html#set-up-jdk) to use the latest OpenJDK version.

## Setting up a project

Clone a project from GitHub and open it in IntelliJ. From there, it should start importing it as a [:simple-gradle: Gradle](https://docs.gradle.org/) project and you'll soon get syntax highlighting and be able to build your project.

!!! note "Note: Working on multiple projects"
    You may use our [composite master project](https://github.com/MineInAbyss/composite-master) to open several projects at once. Note it slows down build times, so you may wish to limit yourself to one at a time.

## Building

You do not need to install anything to build. You can do it from command line or IntelliJ (running builds through IntelliJ will be more convenient)

### From a terminal
=== ":simple-windows: Windows"
    ```shell
    cd <insert project directory here>
    gradlew.bat build
    ```
=== ":simple-apple: :simple-linux: MacOS/Linux"
    ```shell
    cd <insert project directory here>
    ./gradlew build
    ```

### In IntelliJ
This video shows how to run Gradle tasks like build, as well as how to work with Gradle in general. Feel free to watch the short clip or full video.

<iframe width="560" height="315" src="https://www.youtube.com/embed/6V6G3RyxEMk?start=823" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

You can also find documentation [here](https://www.jetbrains.com/help/idea/getting-started-with-gradle.html).

### Automatically copy our build

!!! tip

    Normally, running `gradle build` produces several jar files in a `build/libs` folder. If you are running a local server, copying them every time gets annoying, so we have an automated solution.

Specify a `plugin_path` in our global Gradle configuration and all our plugins will automatically copy to it:

- Create/open `~/.gradle/gradle.properties` (`~` being your user directory).
- Add `#!shell plugin_path=path/to/my/server/plugins`.
- Ensure you use `/` and not `\`.
