---
title: Factory Method Pattern
layouts: posts
tags: designPattern
---

## Motivation

Also known as Virtual Constructor, The Factory Method is related to the idea on which libtarirs work; a libraries
work: a library used abstract classes for defining and maintaining relations between objects. One type of
responsibility is creating such objects. The library knows when a object needs to be created,but not know what
kind of object it should be created, but not what kind of object it should create, this being specific to the
application using the library.

The Factory method works just the same way: it defindes an interface for creating an object, but leaves the choice of its type to the subclasses,ceration being deferred at run-time. A simple real life example of the
Factory Method is the hotel. When staying in a hotel you first have to check in. The person working at the front desk will give you a key to your room after you've paid for the room you want and this way he can be looked as a room factory.

## Intent

* Defines an interface for creating objects, but let subclasses to decide which class to instantiate

* Refers to the newly created object through a common interface.

## Implementation

The pattern basically works as shown below, in the UML diagram:

![factory method implementation](/pictures/factory method implementation - uml class diagram.gif)

The participants classes in this pattern are:

* Product defines the interface for objects the factory method creates.

* ConcreteProduct implements the Product interface.

* Creator (also refered as Factory because it creates the Product objects) declares the method FactoryMethod,
which returns a Product object. May call the generating method for creating Product objects.

* ConcreteCreator overrides the generating method for creating ConcreteProdcut objects

All concrete products are subclasses of the Product class, so all of them have the same basic implementation,
at some extent. The Creator class specifies all standard and generic behavior of the products and when a new
product is needed, it sends the creation details that are supplied by the client to the ConcreteCreator. Having
this diagram in mind, it is easy for us now to product the code related to it. Here is how the implementation of
the classic Factory method should look:
```java
public interface Product {}

public abstract class Creator 
{
	public void anOperation() 
	{
		Product product = factoryMethod();
	}
	
	protected abstract Product factoryMethod();
}

public class ConcreteProduct implements Product {}

public class ConcreteCreator extends Creator 
{
	protected Product factoryMethod() 
	{
		return new ConcreteProduct();
	}
}

public class Client 
{
	public static void main( String arg[] ) 
	{
		Creator creator = new ConcreteCreator();
		creator.anOperation();
	}
}
```

## Applicability & Examples

The need for implementing the Factory Method is very frequent. The cases are the ones below:

* when a class can't anticipate(预料) the type of the object it is supposed to create

* When a class wants its subclasses to be the ones to specific the type of a newly created object.

## Drawbacks and Benefits

Here are the benefits and drawbacks of factory pattern:

* + The main reason for which the factory pattern is used is that it introduces a separation between the application and a family of classes(it introduces weak coupling instead of tight coupling hiding concrete classes from the application). It provides a simple way of extending the family of products with minor changes in application code.

* + It provides customiztion hooks. When the objects are created directly inside the class it's hard to replace them by objects which extend their functionality. If a factory is used instead to create a family of objects the customized objects can easily repalce the original objects, configuring the factory to create them.

* - The factory has to be used for a family of objects. If the classes doesn't extend common base class or interface they can not be used in a factory design template.

## Hot points

The factory method is one of the most used and one of the more robust design patterns. There are only few points which have to be considered when you implement a factory method.

When you design an application just think if you really need it a factory to create objects. Maybe using it will bring unnecessary complexity in your application. Anyway if you have many object of the same base type and you manipulate them mostly as abstract objects, then you need a factory. If your have a lot of code like the following, reconsider it.

```java

if (genericProduct typeof ConcreteProduct)
	((ConcreteProduct)genericProduct).doSomeConcreteOperation();
```