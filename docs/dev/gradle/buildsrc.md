# buildSrc

You can make a `buildSrc` folder in your project which lets you reuse code across gradle files within the project.

This is useful for some of our projects that are split into dozens of subprojects, but we generally haven't used it due to some limitations

## Limitations

- Adding plugins to share them across config files is a lot harder (instead of a `plugins` block with a version specified, you need to add them into `dependencies`, find the full name, and keep track of version in there)
  - This is especially annoying for our idofront plugins
- Using [version catalogs](version-catalogs.md) is more complicated.
