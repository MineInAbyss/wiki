!!! important
    We describe usage and some common errors in [Idofront Gradle Catalog](/idofront/gradle/catalog/).

## Using catalogs in Gradle plugins

We use this method in Idofront for getting our version catalog within our own gradle plugins:

```kotlin
val libs = the<LibrariesForLibs>()
```

This causes some limitations described in catalog and a better method would be nice
