---
title: Extra Info
parent: Contribute
nav_order: 6
---

# Extra Info

## Workflows and Maven Repo

Some of our projects have CI and GitHub Package publish workflows. GitHub packages acts like a maven repo for us to publish to, though sadly they require authentication even for public packages normally. I originally made a gradle plugin called [gpr-for-gradle](https://github.com/0ffz/gpr-for-gradle) which automatically authenticates and configure these package repos, but it's rather inconvenient for others to use.

We also now have our own maven repo at [repo.mineinabyss.com](https://repo.mineinabyss.com), but some projects still need to be migrated there.

## Gradle Code Reuse

We share some basic config in the [shared-grade](https://github.com/MineInAbyss/shared-gradle) repo by using the `apply` block. It's also now a Gradle plugin itself.

Recently we've started converting our Gradle buildscripts to use Kotlin, which lets us reuse a ton of code with the shared-gradle plugin.
