# Quickstart

This guide is designed for more advanced devs who want to get started quickly. If you're new to programming, or want to see a more in-depth guide, continue with the guide on the sidebar.


## Private access

Let's explore how to get started with parts of the project that aren't publicly accessible. Access will be granted based on developer needs, but most of this stuff is already set up on our dev server!

### Set up auth

Let's explore some variables that you'll need to set up to access private resources.

```properties
KEEPUP_GITHUB_AUTH_TOKEN=<auth token for keepup to not get ratelimited/download private plugins if ever needed>
PACKY_ACCESS_TOKEN=<token for packy to read private repos>
PRIVATE_PLUGINS_TOKEN=<reposilite token for keepup to download private plugins>
```

### BBModels

We serve generated BBModel files using packy, and bind a clone repo from our filesystem.

```bash
# Install gh cli
cd /opt/docker/data/
gh auth login
gh repo clone MineInAbyss/BBModel-Files
```

## Download from world backup

Set up a backup remote in rclone and run the following (changing -t to your world location),

```shell
restic restore latest -r rclone:mineinabyss:build --target /opt/docker/data/minecraft/mineinabyss/world
```
