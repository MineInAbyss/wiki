# Server Setup

## Install Docker

We use Docker to create a consistent server environment for everyone. Follow [:simple-docker: Docker's install guide](https://docs.docker.com/get-docker/).

On Windows you can run `winget install -e --id Docker.DockerDesktop` to get it. Learn more about winget [here](https://learn.microsoft.com/en-us/windows/package-manager/winget/). The Docker Desktop app is useful for starting/stopping your server and viewing logs, have a look through it later!

## Create a docker compose file

- Create a server directory wherever you like
- Add a file named `docker-compose.yml` to it
- Paste one of the following configurations into this file:

=== "Simple server"
    ```yaml
    --8<-- "docker-compose.yml"
    ```


!!! tip
    You may wish to turn certain features on or off for your server (ex. auto updating plugins.) We list all supported `environment` options for our containers [here](https://github.com/MineInAbyss/Docker).

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
