We use several plugins across our configs, this page will try to link to the appropriate documentation and explain what each plugin does.

## Geary

Geary is our own plugin for creating custom items, blocks, and mob spawning. These configs follow a very similar pattern, you create prefab files under `pluigns/Geary/prefabs` which are just a list of "components" that describe what the prefab is. For example, the `set.item` component tells Geary a prefab is an item and should have a certain name, lore, custom model data, etc...

Note that Geary mostly doesn't enforce a file structure. We try to keep files organized ourselves but there is no strict format to follow. Here are some example file locations under the `feat` folder:

| Example                                       | Path                                                                                |
|-----------------------------------------------|-------------------------------------------------------------------------------------|
| Mob drop                                      | `items/plugins/Geary/prefabs/mineinabyss/items/drops/abyssal_snail`                 |
| Mob spawns for layer 1 hostile mobs           | `mobs/plugins/Geary/spawns/mineinabyss/layer1/hostile.yml`                          |
| Custom recipe for a vanilla item              | `items/plugins/Geary/prefabs/minecraft/recipes/book.yml`                            |
| Bamboo chair furniture piece                  | `blocks/plugins/Geary/prefabs/mineinabyss/blocks/furniture/bamboo/bamboo_chair.yml` |
| Crate block                                   | `blocks/plugins/Geary/prefabs/mineinabyss/blocks/building/crate1.yml`               |
| Prefab that prevents allays from regenerating | `mobs/plugins/Geary/prefabs/mineinabyss/mobs/custom_allay.yml`                      |

[Read the Geary docs here.](../geary-minecraft/content-packs/file-structure.md)

## MythicMobs

MythicMobs is a popular plugin for creating custom mobs with custom behaviours. It comes with many features, but we mostly just use it for creating our custom mobs and for its extensive "skill" system which lets us define fancy actions for mobs (ex. how the Silkfang shoots webs at players that stun them for a few seconds.)

Example configs:

| Example                       | Path                                                              |
|-------------------------------|-------------------------------------------------------------------|
| The mob file for a Silkfang   | `mobs/plugins/MythicMobs/mobs/mineinabyss/hostile/silkfang.yml`   |
| The skills used by a Silkfang | `mobs/plugins/MythicMobs/skills/mineinabyss/hostile/silkfang.yml` |

[Read the MythicMobs docs here.](https://git.mythiccraft.io/mythiccraft/MythicMobs/-/wikis/Guides)
