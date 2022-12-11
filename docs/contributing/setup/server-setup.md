# Server Setup

We use Docker to quickly set up a local server.

!!! warning
    Docker on Windows 11 may have issues with world corruption. Do not keep anything important on your server world!

## Install Docker

Follow [these instructions](https://docs.docker.com/get-docker/) to install Docker.

## Create a docker compose file

- Create a server directory wherever you like
- Add a file named `docker-compose.yml` to it
- Paste one of the following configurations into this file:

=== "Simple server"
    ```yaml
    version: "3.9"
    services:
      minecraft:
        image: marctv/minecraft-papermc-server:1.19 # Update as needed (1)
        container_name: papermc
        ports:
          - "25565:25565"
        volumes:
          # Stores server files in a folder named papermc next to your compose.yml
          - ./papermc:/data:rw
        environment:
            MEMORYSIZE: 2G # Try to keep low
            PAPERMC_FLAGS: "" # Shows colors in console
        # Allows 'docker attach' to work
        stdin_open: true
        tty: true
    ```

    1. You may also use `latest`. **don't** add a third number like `1.19.2`.

    ??? info "Info: :penguin: Linux users"
        Set your user/group id to avoid permission issues, ex:
        ```yaml
        environment:
            PUID: 1000
            PGID: 1000
        ```

## Running the server

### In Terminal

Open a terminal in the directory of your docker-compose file and run:

- `#!shell docker-compose up -d && docker attach papermc` to start in the background and attach to the papermc server for input (to be able to run commands.)

!!! tip "Tip: Use an alias for convenience"
    Linux/MacOS terminals usually let you modify their startup commands, here's an example I use to start my server by calling `papermc`:

    ```shell
    alias papermc="docker-compose -f /path/to/my/docker-compose.yml up -d && docker attach papermc"
    ```
### In IntelliJ

<iframe width="560" height="315" src="https://www.youtube.com/embed/ck6xQqSOlpw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Documentation here: [Intellij Docker Guide](https://www.jetbrains.com/help/idea/docker.html).

## First run

Attach to papermc and run `op <yourname>` to give yourself permissions.

You can now put plugins into the `plugins` folder! Later we will show you how to automatically put the plugins you build into this folder.
