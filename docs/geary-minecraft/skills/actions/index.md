# Actions

!!! tip
    All setters for mobs are valid actions (ex. `set.woolColor`), if present directly on a prefab, they will simply choose a default event trigger (usually on spawn or death.)

## General actions

### Run skills

Runs skills, finding them by prefab name. Create a new prefab and add a skill using the `geary:skill` component, then reference it by file name.

```yaml
runSkills:
    - "myproject:some_skill"
    - "justASkillName"
```

### Run MythicMobs skills

Runs MythicMobs skills. These can be tested with the `mm test cast` command and should copy its behaviour. We're planning to add support for MM's built in mechanic syntax.

```yaml
runMythicSkills:
    - "skill1"
    - "skill2"
```
