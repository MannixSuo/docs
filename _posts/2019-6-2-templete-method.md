---
title: Templete Method
layout: posts
tags: designPattern
---
## Motivation

If we take a look at the dictionary definition of a template we can see that a template is apresent format,used as a staring point for a particular application so that the format does not have to recreated each time it is used.

On the same idea is the template method is based. A template method defines an algorithm in a base class using abstract operations that subclasses override to provide concrete behavior.

## Intent

* Define the seleton(骨架) of an algorithm in an operation, deferring some steps to subclasses.

* Template Method lets subclasses redefine certain steps of an algorithm without letting them to change the algorithm's structure.

![template method](/pictures/pattern/template_method_implementation_-_uml_class_diagram.gif)

## Implementation

1. **AbstractClass**

     1. Define abstract primitive operations that concrete subclasses define to implement step of an algorithm.

     2. Implements a template method which defines the skeleton of an algorithm. The template method calls primitive operations as well as operations defined in AbstraceClass or those of other objects.

2. **ConcreteClass**

    1. Implements the primitive operations to carry out subclass-specific steps of an algorithm. When a concrete class is called the template method code will be executed from the base class while for each method use inside the template method will be called the implementation from the derived class.

## Applicability & Examples

The tempalte Method pattern should be used:

1. To implement the invariant parts of an algorithm once and leave it up to subclasses to implement the behavior that can vary.

2. When refactoring is performed and common behavior is identified among classes. A abstract base class containing all the common code(in the template method) should be created to avoid code duplication.

## Example - Application used by a travel agency.

![travel agency](/pictures/pattern/template_method_example_trips_-_uml_class_diagram.gif)

Let's assume we have to develop an application for a travel agency. The travel agency is managing each trip. All the trips contain common behavior but there are serveral packages. For example each trip contains the basic steps:

* The toutist are transported to the holiday location by plane/train/ships...

* Each day they are visiting something

* They are returning back home.

So we create an abstract class containing each step as an abstract method and one concrete final method that calls all the abstract methods. Then we create one superclass for each package:

```java
public class Trip {
        public final void performTrip(){
                 doComingTransport();
                 doDayA();
                 doDayB();
                 doDayC();
                 doReturningTransport
        }
        public abstract void doComingTransport();
        public abstract void doDayA();
        public abstract void doDayB();
        public abstract void doDayC();
        public abstract void doReturningTransport();
}

public class PackageA extends Trip {
        public void doComingTransport() {
                 System.out.println("The turists are comming by air ...");
        }
        public void doDayA() {
                 System.out.println("The turists are visiting the aquarium ...");
        }
        public void doDayB() {
                 System.out.println("The turists are going to the beach ...");
        }
        public void doDayC() {
                 System.out.println("The turists are going to mountains ...");
        }
        public void doReturningTransport() {
                 System.out.println("The turists are going home by air ...");
        }
}
public class PackageB extends Trip {
        public void doComingTransport() {
                 System.out.println("The turists are comming by train ...");
        }
        public void doDayA() {
                 System.out.println("The turists are visiting the mountain ...");
        }
        public void doDayB() {
                 System.out.println("The turists are going to the beach ...");
        }
        public void doDayC() {
                 System.out.println("The turists are going to zoo ...");
        }
        public void doReturningTransport() {
                 System.out.println("The turists are going home by train ...");
        }
}
```

## Specific problems and implementation

### Concrete base class

It is not necessary to have the superclass as an abstract class. It can be a concrete class containing a method(template method) and some default functionality. In this case the primitive methods can not be abstract and this is a flaw because it is not so clear which methods have to be override and which not. A concrete base class should be used only when customizations hooks are implemented.

### Template method can not be override

The template method implemented by the base class should not be overriden. The specific programing language modifiers should be used to ensure this.

### Customization Hooks

A particular case of the template method pattern is represented by the hooks. The hooks are generally empty method that are called in superclass(and does nothing because are empty), but can be implemented in subclasses. Customization Hooks can be considered a particular case of the template method as well as a totally different mechanism. Usually a subclass can have a method extended by overriding and calling the parent method explicitly:

```java
class Subclass extends Superclass
{
    ...
    void something() {
    // some customization code to extend functionality
    super. something ();
    // some customization code to extend functionality
    }
}
```

Unfortunately it is easy to forget to call the super and this is forcing the developer to check the existing code from the method in Superclass.

Instead of overriding some hook methods can be added. Then in the subclasses only the hooks should be implemented without being aware of the method something:

```java
class Superclass
{
    ...
    protected void preSomethingHook(){}
    protected void postSomethingHook(){}
    void something() {
        preSomethingHook();
        // something implementation
        postSomethingHook();
    }
}
class Subclass extends Superclass
{
    protected void preSomethingHook()
    {
        // customization code
    }
    protected void postSomethingHook()
    {
        // customization code
    }
}
```

### Minimizing primitive methods number

It's important to identify the primitive methods is it better to use a specific naming convention. For example the prefix "do" can be used for primitive methods. In a similar way the customizations hooks can have prefixes like "pre" and "post".

### When methods taht should be abstract or not

When there is a method in the base class that should contain some default code, but on the other side it's necessary to be extended in the subclasses it should be split in 2 methods: one abstract and one concrete. We can not rely in the fact that the subclasses will override the method and will call the super implementation in it like this :

```java
void something() {
    super. something ();
    // extending the method
}
```

### Template Method and Strategy Pattern

The Strategy Pattern is with Template Method Pattern. The difference consists in the fact that Strategy uses delegation while the Template Methods uses the inheritance.

## Hot Points

Template method is using as an inverted controls structure, sometimes referred as "the Hollywood principle": from the superclass point of view: "Don't call us, we'll call you". This refers to the fact that instead of calling the methods from base class, the methods from subclass are called in the template method from superclass.
Due to the above fact a special care should be paid to access modifiers: the template method should be implemented only in the base class, and the primitive method should be implemented in the subclasses. A particular case of the template method is represented by the customization hooks.
