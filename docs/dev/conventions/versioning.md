We have a plugin called autoversion in Idofront that will automatically append a third version number.

Keep the `version` and `group` properties in a project's `gradle.properties` file and try to follow the MAJOR.MINOR pattern as described by [semver](https://semver.org/), with the PATCH appended automatically by autoversion.

## Future plans

Forcing pull requests and applying a major/minor/patch tag to the PR could be a nicer way of handling plugin versions and making sure we follow semver standards.
