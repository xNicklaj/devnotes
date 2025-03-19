---
tags: 
aliases:
  - Scriptable Objects Architectural Pattern
---
**Scriptable Objects** are a new way for Unity to store data, and are generally used only for static data (Player Stats, Item Stats, Enemy Stats, Settings, etc...) however they can be used for so much more... 

You can use Scriptable Objects to save anything that can be static in the game. Object Pools? Scriptable. Loot Tables? Scriptables. User Settings? Scene Loader? Scriptable.
You can basically replace all [[Singleton|singletons]] with Scriptable Objects.


> [!warning] 
> As useful as they are, what many developers fail to understand is that scriptable data changed in runtime does not persist across sessions. A scriptable's default state is generated when building the game, so if you need your data to persist you'll have to serialize it and save it somehow.


## Controlling GameObjects behaviour via ScriptableEvents
You can insert an **Event** in a scriptable and use it to control the execution order of functions for GameObjects.

![[Pasted image 20250319091349.png]]

This create some sort of **Single Entry Point** without even having to touch code.

## Runtime Variables with ScriptableObjects

```csharp
[CreateAssetMenu()]
```

[^1]: [Unity Architecture: Scriptable Objects Pattern](https://medium.com/@simon.nordon/unity-architecture-scriptable-object-pattern-0a6c25b2d741)