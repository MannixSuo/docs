---
title: Proxy Pattern
layout: posts
tags: designPattern
---

## Motivation

Sometimes we need the ability to control the access to an object. For example if we need to use only a few methods of some coostly objects we'll initialize those objects when we need them entirely. Until that point we can use some light objects exposing the same interface as the heavy objects. These light objects are called proxies and they will instantiate those heavy objects when they are really need and by then we'll use some light objects instead.

This ability to control the access to an object can be required for a variety of reasons: controling when a costly objects needs to be instantiated and initalized, giving different access rights to an object, as well as providing a sophisticated means of accessing and referencing objects running in other process, on other machines.

Consider for example an image viewer program. An image program must be able to list and display high resolution photo objects that are in folder, but how often do someone open a folder and view all the images inside. Sometimes you will be looking for a particular photo, sometimes you will only want to see  an image name. The image viewer must be able to list all photo obkects, but the photo objects must not be loaded into memory until they are required to be rendered.

## Intent

The intent of this pattern is to provide a `Plactholder` for an object to control references to it.

## Implementation

The figure below shows a UML class diagram for the Proxy Pattern:

![proxy pattern](/pictures/proxy-design-pattern-implementation-uml-class-diagram.png)

The participants classes in the proxy pattern are:

* **Subject** - interface implemented by the RealSubject and representing its services. The interface must be implemented by the proxy as well so that the proxy can be used in any location where the RealSubject can be used.

* **Proxy**

* * Maintains a reference that allows the Proxy to access the RealSubject.

* * Implements the same interface implented by the RealSubject so that the Proxy can be substitude for the RealSubject.

* * Controls access to the RealSubject and may be responsible for its creation and deletion

* * Other responsibilities depend on the kind of proxy.

* **RealSubject** - the real object that the proxy represents.

## Description

A client obtains a reference to a Proxy, the client then handles the proxy in the same way it handles RealSubject and thus invoking the method doSomething(). At that point the proxy can do different things prior to invoking RealSubject's doSomething() method. The client might create a RealSubject object at that point, perform initialization, check permissions of the client to invoke the method on the object. The client can also do additional tasks after invoking the doSomething() method, such as incrementing the number of references to the object.

## Applicability & Examples

The Proxy design pattern is applicable when there is a need to control access to an Object, as well as when there is a need for a sophisticated reference to an object. Common Situations where the proxy pattern is applicable are :

* **Virtual Proxies:** delaying the creation an initialization of expensive objects until needed, where the objects are created on demand (for example creating the RealSubject object only when the doSomething method is invoked).

* **Remote Proxies:** providing a local representation for an object that is in a different address space. A common example is Java RMI stub objects. The stub object acts as a proxy where invoking methods on the stub would cause the stub to communicate and invoke methods on a remote object (called skeleton) found on a different machine.

* **Protection Proxies:** Where a proxy controls access to RealSubject methods, by giving access to some objects while denying access to others.

* **Smart References:** Providing a sophisticated access to certain objects such as tracking the number of reference to an object and denying access if a certaion number is reached, as well as loading an object from database into memory on demand.ss

## Example - Virtual Proxy Example.

Consider an image viewer program that lists and displays high resolution photos. The program has to show a list of photos however it does not need to display the actual photo until the user selects an image item from a list.

![proxy](/pictures/proxy-design-pattern-image-example-uml-class-diagram.png)

The code below shows the Image interface representing the Subject. The interface has a single method showImage() that the concrete images must implement it to render an image to screen.

```java
/**
 * Subject Interface
 */
public interface Image {

	public void showImage();
	
}
```
The code below shows the Proxy implementation, the image proxy is a virtual proxy that creates and loads the actual image object on demand, thus saving the cost of loading an image into memory until it needs to be rendered:

```java
/**
 * Proxy
 */
public class ImageProxy implements Image {

	/**
	 * Private Proxy data 
	 */
	private String imageFilePath;
	
	/**
	 * Reference to RealSubject
	 */
	private Image proxifiedImage;
	
	
	public ImageProxy(String imageFilePath) {
		this.imageFilePath= imageFilePath;	
	}
	
	@Override
	public void showImage() {

		// create the Image Object only when the image is required to be shown
		
		proxifiedImage = new HighResolutionImage(imageFilePath);
		
		// now call showImage on realSubject
		proxifiedImage.showImage();
		
	}

}
```

The code below displays the RealSubject Implementation which is the concrete and heavyweight implementation of the image interface. The High resolution image loads a high resolution image from disk, and renders it to screen when showImage() is called:

```java

/**
 * RealSubject
 */
public class HighResolutionImage implements Image {

	public HighResolutionImage(String imageFilePath) {
		
		loadImage(imageFilePath);
	}

	private void loadImage(String imageFilePath) {

		// load Image from disk into memory
		// this is heavy and costly operation
	}

	@Override
	public void showImage() {

		// Actual Image rendering logic

	}

}

```

The code below illustrates a asmple image viewer program; the program simply loads three images, and renders only one image, once using the proxy pattern and another use directly. Note that when using the proxy pattern, although three images have been loaded, the high resolution image is not loaded into memory until it needs to be rendered, while in the part not using the proxy,three images are loaded into memory alough only one of them is actually rendered.

```java

package proxy;

/**
 * Image Viewer program
 */
public class ImageViewer {

	
	public static void main(String[] args) {
		
	// assuming that the user selects a folder that has 3 images	
	//create the 3 images 	
	Image highResolutionImage1 = new ImageProxy("sample/veryHighResPhoto1.jpeg");
	Image highResolutionImage2 = new ImageProxy("sample/veryHighResPhoto2.jpeg");
	Image highResolutionImage3 = new ImageProxy("sample/veryHighResPhoto3.jpeg");
	
	// assume that the user clicks on Image one item in a list
	// this would cause the program to call showImage() for that image only
	// note that in this case only image one was loaded into memory
	highResolutionImage1.showImage();
	
	// consider using the high resolution image object directly
	Image highResolutionImageNoProxy1 = new HighResolutionImage("sample/veryHighResPhoto1.jpeg");
	Image highResolutionImageNoProxy2 = new HighResolutionImage("sample/veryHighResPhoto2.jpeg");
	Image highResolutionImageBoProxy3 = new HighResolutionImage("sample/veryHighResPhoto3.jpeg");
	
	
	// assume that the user selects image two item from images list
	highResolutionImageNoProxy2.showImage();
	
	// note that in this case all images have been loaded into memory 
	// and not all have been actually displayed
	// this is a waste of memory resources
	
	}
		
}
```

## Specific problems and implementation

### Java Remote Method Invocation (RMI)

In Java RMI an object on one machine (executing in one JVM) called a client can invoke methods on am object in another machine (another JVM) the second object is called a remote object. The proxy (also called a stub) residers on the client machine and the client invokes the proxy in as if it is invoking the object itself (remember that the proxy implements the same interface that RealSubject implements). The proxy itself will handle communication to the remote object, invoke the method on that remote object, and would return the result if any to the client. The proxy in this case is a Remote proxy.s


## Related Patterns

**Decorator Design Pattern** - A decorator implementation can be the same as the proxy, however a decorator adds reponsibilities to an object while a proxy controls access to it.