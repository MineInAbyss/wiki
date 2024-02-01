# Quickstart

This guide is designed for more advanced devs who want to get started quickly. If you're new to programming, or want to see a more in-depth guide, continue with the guide on the sidebar.

## IDE

- Install [IntelliJ IDEA](https://www.jetbrains.com/idea/download/)
- Install a Java 17 JDK, most easily via IntelliJ using `Ctrl+Alt+Shift+S`, then clicking the dropdown in project settings.
- Our [composite-projects](https://github.com/MineInAbyss/composite-projects) repo can be useful for working on multiple projects at once, or just clone the repos you need.

## Server

- Install Docker, follow [:simple-docker: Docker's install guide](https://docs.docker.com/get-docker/).
- Create a server directory, we use `/opt/docker/data/minecraft` internally.
- Make a file named `docker-compose.yml` and paste a config from below.
- Server config and plugins will be downloaded automatically with the setup below, if you build any plugins locally, they'll override the downloaded ones.

```yaml title="docker-compose.yml"
--8<-- "docker-compose.yml"
```

!!! tip "More environment options avialable on [GitHub](https://github.com/MineInAbyss/Docker)."

## Running the server

- Install [lazydocker](https://github.com/jesseduffield/lazydocker) or [Docker Desktop](https://www.docker.com/products/docker-desktop/) for a GUI to manage your containers.
- Run `docker-compose up -d` in the directory of your `docker-compose.yml` to start the server in the background.
- Some IDEs like VSCode or Intellij let you manage Docker containers too, it may be convenient to open your entire minecraft folder to be able to modify the compose file and see local configs.
- Join in Minecraft as `localhost`
- Op yourself by attaching to your container (`a` in lazydocker) or `docker attach mineinabyss`.

## Extras

- If you need BBModels or other private assets see [[Private Assets]], or test on the dev server. You might be ratelimited by GitHub when downloading plugins, the page also explains how to set up your own token for it.
- If you need access to internal services like browsing files or restarting containers/seeing dev logs, let us know in Discord, you will be added to a team on GitHub.
