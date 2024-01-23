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

The `event` tag defines triggers for your skills. You may find a list of event sources and their associated entities in [[Events]]. Specifying multiple events at once is not yet supported, but a global skill entity may be defined and called.

This tag follows ususal component format, with namespaces optionally imported.

## Using

The `using` tag defines the entity to run actions on, internally we call this the `target`. [[Events]] have a list of possible entities to use, these are carried down to subskills, and subskills may swap back and forth between these targets as needed.

This tag follows ususal component format, with namespaces optionally imported.

## Variables

The `vars` tag defines a list of variables to be used in actions. Not all actions support using variables, but almost always, locations or other entity references will support variables.

Each variable specifies a type, followed by a camelCased name, and may be specified in one of three ways:

### Inline

Since all types are just component references, they may be defined like any serializable component:

```yaml
vars:
  - string name: "Just a string"
  - int age: 42
  - geary:playSound extinguish:
      sound: minecraft:entity.generic.extinguish_fire
```

### References

Variables may reference other variables defined before them
```yaml
vars:
  - string name: $otherName
```

### Derived

Derived variables run an event that reads data on a target. These are useful for getting data from your events. See [[Derived Variables]] for a list of options.

```yaml
using: itemHolder
vars:
  # Read the location of itemHolder
  - derived location targetLoc:
      read.location: { }
```

## Conditions

The `conditions` tag defines a list of conditions that must be met for this skill to run. See [[Conditions]] for more info.

## Actions

Actions get executed when the skill runs. They are defined as extra component keys, with namespaces optionally imported. Some actions support passing variables into them, and some require the `used` entity to be a certain type (ex. a Zombie or an item).

See [[Actions]] for a list of actions you may use.

## Subskills

The `run` tag defines a list of subskills to run when this skill runs. Subskills are just skills, and may be nested as deep as you like. They may also define their own conditions, actions, etc. Subskills may also define their own `using` entity, which will override the parent skill's `using` entity.
