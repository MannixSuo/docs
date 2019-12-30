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

![observer_implementation](/pictures/pattern/observer_implementation_-_uml_class_diagram.gif)

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

## Specific Implementation Problems

### Many Subjects to Many observers

It's not a common situations but there are cases when many obervers that
need to observer more than one subject. In this case the observer need to
be notified not only about the change, but also which is the subject with
the state changed. This can be realized very simple by adding the subjects
reference in the update notification method. The subject will pass a reference
to itself to notify the observer.

### Who triggers the update?

The communication between the subject and its observers is done through the
notify method declared in observer interface. But who it can be triggered from
either subject or observer object. Usually the notify method is triggered by
the subject when it's state is changed. But sometimes when the updates are
frequent the consecutive changes in the subject will determin many unnessary
refresh operations in the observer. In order to make this process more efficient the observer can be made responsible for starting the notify operation when it consider nessary.

### Push and pull communication methods

There are 2 methods of passing the data from the subject to the observer when the state is being changed in the subject side:

> **Push Model** - The subjects send detailed information about the change to
> the observer whether it uses it or not. Because the subject needs to send
> detailed information to the observer this might be inefficient when a large
> amount of data needs to be send and it is not used. Another approach would
> be to send only the information required by the observer. In this case the
> subject should be able to distinguish between different types of observers
> and to know the required data of each of them, meaning that the subject
> layer is more coupled to observer layer.
>
> **Pull Model** - The subject notifies the observers when a change in his
> state appears and it's the responsibility of each observer to pull the
> required data from the subject. This canbe inefficient because the
> communication is done in 2 steps and problems might appear in multithreading
> environments.

### Encapsulating complex update semantics

When we have several subjects and observers the relations between them will become more complex. First of all are have a many to many relation, more difficult to manage directly. Second of all the relation between subjects and observers can contain some logic. Maybe we want to have an observer notified only when all the subjects will change their states. In this case we should introduce another object responsible (called ChangeManager) for the following actions:
> to maintain the many to many relations between the subjects and their
> observers.
>
> to encapsulate the logic of notify the observers.
>
> to receive the notifications from subjects and delegate them to the
> observers(based on the logic it encapsulate)

Basically the ChangeManager is an observer because if gets notified of the changes of the subject and at the same time it's an subject because it notify the observers. The ChangeManager is an implemenation of the Mediator pattern.

The Observer pattern is usually used in combination with other design patterns:
> **Factory pattern** - It's very likely to use the factory pattern to create
> the Observers so no changes will be required even in the main framework.
> The new observers can be added directly in the configuration files.
>
> **Template Method** - The observer pattern can be used in conjunction with
> the Template Method Pattern to make sure that Subject state is
> self-consistent before notification
>
> **Mediator Pattern** - The mediator pattern can be used when we have cases
> of complex cases of many subjects an many observers