Geary-papermc provides a basic system for writing custom behaviour across all entity types (mobs, items, and blocks). These are called skills, and are defined by the `geary:skills` component.

The component takes a list of skills, here's a quick overview that we'll explore in detail below:

```yaml
skills:
  - # Event triggers - when to run this skill
    event: on.itemRightClick
    
    # Using - run on a certain entity taking part in this event
    using: itemHolder
    
    # Variables - custom data that may be used in the run section
    vars:
      - derived location spawnAt:
          read.targetBlock:
            maxDistance: 10
            
    # Conditions - all must be met for this skill to run, ex.
    conditions:
      - cooldown:
          length: 2s
          displayName: My cooldown!
          
    # Actions - extra keys represent skill actions to execute 
    # they use regular component syntax with namespaces optional
    geary:playSound: ...
    particle: ...
    
    # Subskills - run more actions, specify extra conditions, etc...
    run:
      # Simple subskill defining only an action
      - explode:
          breakBlocks: false
          location: $spawnAt # A variable reference
      # Subskill using a different entity and extra conditions
      - using: somethingElse
        conditions:
          - <extra condition>
        particle: <fancy particle>

  # Another skill triggered by another event
  - event: ...
    using: ...
    ...
```

## Event

The `event` tag defines triggers for your skills. You may find a list of event sources and their associated entities in [[Events]].

## Target selector

The `using` tag defines the entity to run actions on. We call this the `target`. [[Events]] provide some targets based on involved entities, but these can also be manually specified (ex nearby entities.)

See [[Target selectors]] for more info.

## Variables

The `vars` tag defines a list of variables to be used in actions. Not all actions support using variables, but almost always, locations or other entity references will support variables.

See [[Variables]] for more info.

## Conditions

The `conditions` tag defines a list of conditions that must be met for this skill to run.

See [[Conditions]] for more info.

## Actions

Actions get executed when the skill runs. They are defined as extra component keys, with namespaces optionally imported.

See [[Actions]] for a list of actions you may use.

## Subskills

The `run` tag defines a list of subskills to run when this skill runs. Subskills are just skills, and may be nested as deep as you like. They may also define their own conditions, actions, etc. Subskills may also define their own `using` entity, which will override the parent skill's `using` entity.
