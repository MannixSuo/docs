---
title: Composite Pattern
layout: posts
tags: designPattern
---

## Motivation

There are times when a program needs to manipulate a tree data structure and it is necessary to treat both Branches as well as Leaf Nodes uniformly. Consider for example a program that manipulates a file system. A file system is a tree structure that contains Branches which are Folders as well as Leaf nodes which are files. Note that a folder object uaually contains one or more file or folder objects and thus is a complex object where a file is a simple object. Note also that since files and folders have many operations and attributes in common, such as moving and copying a file or a folder, listing file or folder attributes such as file name and size, it would be easier and more convenient to treat both file and folder objects uniformly by defining a File System Resource Interface.

## Intent

* The intent of this pattern is to compose objects into tree structures to represent part-whole hierarchies.

* Composite lets clients treat individual objects and compositions of objects uniformly.

## Implementation

The figure below shows a UML class diagram for the Composite Pattern:

![composite pattern](/pictures/composite-design-pattern-implementation-uml-class-diagram.png)

* **Component** - Component is the abstraction for leafs and composites. It defines the interface that must be implemented by the objects in the composition. For example a file system resource defines move, copy, rename, and getSize methods for files and folders.

* **Leaf** - Leafs are objects that have no children. They implementing method by the Component interface. For example a file objects implements move, copy, rename, as well as getSize mthods which are related to the Component interface.

* **Composite** - A Composite stores child components in addition to implementing methods defined by the component interface. Composites implement methods defined in the Component interface by delegating to child components. In addition composites pprovide additional methods for adding, removing, as well as geting components.

* **Client** - The client manipulate objects in the hierarchy using the componment interface.

A client has a reference to a tree data structure and needs to perform operations on all nodes independent of the fact that a node might be a branch or a leaf. The client simply obtains reference to the required node using the component interface, and deals with the node using this interface; it doesn't matter if the node is a composite or a leaf.

## Applicability & Examples

The composite pattern applies when there is a part-whole hierarchy of objects and a client needs to deal with objects uniformly regardless of the fact that an object might be a leaf or a branch.

### Example - Graphics Drawing Editor

In graphics editors a shape can be basic or complex. An example of simple shape is a line, where a complex shape is a rectangle which is made of four lines objects. Since shapes have many operations in common such as rendering the shape to screen, and since shapes follow a part-whole hierarchy, composite pattern can be used to enable the program to deal with all shapes uniformly (一致的).

In the example we can see the following actors:

* **Shape(Component)** - Shape is the abstraction for Lines, Rectangles(leafs) and ComplexShapes (Composites).

* **Line, Rectangle(Leafs)** - objects that have no children. They implement services described by Shape interface.

* **ComplexShape(Composite)** - A Composite stores child Shapes in addition to implementing methods defined by the Shape interface.

* **GraphicsEditor(Client)** - The GraphicsEditor manipulates Shapes in the hierarchy.

```java

/**
 * 
 * Shape is the Component interface
 *
 */
public interface Shape {
    
    /**
     * Draw shape on screen 
     * 
     * Method that must be implemented by Basic as well as 
     * complex shapes 
     */
    public void renderShapeToScreen();
    
    /**
     * Making a complex shape explode results in getting a list of the 
     * shapes forming this shape 
     * 
     *  For example if a rectangle explodes it results in 4 line objects 
     *  
     * Making a simple shape explode results in returning the shape itself 
     */
    public Shape[] explodeShape();
    
}

package composite;

/**
 * 
 * Line is a basic shape that does not support adding shapes  
 */
public class Line implements Shape {
    /**
     * Create a line between point1 and point2
     * @param point1X
     * @param point1Y
     * @param point2X
     * @param point2Y
     */
    public Line(int point1X, int point1Y, int point2X, int point2Y) {
    }
    @Override
    public Shape[] explodeShape() {

        // making a simple shape explode would return only the shape itself, there are no parts of this shape
        Shape[] shapeParts = {this};
        return shapeParts;
    }

    /**
     * this method must be implemented in this simple shape
     */
    public void renderShapeToScreen() {
        // logic to render this shape to screen
    }
}

/**
 * Rectangle is a composite 
 *
 *Complex Shape
 */
public class Rectangle implements Shape{

    // List of shapes forming the rectangle
    // rectangle is centered around origin
    Shape[] rectangleEdges = {new Line(-1,-1,1,-1),new Line(-1,1,1,1),new Line(-1,-1,-1,1),new Line(1,-1,1,1)};

    @Override
    public Shape[] explodeShape() {
        return rectangleEdges;
    }

    /**
     * this method is implemented directly in basic shapes 
     * in complex shapes this method is implemented using delegation
     */
    public void renderShapeToScreen() {
        for(Shape s : rectangleEdges){
               // delegate to child objects
            s.renderShapeToScreen();
        }
    }

}


/**
 * Composite object supporting creation of more complex shapes
 *  Complex Shape
 */
public class ComplexShape implements Shape {

    /**
     * List of shapes 
     */
    List shapeList = new ArrayList();

    /**
     * 
     */
    public void addToShape(Shape shapeToAddToCurrentShape) {
        shapeList.add(shapeToAddToCurrentShape);
    }

    public Shape[] explodeShape() {
        return (Shape[]) shapeList.toArray();
    }

    /**
     * this method is implemented directly in basic shapes 
     * in complex shapes this method is handled with delegation
     */
    public void renderShapeToScreen() {

        for(Shape s: shapeList){
            // use delegation to handle this method
            s.renderShapeToScreen();
        }
    }
}

/**
 * Driver Class
 *
 */
public class GraphicsEditor {

    public static void main(String[] args) {
        List allShapesInSoftware = new ArrayList();
        // create a line shape
        Shape lineShape = new Line(0,0,1,1);
        // add it to the shapes 
        allShapesInSoftware.add(lineShape);
        // create a rectangle shape
        Shape rectangelShape = new Rectangle();
        // add it to shapes 
        allShapesInSoftware.add(rectangelShape);
        // create a complex shape 
        // note that we have dealt with the complex shape 
        // not with shape interface because we want 
        // to do a specific operation 
        // that does not apply to all shapes 
        ComplexShape complexShape = new ComplexShape();
        // complex shape is made of the rectangle and the line
        complexShape.addToShape(rectangelShape);
        complexShape.addToShape(lineShape);
        // add to shapes
        allShapesInSoftware.add(complexShape);
        // create a more complex shape which is made of the 
        // previously created complex shape 
        // as well as a line 
        ComplexShape veryComplexShape = new  ComplexShape();
        veryComplexShape.addToShape(complexShape);
        veryComplexShape.addToShape(lineShape);
        allShapesInSoftware.add(veryComplexShape);
        renderGraphics(allShapesInSoftware);
        // you can explode any object
        // despite the fact that the shape might be 
        // simple or complex
        Shape[] arrayOfShapes = allShapesInSoftware.get(0).explodeShape();
    }
    private static void renderGraphics(List shapesToRender){
        // note that despite the fact there are 
        // simple and complex shapes 
        // this method deals with them uniformly 
        // and using the Shape interface
        for(Shape s : shapesToRender){
            s.renderShapeToScreen();
        }
    }
}
```

**Alternative Implementation :** Note that in the previous example there were times when we have avoided dealing with composite objects through the Shape interface and we have specifically dealt with them as composites (when using the method addToShape()). To avoid such situations and to further increase uniformity one can add methods to add, remove, as well as get child components to the Shape interface. The UML diagram below shows it:

![alternative implementation](/pictures/composite-design-pattern-alternative-implementation-uml-class-diagram.png)

```java
/**
 * 
 * Shape is the Component interface
 *
 */
public interface Shape {
    /**
    * Draw shape on screen 
    * 
    * Method that must be implemented by Basic as well as 
    * complex shapes 
    */
    public void renderShapeToScreen();
    /**
    * Making a complex shape explode results in getting a list of the 
    * shapes forming this shape 
    * 
    *  For example if a rectangle explodes it results in 4 line objects 
    *  
    * Making a simple shape explode results in returning the shape itself 
    */
    public Shape[] explodeShape();

    /**
    * 
    * Although this method applies to composites only 
    * it has been added to interface to enhance uniformity 
    *  
    * @param shape
    */
    public void addToShape(Shape shape);
}

/**
 * 
 * Line is a basic shape that does not support adding shapes  
 */
public class Line implements Shape {

// same as previous implementation


    /**
    * Implementation of the add to shape method
    * @param shape
    */
    public void addToShape(Shape shape){
        throw new RuntimeException("Cannot add a shape to simple shapes ...");
    }
}
```

## Specific problems and implementation

Graphics Editors use composite pattern to implement complex and simple graphics as previously explained.

File System Implemtntations use the composite design pattern as described previously.

### Consequences

* The composite pattern defines class hierarchies consisting of primitive objects and composite objects. Primitive objects can be composed into more complex objects, which in turn can be composed.

* Clients treat primitive and composite objects uniformly through a component interface which makes client code simple.

* Adding new components can be easy and client code does not need to be changed since client deals whith the new components through the component interface.

### Known Uses

File System Implementation.

Graphics Editors.

