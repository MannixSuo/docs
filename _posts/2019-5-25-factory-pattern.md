---
title: Factory Pattern
layout: posts
tags: designPattern
---

## Motivation

The Factory Design Pattern is probably the most used design pattern
in modern programming languages like Java and C#. It comes in different
variants and implementations. If you are searching for it, most likely, you'll
find references about the GoF patterns: Factory Method and Abstrace Factory.

In this article we'll describe a flavor of factory pattern commonly used nowdays.
You can also check the original Factory Method pattern which is very similar.

## Intent

1. create objects without exposing the instantiation logic to the client.

2. refers to the newly created object through a common interface.

## Implementation

![factory implementation](/pictures/factory_implementation.gif)

The implementation is really simple

1. The client needs a product, but instead of creating it directly using the new
operator, it asks the factory object for a new product, providing the information
about the type of object it needs.

2. The factory instantiates a new concrete product and then returns to the client the
newly created product(casted to abstract product class).

3. The client uses the products as abstract products without being aware about their
concrete implementation.

## Applicability & Examples

Probably the factory pattern is one of the most used patterns.

For example a graphical application works with shapes. In our implementation the drawing
framework is the client and the shapes are the products. All the shapes are derived from an
abstract shape class(or interface). The Shape class defines the draw and move operation which
must be implemented by the concrete shapes. Let's assume a command is selected from the menu
to create a new Circle. The framework receives the shape type as a string parameter,it asks the
factory to create a new shape sendinf the parameter received from menu. The factory creates
a new circle and returs it to the abstract class without being aware of the concrete object type.

The advantage is obvisious: New shapes can be add without changing a single line of code in the
framework(the client code that uses the shapes from the factory). As it is shown in the next
sections, there are certain factory implementations that allow adding new products without even
modifying the factory class.

## Specific problems and implementation

### Procedural Solution - siwtch/case noob instantiation.

![factory noob implementation](/pictures/factory noob implementation.gif)

Those are also known as parameterized Factories. The generating method can be written so that
it can generate more types of Product objects, using a condition(entered as a method parameter
or read from some global configuration parameters - see abstract factory pattern) to identify the
type of the object that should be created, as below:

```Java
public class ProductFactory{
    public Product createProduct(String ProductID){
        if (id==ID1)
            return new OneProduct();
        if (id==ID2) return
            return new AnotherProduct();
        ... // so on for the other Ids
        return null; //if the id doesn't have any of the expected values
    }
    ...
}
```

This implementation is the most simple and intuitive (Let's call it noob implementation). The problem
here is that once we add a new concrete product call we should modify the Factory calss. It is not very
flexible and it violates open close principle. Ofcourse we can subclass the factory class,but we should
not forget that the factory class is usually used as a singleton. Subclassing it means replacing all the
factory class references everywhere through the code.

## Class Registration - using reflection

If you can use reflection, for example in Java or .NET languages, you can register new product classes
to the factory without even change the factory it self. For creating objects inside the factory calss
without knowing the object type we keep a map between the productID and the calss type of the product.
In this case when a new product is added to the application it has to be registered to the factory.
This operation doesn't require any change in the factory class code.

```Java
class ProductFacotry
{
    private HashMap m_RegisteredProducts = new HashMap();

    public void registerProduct(String productID,Class productClass)
    {
        m_RegisteredProducts.put(productID,productClass);
    }

    public Product createProduct(String productID)
    {
        Class productClass = (Class) m_RegisteredProducts.get(productID);
        Constructor productConstructor = productClass.getDeclaredConstructor(new Class[] { String.class });
        return (Product) productConstructor.newInstance(new Object[] { });
    }
}
```

We can put the registration code anyhwere in our code, but a convenient place is inside the product class in
a static constructor. Look at the example below:

1. Registration done outside of product classes:

```Java
public static void main(String args[])
{
    Factory.instance().registerProduct("ID1", OneProduct.class);
}
```

2. Registration done inside the product classes:

```Java
class OneProduct extends Product
{
    static {
        Factory.instance().registerProduct("ID1",OneProduct.class);
    }
    ...
}
```

We have to make sure that the concrete product classes are loaded before they are required by the factory
for registration(if they are not loaded they will not be registered in the factory and the createProduct
method will return null). To ensure it we are going to use the Class.forName method right in the static
section of the main class. This section is executed right after the main class is loaded. Class.forName
is supposed to return a Class instance of the indicated class. If the class is not loaded by the compiler
yet,it will be loaded when the Class.forName is invoked. Consequently the static block in each class will
be executed when each class is loaded:

```Java
class Main
{
    static
    {
        try
        {
            Class.forName("OneProduct");
            Class.forName("AnotherProduct");
        }
        catch (ClassNotFoundException any)
        {
            any.printStackTrace();
        }
    }
    public static void main(String args[]) throws PhoneCallNotRegisteredException
    {
        ...
    }
}
```

This reflection implementation has its own drawback. The main one is performance. When the reflection is
used the performance on code involving reflection can decrease even to 10% of the performance of a non
reflection code. Another issue is that not all the programming languages provide reflection mechanism.

## Class Registration - avoiding reflection

As we saw in the previous paragraph the factory object uses internally a HashMap to keep the mapping
between parameters(in our case String) and concrete products class. The registration is made from
outside of the factory and because the objects are created using reflection the factory is not aware of
the objects types.

We don't want to use reflection but in the same time we want to have a reduce coupling between the
factory and concrete products. Since the factory should be unware of products we have to move the creation
of objects outside of the factory to an object arare of the concrete product class. That would be the
concrete class it self.

We add a new abscract method in the product abstract class. Each concrete class will implement this method
to create a new object of the same type as itself. We also have to change the registration method so that
we'll register concrete product objects instead of Class objects.

```Java
abstract class Product
{
    private product;
    public abstract Product createProduct();

    public void first()
    {
        // Product class aware of the actual type of this product
        // only the classes that implements createProduct() method knows
        product = createProduct();
    }
    ...
}

class OneProduct extends Product
{
    public OneProduct createProduct()
    {
        return new OneProduct();
    }
    ...
}

class AnotherProduct extends Product
{
    public AnotherProduct createProduct()
    {
        return new AnotherProduct();
    }
    ...
}
```

## A more advanced solution - Factory design pattern with abstractions(Factory Method)

![factory design pattern with abstractions](/pictures/factory design pattern with abstractions.gif)

This implementation represents an alternative for the class registration implementation. Let's assume
we need to add a new prodcut to the application. For the prcedural switch-case implementation we need
to change the Factory class, while in the class registration implementation all we need is to register
the class to the factory without actually modifying the factory class. For sure this is a flexible solution.

The procedural implementation is the classical bad example for the [Open-Close Principle](). As we can
see there the most intuitive solution to avoid modifying the Factory class is to extend it.

This is the classic implementation of the factory method pattern. There are some drawbacks over the
class registration implementation and not so many advantages:

* The derived factory method can be changed to perform additional operations when the objects are
created(maybe some initialization based on some global parameters ...)

* The factory can not be used as a singleton

* Each factory has to be initialized before using it.

* More difficult to implement.

* If a new Object has to be added a new factory has to be created.

Anyway, this classic implementation has the advantage that it will help us understanding the Abstract
Factory design pattern.

## Conclusion

When you design an application just think if you really need it a factory to create objects. Maybe using it
will bring unnessary complexity in your application. If you have many objects of the same base type and you
manipulate them mostly casted to abstract types, then you need a factory. If you have a lot code like the
following, you should reconsider it:

```java
(if (genericProduct typeof ConcreteProduct)
    ((ConcreteProduct)genericProduct).doSomeConcreteOperation();
```

Please note that the procedural switch-case(noob) implementation is simplest, The only wise way to use it is for temporary modules until it is replaced with an real factory. Is it means that we should use factory to create
objects only? I don't think so, if we do that our code will become very complicated. Only if we know the object
we created maybe changed in future then we use factory pattern.