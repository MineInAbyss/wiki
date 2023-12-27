Let's make a custom slime mob. We make a prefab file `plugins/Geary/example/bigslime.yml`.
All prefabs are defined as a map of component name to component data:

```yaml title="bigslime.yml"
geary:set.entityType: minecraft:slime
mobzy:set.slimeSize: 10
```

### Namespaces

It's often annoying to remember namespaces, so we may import them at the top. Any components without a namespace will check this list in order:
```yaml title="bigslime.yml"
namespaces: [ geary, mobzy ]
set.entityType: minecraft:slime
set.slimeSize: 10
```

### Prefix keys

Many components may start with the same prefix, in this case we may choose to group them under a prefix key that ends with `*`:

```yaml title="bigslime.yml"
namespaces: [ geary, mobzy ]
set.*:
  entityType: minecraft:slime
  slimeSize: 10
```
