# Designing Components

## Use `data` classes when possible

[Data classes](https://kotlinlang.org/docs/data-classes.html) auto-generate useful methods like `copy`, `equals`, and `hashCode`. Try to use data classes for all your components, ex:

```kotlin
data class Location(val x: Int, val y: Int, val z: Int)
```

## Avoid inheritance

Inheritance breaks system modularity. Only use it if a library you work with already uses it. In this case, set the component both as a reasonable common parent class and as the exact class.

```kotlin
class Animal { ... }
class Pig: Animal { ... }

entity().set(Pig())
```
??? fail "Unclear whether this be set as an animal or pig"
    If we set a `Pig`, then anything that reads `Animal` will think it's not there! Similarly, if we set `Animal`, then nothing will know it's actually a `Pig`. Finally, if our hierarchy is big enough, we can't reasonably set every possibility.

<hr>

```kotlin
class Alive
data class Oinks(interval: Duration)

entity {
    add<Alive>()
    set(Oinks(5.seconds))
}
```
??? success "Each component does one thing clearly"
    We can access either component without worrying about the other, or both if we like to.


## Aim for immutable components

Keep component properties immutable unless they are absolutely performance critical and will only be used by ticking systems.

```kotlin
data class Health(var amount: Int)

val myHealth = Health(20)
entity {
    set(myHealth)
}
myHealth.amount = 0
```
!!! fail "We can change amount without anyone knowing!"

<hr>

```kotlin
data class Health(val amount: Int)

entity {
    set(Health(20))
    set(Health(0))
}
```

!!! success "Changing health must be done through Geary, listeners will always know!"
