---
title: "Prefab Factory"
layout: post
excerpt_separator: <!--more-->
---
As I was messing around with Unity, I found there was no easy way to instantiate a Prefab from where you wanted. If your class didn’t have access to the prefab there no way you could easily instantiate it.

<!--more-->

A really clean way is to have a factory class that can build the objects out for you.

A factory class is a design pattern used for creating objects. It is a centralized place where the creation of the objects can take place without other classes having to worry how to create them. This cleans up the code dramatically because every time there is an update to the type of object, you don’t need to handle that in a bunch of places.

The prefab class would have be a `GameObject` in the scene. For each prefab you want to create, you need to create reference in the factory class.

```C#
public class PrefabFactory : MonoBeahvior 
{
    public GameObject chair;
    public GameObject desk;
    public GameObject bed;
}
```

Since the class is a MonoBehavior attached to the game object, we use the Start() method to create an instance of the object so that when the static method is called, it can can access the references to the prefabs. This is because Prefabs can only be assigned in the instance of a class and not statically. 

```C#
public class PrefabFactory : MonoBeahvior 
{
    ...
    private PrefabFactory instance;
    private static readonly object _lock = new object();
    void Start() {
        if (instance == null) {
            lock (_lock) {
                instance = this;
            }
        }
    }
    
}
```

The code above checks if the instance is null and if it is, locks and sets the instance to itself. This `instance` will be used later to get the `PrefabFactory`.

You will then need to define a `PrefabType` enum. This enum indicates what kind of prefab you want to make. For each prefab, you will need to have an enum value.

```C#
public enum PrefabType 
{
    Chair,
    Desk,
    Bed
}
```

Next you need to have a static method to get the Prefab, or in engineering mumbo jumbo, Factory Method. Using the prefabType parameter, we determine the type you want to create.
```c#
public class PrefabFactory : MonoBeahvior 
{
    public GameObject chair;
    public GameObject desk;
    public GameObject bed;
    
    private PrefabManager instance;
    private static readonly object _lock = new object();
    void Start() {
        if (instance == null) {
            lock (_lock) {
                instance = this;
            }
        }
    }
    
    public static GameObject GetPrefab(PrefabType prefabType) {
        switch (prefabType) {
            case PrefabType.Chair:
                return instance.Chair;
            case PrefabType.Desk:
                return instance.Desk;
            case PrefabType.Bed:
                return instance.Bed;
            default:
                // You can also log an error here.
                return null;
        }
    }
    
    public enum PrefabType 
    {
        Chair,
        Desk,
        Bed
    }
}
```
The cleanliness of the code is a major benefit to this approach. The creating of these prefabs are put in one place so we only have to maintain one region of code. Additionally, the logic for properly creating the `GameObject` from the prefab can reside in here so other developers don’t need to know how to create the `GameObject`.

Though, having to define each prefab, create a enum, and update the factory method is a major drawback in terms of maintenance. Though, we only have to update these places and not multiple places if something changes. We also have to keep a `PrefabFactory` `GameObject` in the world, not a major drawback, but an annoying one.
