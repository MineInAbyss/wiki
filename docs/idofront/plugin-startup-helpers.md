# Plugin startup helpers

Here's an excerpt from another plugin using some helper methods in onEnable()


```kotlin
override fun onEnable(){
    //services register one by one
    registerService<PlayerManager>(PlayerManagerImpl())

    //registering a list of event listeners
    registerEvents(
            MovementListener(),
            PlayerListener()
    )
}
```
