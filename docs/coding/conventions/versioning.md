We have a plugin called autoversion in Idofront that will automatically append a PATCH (third) version number. You can manually set the rest, ex. `0.8`

Keep the `version` and `group` properties in a project's `gradle.properties` file and try to follow the MAJOR.MINOR pattern as described by [semver](https://semver.org/), with the PATCH appended automatically by autoversion.

Use the `develop` branch to automatically create pre-releases with `-dev.x` appended to the end.
