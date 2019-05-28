---
title: Singleton Pattern
layout: posts
tags: designPattern
---

## Motivation

Sometimes it's important to have only one instance for a class. For example,
in a system there should be only one window manager(or only a file system).
Usually singletons are used for centralized management of internal or external resources and they provide a global point of access to themselves.

The singleton pattern is one of the simplest design patterns: it involves only one class which is responsible to instantiate itself, to make sure it
creates not more than one instances; in the same time it provides a global
point of access to that instance. In this case the same instance can be used
from everywhere, being impossible to invoke the constructor directly each
time.

## Intent

* Ensure that only one instance of a class is created.
* Provide a global point of access to the object.

## Implementation

The implementation involves a static member in the 'Singleton' class, a private constructor and a static public method that returns a reference to
the static member.

![singleton_implementation](/pictures/singleton_implementation_-_uml_class_diagram.gif)

The Singleton Pattern defines a getInstance operation which exposes the unique instance which is accessed by the clients. *getInstance()* is responsible for creating its class in case it is not created yet and return that instance.

```java
class Singleton {
    private static Singleton instance;

    private Singleton() {
        ...
    }
    
    public static synchronized Singleton getInstance(){

        if (instance == null)
            instance = new Singleton();

        return instance;
    }
   ...

    public void doSomething()
    {
        ...
    }
}

```

You can notice in the above code that *genInstance* method ensures that only one instance of the class is created. The constructor should not be accessible fron the outside of the class to ensure the only way of instantiating the class would be only through the *getInstance* method.

The getInstance method is used also to provide a global point of access to the object and it can be use in the following manner:

```java
Singleton.getInstance().doSomething();
```

## Applicability & Examples

According to the definition the singleton pattern should be used when there must be exactly one instance of a class, and it must be accessible to clients from a global access point. Here are some real situations where the
singleton is used.

### Example 1 - Logger Classes

The singleton pattern is used in the design fo logger classes. This classes are usually  implemented as a singleton, and provides a global logging access point in all the application components without being necessary to create an object each time a logging operations is performed.

### Example 2 - Configuration Classes

The singleton pattern is used to design the classes which provieds the configuration settings for an application. By implementing configuration classes as singleton not only that we provide a global access point, but
we also keep the instance we used a cache object. When the class is instantiated(or when a value is read) the singleton will keep the values in its internal structure. If the values are read from the database or from files this avoid to realoading the values each time the configuration parameters are used.

## Specific problems and implementation

### Thread - safe implementation for multi-threading use.

A robust singleton implementation should work in any conditions. This is why we need to ensure it works correctly when multiple threads uses it. As seen in the previous examples singleton can be used specifically in multi-thread application to make sure the read/write are synchronized.

Lazy instantiation using double locking mechanism.

The standard implementation shown in the above code is a thread safe implementation, but it's not the best thread-safe implementation because synchronization is very expensive when we are talking about the performance.
We can see that the synchronized method *getInstance* does not need to be checked for synchronization after the object is initialized. If we see that
the singleton object is already created we just return it. This optimization consist inchecking in an unsynchronized block if the object is null and if not to check again and create it in an synchronized block. This is called double locking mechanism.

```java
//Lazy instantiation using double locking mechanism.
class Singleton
{
	private static Singleton instance;

	private Singleton()
	{
	System.out.println("Singleton(): Initializing Instance");
	}

	public static Singleton getInstance()
	{
		if (instance == null)
		{
			synchronized(Singleton.class)
			{
				if (instance == null)
				{
					System.out.println("getInstance(): First time getInstance was invoked!");
					instance = new Singleton();
				}
			}            
		}

		return instance;
	}

	public void doSomething()
	{
		System.out.println("doSomething(): Singleton does something!");
	}
}
```

Early instantiation use implementation with static field

In the following implementation the singleton object is instantited when the class is loaded and not when it is first used, due to the fact that the instance menber is declared static. This is why we do't need to synchronize any portion of the code in this case. The class is loaded once this guarantee the uniquity of the object.

```java
//Lazy instantiation using double locking mechanism.
class Singleton
{
	private static Singleton instance;

	private Singleton()
	{
	System.out.println("Singleton(): Initializing Instance");
	}

	public static Singleton getInstance()
	{
		if (instance == null)
		{
			synchronized(Singleton.class)
			{
				if (instance == null)
				{
					System.out.println("getInstance(): First time getInstance was invoked!");
					instance = new Singleton();
				}
			}            
		}

		return instance;
	}

	public void doSomething()
	{
		System.out.println("doSomething(): Singleton does something!");
	}
}
```

## Multiple singleton instances if classess loaded by different classloaders.

If a class(same name, same package) is loaded by 2 different classloaders they represent 2 differnt classes in memory.

## Serialization

In java, if a singleton class implements the java.io.Serializable interface,then the singleton is serialized and can be deserialized more than once, there will be multiple instances of singleton class created. In order to avoid this the readResolve method should be implemented.

```java
public class Singleton implements Serializable {
	...

	// This method is called immediately after an object of this class is deserialized.
	// This method returns the singleton instance.
	protected Object readResolve() {
		return getInstance();
	}
}
```

## Abstract Factory and Factory Method implemented as singletons

There are certain situations when the factory should be unique. Haveing 2 factories might have undesired effects when objects are created. To ensure that a factory is unique it should be implemented as a singleton. By doing so we also avoid to instantiate the class before using it.

## Hot Spot

* **Multithreading** - A special care should be taken when singleton has to be used in a multithreading application.
* **Serialization** - when singletons are implementing serializable interface they have to implement readResolve method in order to avoid having 2 different objects.
* **Classloaders** - If the singleton class is loaded by 2 different classloaders we'll have 2 different classes, one for each classloader.
* **Global Access Point represented by the class name** - The singleton instance is obtained using the class name. At the first view this is an easy way access it, but it is not very flexible. If we need to replace the singleton class, all the reference in the code should be changed accordinglly.

