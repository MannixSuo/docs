---
title: Observer Pattern
layout: posts
tags: designPattern
---

## Motivation

We can not talk about Object Oriented Programming without
considering the state of the objects. After all(毕竟) object
oriented programing is about objects and their interaction.
The cases when certain objects need to be informed about
the changes occured in other objects are frequent. To have a
good design means to decouple as much as possible and to reduce
the dependencies. The Observer Design Pattern can be used whenever
a subject has to be observed by one or more observers.

Let's assume we have a stock system which providers data for several
types of client. We want to have a client implemented as a web based
application but near future we need to add clients for mobile devices,
Palm(palm PC 掌上电脑) or Pocket PC, or to have a system to notify
the user with sms alerts. Now it's simple to see what we need from
the observer pattern: we need to separete the subject(socket server)
from it's observer(client applications) in such a way that adding
new observer will be transparent for the server.

## Intent

Defines a one-to-many dependency between objects so that when one
object changes state, all its dependents are notified and
updated automatically.

![observer_implementation]("observer_implementation_-_uml_class_diagram.gif")

Implementation

The participants classed in this pattern are:

> **Observable** - interface defining the operations for attaching 
> and de-attaching observers to the client. In the GOF book this
> interface is known as **Subject**.
>
> **ConcreteObservable** - concrete Observable class. It
> maintain the state of the object and when a change in the
> state occurs it notifies the attached **Observers**.
>
> **Observer** - interface drfining the operations to be used
> to notify this object.
>
> **ConcreteObserverA**,**ConcreteObserverB** - concrete Observer
> implementations.

The flow is simple : the main framework instantiate the
ConcreteObservable object. Then it instantiate and attaches
the concrete observers to using the methods defined in the
Observable interface. Each time the state of the subject
changed it notifies all the attached Observers using the
methods defined in the Observer interface. When a new Observer
is added to the application, all we need to do is to instantiate
it in the main framework and attach it to the Observable object.
The classes already created will remain unchanged.

## Applicationbility & Examples

The observer pattern is used when:

> The change of a state in one object must be reflected in
> another object without keeping the objects tight coupled.
>
> The framework we are writing needs to be enhanced in future
> with new observers with minmal changes.

Some Classical Examples:

> Model View Controller Pattern - The observer pattern is used
> in the model view controller(MVC) architectural pattern. In
> MVC this pattern is used to decouple the model from the view.
> Views represents the Observer and model is the Obervable
> object.
>
> Event management - This is one of the domains where the Obser
> patterns is extensively used. Swing and .Net are extensively
> using the Observer pattern for implementing the events mechanism.