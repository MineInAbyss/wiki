---
title: Defining Sections
parent: DeeperWorld
nav_order: 2
---

# Creating your world

It is up to you to create your sections however you like, with whatever overlap you like.

#### DeeperWorld simply needs to know:
- X-Z coordinates of two corners of each section.
- A reference location on one section, and where the corresponding location is in the next.

# Configuring

Open The plugin config file under `plugins/DeeperWorld/config.yml`.

## What you'll be defining:

<img src="https://user-images.githubusercontent.com/16233018/97783275-025d8280-1b6d-11eb-9a68-d15aadc1b50d.png" width="600px">

As you can see in this masterpiece of an illustration, DeeperWorld automatically knows the overlap to synchronize just by defining two locations.

There is also no restriction on differently sized regions. DeeperWorld will prevent teleports between two sections (red area) by bouncing players up/down if they would otherwise result in teleporting out of bounds.

## Example config

The plugin comes with an example config similar to the following:

```yaml
### Example config:
sections:
  - name: section1 # will show up when using /linfo
    refTop: [0, 0, 0]
    refBottom: [0, 16, 0] # a location in this section that corresponds to the lower section's refTop
    region: [0, 0, 1000, 1000] # corners for this section (x1, y1, x2, y2)
    world: world
  - name: section2
    refTop: [1000, 240, 0] # a location in this section that corresponds to the higher section's refBottom
    refBottom: [2000, 16, 0] # since this is the last section, the bottom won't matter
    region: [1000, 0, 2000, 1000]
    world: world
# The damage players will take when outside a managed section.
damageOutsideSections: 1.0
# Worlds which shouldn't damage players when outside of a section.
#damageExcludedWorlds:
#  - world
```
