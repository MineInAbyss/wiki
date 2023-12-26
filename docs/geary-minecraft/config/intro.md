We define custom mobs, items, or blocks in config files, which get loaded as *prefabs*. Prefabs hold a list of components that will be added to an entity when it gets created.

When we spawn an entity, it is an *instance* of a prefab. It may use data directly on the prefab, or copy it onto itself (ex. for persistent data that may change per entity.)

## Directory structure

Place Geary config files in `plugins/Geary/<namespace>`, where `<namespace>` is unique to your project (ex. mineinabyss).
Files will be loaded by name, regardless of folder structure. Feel free to organize things yourself.
Multiple file formats are supported, but this guide will exclusively use `.yml` files for configs.

## Commands

`/geary prefab reload <namespace>:<name>` will re-read a prefab from disk. It will be updated live, but instances will not be updated until a chunk reload or restart.

`/geary prefab load <namespace> path/to/file.yml` will load a file that was not previously loaded by geary.
