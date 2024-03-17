# Quickstart

Let's set up a local server and dev environment. If you're new to programming, continue with the guide on the sidebar and come back here afterward!

## IDE

- Install [:simple-intellijidea: IntelliJ IDEA](https://www.jetbrains.com/idea/download/)
- Install a Java 17 JDK, most easily via IntelliJ using `Ctrl+Alt+Shift+S`, then clicking the dropdown in project settings.
- Set your server plugin path in `~/.gradle/gradle.properties` (see [Configuring gradle](#configuring-gradle) below), we also recommend some other settings for better performance.
- Build the project with gradle (see [Building a project](#building-a-project) below).
- Our [composite-projects](https://github.com/MineInAbyss/composite-projects) repo can be useful for working on multiple projects at once, or just clone the repos you need.

## Server

- Install Docker, follow [:simple-docker: Docker's install guide](https://docs.docker.com/get-docker/).
- Create a server directory, we use `/opt/docker/data/minecraft` internally.
- Make a file named `docker-compose.yml` and paste a config from below.
- Server config and plugins will be downloaded automatically using Keepup, if you build any plugins locally, they'll override the downloaded ones!
- You can create a `.env` file in the same directory to pass secrets (ex. to download our mob models)

=== "Simple server"
    ```yaml
    --8<-- "docker-compose.yml"
    ```

[//]: # (=== "Proxy and server")
[//]: # (    ```yaml title="docker-compose.yml")
[//]: # (    --8<-- "docker-compose.yml")
[//]: # (    ```)

!!! tip "More environment options avialable on [GitHub](https://github.com/MineInAbyss/Docker)."

### Running the server

- Install [lazydocker](https://github.com/jesseduffield/lazydocker) or [Docker Desktop](https://www.docker.com/products/docker-desktop/) for a GUI to manage your containers.
- Run `docker-compose up -d` in the directory of your `docker-compose.yml` to start the server in the background.
- Some IDEs like VSCode or Intellij let you manage Docker containers too, it may be convenient to open your entire minecraft folder to be able to modify the compose file and see local configs.
- Join in Minecraft as `localhost`
- Op yourself by attaching to your container (`a` in lazydocker) or `docker attach mineinabyss`.

## Extras

### Configuring gradle

Gradle has a global configuration file located at `~/.gradle/gradle.properties` (`~` being your user directory). Create the file and add the lines below:

```properties
plugin_path=path/to/my/server/plugins
paperweight.experimental.sharedCaches=true
```

- `plugin_path` will instruct our builds to copy the final jar into this path
- `paperweight.experimental.sharedCaches` will reuse the same decompiled Minecraft server across projects, saving a lot of time


### Building a project

You do not need to install anything to build. You can do it from command line or IntelliJ (running builds through IntelliJ will be more convenient)

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

??? info "IntelliJ gradle tutorial"
    This video shows how to run Gradle tasks like build, as well as how to work with Gradle in general. Feel free to watch the short clip or full video.

    <iframe width="560" height="315" src="https://www.youtube.com/embed/6V6G3RyxEMk?start=823" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    
    You can also find documentation [here](https://www.jetbrains.com/help/idea/getting-started-with-gradle.html).

### Private assets

- If you need BBModels or other private assets see [[Private Assets]], or test on the dev server. You might be ratelimited by GitHub when downloading plugins, the page also explains how to set up your own token for it.
- If you need access to internal services like browsing files or restarting containers/seeing dev logs, let us know in Discord, you will be added to a team on GitHub.

### Podman

Podman is a Docker alternative that's sometimes easier to set up and can run our docker-compose file! Here are some tips to get it running:

- `pipx install podman-compose` to install podman-compose.
- `sudo podman-compose up -d` to start the server in the background.
- Placing `PODMAN_USERNS=keep-id` in your `.env` lets you run the server as your own user, be sure to append `:z` to any volume mounts in your compose file to avoid issues with SELinux.
- [Pods](https://flathub.org/apps/com.github.marhkb.Pods) is a nice UI for managing your containers.
