# Server setup

You will need a working [PaperMC](https://papermc.io) server if you wish to test plugins locally.

Get the [Paper jar](https://papermc.io/downloads), then install Paper with [this guide](https://paper.readthedocs.io/en/latest/server/getting-started.html). Most of our plugins have setup guides you can follow to continue.

## Automatic copyJar

Most of our projects automatically copy the generated jar to a specified `plugin_path`. To specify it globally:

- Create and open `~/.gradle/gradle.properties` (`~` being your user directory).
- Add `plugin_path=path/to/my/server/plugins`.
- Ensure you use `/` and not `\`.

!!! note
    We are working on a Docker image for local development which will help you get set up way faster.
