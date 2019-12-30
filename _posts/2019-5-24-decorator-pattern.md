---
title: Decorator Pattern
tags: designPattern
layout: posts
---

## Motivation

Extending an object's functionality can be done statically(at
compile time) by using inheritance however it might be nessary
to extend an object's functionality dynamically(at runtime)
as an object is used.

Consider the typical example of a graphical window. To extend
the functionality of the graphical window for example by adding
frame to the window, would require extending the window class
to create a FramedWindow calss. To create a framed window it is
nessary to create an object of the FramedWindow class. However it
would be impossible to start with a plain window and to extend
its functionality at runtime become a framed window.

## Intent

The intent of this pattern is to add additional responsibilities
dynamically to an object.

## Implementation

The figure below shows a UML class diagram for the Decorator
Pattern:

![decorator pattern implementation](/pictures/pattern/decorator-design-pattern-implementation-uml-class-diagram.png)

The participants classes in the decorator pattern are:

**Componment** - Interface for objects that can have responsibilities added to them dynamically.

**ConcreteComponent** - Define an object to which additional responsibilities can be added.

**Decorator** - Maintains a reference to a Component object and defines an interface that conforms to Component's interface.

**ConcreteDecorators** - Concrete Decorators extend the functionality of the component by adding state or adding behavior.

## Description

The decorator pattern applies when there is a need to dynamically add as well as remove responsibilities to a
class, and then subclassing would be impossible due to the large
number of responsilities that can add and mixed togther.

## Applicability & Examples

Example - Extending capabilities of a Graphical Window at runtime.

![example](/pictures/pattern/decorator-design-pattern-example-uml-class-diagram.png)

In Graphic User Interface toolkit windows behaviors can be add
dynamically by using decorator design pattern.

## Code Example

```Java
interface Window
{
    void renderWindow();
}

class SimpleWindow implements Window
{
    public void renderWindow()
    {
    // implementation
    }
}

class DecoratedWindow implements Window
{
    private Window privateWindowRefernce = null;

    public DecoratedWindow(Window window)
    {
    this.privateWindowRefernce = window;
    }

    public void renderWindow()
    {
    privateWindowRefernce.renderWindow();
    }
}

class ScrollableWindow extends DecoratedWindow
{
    // additional state
    private Object scrollBarObject = null;

    public ScrollableWindow(Window window)
    {
    super(window);
    }

    @Override
    public void renderWindow()
    {
    // render scroll bar
    renderScrollBarObject();
    // render decorated window
    super.renderWindow();
    }

    private void renderScrolBarObject()
    {
    // prepare scroll bar
    scrollBarObject = new Object();
    // render scrollbar
    }
}
```

The code below illustrates a driver program for the window component. Note how the scrolling functionality has been added
dynamically at runtime.

```Java
public class GUIDriver {

    public static void main(String[] args) 
    {
    // create a new window
    Window window = new ConcreteWindow();
    window.renderWindow();
    // at some point later
    // maybe text size becomes larger than the window
    // thus the scrolling behavior must be added
    // decorate old window
    window = new ScrollableWindow(window);
    //  now window object
    // has additional behavior / state
    window.renderWindow();
    }
}
```

## Specific problems and implementation

### Graphical User Interface Frameworks

GUI toolkits use decoration pattern to add functionalities dynamically as explained before.

## Related Patterns

**Adapter Pattern** - A decorator is different from an adapter in that a decorator changes object's responsibilities, while an adapter changes an object interface.

**Composite Pattern** - A decorator can be viewed as degenerate composite with only one component. However, a decorator adds additional responsibilities.

## Consequences

Decoration is more convenient for adding functionalities to objects instead of entire classes at runtime. With decoration it is also possible to remove the added functionalities dynamically.

Decoration adds functionality to objects at runtime which would make debugging system functionality harder.

## Known Uses

GUI toolkits as has been previously explained.

Java IO library.