# Server setup

We provide a Docker image that lets you quickly set up a local server.

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
            MEMORYSIZE: 2G 
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

=== "Server with Waterfall"

    !!! info
        Waterfall is a proxy that lets multiple servers talk to each other (we use it to keep survival and build connected).
        <br>
        We recommend adding two folders next to each other, one for Minecraft, the other for Waterfall.

    ```yaml
    version: "3.9"
    services:
      minecraft:
        image: ...
        ports:
          - "25564:25565"
        volumes:
          - ./minecraft:/server
      waterfall:
        image:
        ports:
          - "25565:25565"
        volumes:
          - ./waterfall:/server
    ```

## Running the server

### In Terminal

Open a terminal in the current directory and run:

- `docker-compose up` to start and see a log
- `docker-compose up -d && docker attach papermc` to start in the background and attach to the papermc server for input (ex to run a command.)
- `docker-compose stop` to stop the server

### In IntelliJ

See [Intellij Docker Guide](https://www.jetbrains.com/help/idea/docker.html).

## First run

Attach to papermc and run `op <yourname>` to give yourself permissions.

You can now put plugins into the `plugins` folder! Later we will show you how to automatically put the plugins you build into this folder.

## Automatic copyJar

Most of our projects automatically copy the generated jar to a specified `plugin_path`. To specify it globally:

- Create and open `~/.gradle/gradle.properties` (`~` being your user directory).
- Add `plugin_path=path/to/my/server/plugins`.
- Ensure you use `/` and not `\`.

!!! note
    We are working on a Docker image for local development which will help you get set up way faster.
