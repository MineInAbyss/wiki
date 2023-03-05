# Server Setup

!!! warning
    Docker on Windows 11 may have issues with world corruption. Do not keep anything important on your server world!

## Install Docker

We use Docker to make images of servers that will run the same on any computer, which saves a lot of headaches! Follow [:simple-docker: Docker's install guide](https://docs.docker.com/get-docker/).

On Windows you can run `winget install -e --id Docker.DockerDesktop` to get it. Learn more about winget [here](https://learn.microsoft.com/en-us/windows/package-manager/winget/).

## Create a docker compose file

- Create a server directory wherever you like
- Add a file named `docker-compose.yml` to it
- Paste one of the following configurations into this file:

=== "Simple server"
    ```yaml
    version: "3.9"
    services:
      minecraft:
        image: ghcr.io/mineinabyss/papermc_dev:master
        container_name: papermc
        ports:
          - "25565:25565"
        volumes:
          # Stores server files in a folder named papermc next to your compose.yml
          - ./papermc:/data:rw
        environment:
            MEMORYSIZE: 2G
            PAPERMC_FLAGS: "" # Shows colors in console
        # Allows 'docker attach' to work
        stdin_open: true
        tty: true
    ```

??? info "Info: :penguin: Linux users"
    Set your user/group id to avoid permission issues, ex:
    ```yaml
    environment:
        PUID: 1000
        PGID: 1000
    ```

!!! tip
    You may wish to turn certain features on or off for your server (ex. auto updating plugins.) We list all supported `environment` options for our containers [here](https://github.com/MineInAbyss/Docker).

## Auto download plugins

The dev image comes with [Keepup](https://github.com/MineInAbyss/Keepup/), our tool for managing plugins. It will automatically download our latest plugin config file to `keepup/mineinabyss.conf` and merge it with `keepup/local.conf`. You can inherit plugins from mineinabyss in `local.conf` as follows:

#### Inherit plugins from specific server
```json
local: ${mineinabyss.servers.survival}
```

#### Inherit multiple parts
```json
local: ${mineinabyss.core} ${mineinabyss.plugins}
```

#### Add your own plugins, or inherit specific ones
```json
local: ${mineinabyss.core} {
  some-plugin: "https://..."
  blocky: ${mineinabys.plugins.blocky}
}
```

!!! note
    Keepup will not install a plugin if a similar one already exist in your plugins folder. So if you build a plugin locally, your version will be used. Remember to delete the file if you want updates!

## Running the server

### In Terminal

Open a terminal in the directory of your `docker-compose.yml` and run:

```shell
docker-compose up -d && docker attach papermc
```

To start in the background and attach to the papermc server for commands.

??? tip "Tip: Use an alias for convenience"
    Linux/MacOS terminals usually let you modify their startup commands, here's an example I use to start my server by calling `papermc`:

    ```shell
    alias papermc="docker-compose -f /path/to/my/docker-compose.yml up -d && docker attach papermc"
    ```
### In IntelliJ

IntelliJ has some visual features for running containers right from the IDE. This is for more advanced devs.

<iframe width="560" height="315" src="https://www.youtube.com/embed/ck6xQqSOlpw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Documentation here: [Intellij Docker Guide](https://www.jetbrains.com/help/idea/docker.html).
