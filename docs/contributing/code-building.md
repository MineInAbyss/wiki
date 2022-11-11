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
