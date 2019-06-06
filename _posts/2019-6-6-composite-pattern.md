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

The composite pattern applies when there is a part-whole hierarchy
