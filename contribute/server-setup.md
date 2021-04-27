---
title: Server Setup
parent: Contribute
nav_order: 2
---

# Server setup

You will need a working Spigot/Paper server if you wish to test plugins. Some plugins currently require Paper, which provides considerably better performance than Spigot. Get the [Paper jar](https://papermc.io/downloads), then install Paper with [this guide](https://paper.readthedocs.io/en/latest/server/getting-started.html).

## Plugin dependencies

[KotlinSpice](https://github.com/MineInAbyss/KotlinSpice) is a library we've made that provides us with shared dependencies between plugins. We do this to avoid shading the Kotlin standard library to every plugin. *I have looked at switching to [pdm](https://github.com/knightzmc/pdm) for our Minecraft plugins, but it appears to be rather buggy currently.*

You may download a plugin jar for KotlinSpice by going onto the CI workflow, downloading the artifact, and unzipping it. We will eventually have automatic releases set up.

Some plugins require extra dependencies, most often ProtocolLib. We will try to list them on each plugin's README page.
