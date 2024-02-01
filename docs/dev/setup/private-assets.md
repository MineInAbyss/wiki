# Private assets

Let's explore how to get started with parts of the project that aren't publicly accessible. Access will be granted based on developer needs, but most of this stuff is already set up on our dev server!

## Set up resources auth

Let's explore some variables that you'll need to set up to access private resources. Pass these along into docker-compose.

```properties
KEEPUP_GITHUB_AUTH_TOKEN=<auth token for keepup to not get ratelimited/download private plugins if ever needed>
PACKY_ACCESS_TOKEN=<token for packy to read private repos>
PRIVATE_PLUGINS_TOKEN=<reposilite token for keepup to download private plugins>
```

## BBModels

We serve generated BBModel files using packy, and bind a clone repo from our filesystem. With the GitHub CLI installed you can do,

```bash
cd /opt/docker/data/
gh auth login
gh repo clone MineInAbyss/BBModel-Files
```

Then bind the volume,

```yaml
volumes:
  - "/opt/docker/data/BBModel-Files/blueprints:/data/plugins/ModelEngine/blueprints"
```

## Download from world backup

Set up a backup remote in rclone and run the following (changing -t to your world location),

```properties
restic restore latest -r rclone:mineinabyss:build --target /opt/docker/data/minecraft/mineinabyss/world
```
