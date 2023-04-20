+++ 
author = "Ilya Pantsyr" 
title = "Class Loaders in JVM: An Overview" 
date = "2023-04-20" 
tags = [ "java", "jvm", ] 
+++

Class loaders are an essential part of the Java Virtual Machine (JVM), but many developers consider them to be mysterious. This article aims to demystify the subject by providing a basic understanding of how class loading works in the JVM.

## What are classloaders

In the Java Virtual Machine (JVM), classes are loaded dynamically and found through a process called class loading. Class loading is the process of loading a class from its binary representation (usually a .class file) into memory so that it can be executed by the JVM. This is where we need classloaders. Classloaders are used to load .class file into the memory.

## How classes are loaded in JVM

Classes are loaded in 3 steps:

1. Creation and Loading step.
   First thing that happens is loading a class file using a class loader. There are two kinds of class loaders: the bootstrap class loader supplied by the JVM, and user-defined class loaders. (More details about class loaders are in the next chapter)
   Then, instance of `java.lang.Class` class is created. What makes the class available to the JVM for further execution [A detailed step-by-step algorithm can be found in the Java Virtual Machine specification](https://docs.oracle.com/javase/specs/jvms/se20/html/jvms-5.html#jvms-5.2)
2. Linking step
   Before the class is ready for execution, the JVM needs to perform a number of preparatory operations, which include verification and preparation of the class for execution.
   Linking steps are following:
   1. Bytecode verification.
      Verification ensures that the binary representation of a class or interface is structurally correct and is not corrupted, otherwise the class file will not be linked and a VerifyError error will be thrown. Verification can be turned off by the `-noverify` option. Turning off the verification can speed up the startup of the JVM, but disabling bytecode verification undermines Java's safety and security guarantees. [Why not to disable bytecode verification](https://wiki.sei.cmu.edu/confluence/display/java/ENV04-J.+Do+not+disable+bytecode+verification)
   2. Preparation.
      Allocate RAM for static fields and initialize them with default values.
   3. Resolution of symbolic links.
      Since all references to fields, methods, and other classes are symbolic. JVM, in order to execute the class, you need to translate the references into internal representations.
3. Initialization step.
   After a class is successfully loaded and linked, it can be initialized. At this stage, the static class initializers or static variable initializers are called, which ensures that static initialization block is executed only once and static variables are initialized correctly.

Also, it is worth remembering that Java implements delayed (or lazy) loading of classes. This means that class loading of reference fields of the loaded class will not be performed until the application explicitly refers to them. In other words, character reference resolution is optional and does not happen by default.

## Class loader features

Class loaders has three important features that is worth to remember.

- Delegation model
  When requested to find a class or resource, a class loader will delegate the search for the class or resource to its parent class loader before attempting to find the class or resource itself.
- Visibility
  Classes loaded by a parent class loader are visible to its child class loaders, but classes loaded by a child class loader are not visible to its parent class loaders or children.
- Uniqueness
  In Java a class is uniquely identified using `ClassLoader + Class` as the same class may be loaded by two different class loaders.
  `Class A loaded by ClassLoader A != Class A loaded by ClassLoader B`
  It is helpful for defining different protection and access policies for different classloaders.

## Class loaders relationship

We should remember that classes in Java are loaded on demand. That is, class is loaded only when is requested to.

As you know, the entry point of every program written in Java is `public static void main(String[] args)` method. Main method is the place where the very first class is loaded. All the subsequently loaded classes are loaded by the classes, which are already loaded and running.

When a class is requested by the running program, the System class loader searches for it in the application classpath. If the class is not found, the Platform class loader is searched, and if still not found, the Bootstrap class loader is searched.

If the requested class is found in a parent class loader, it is loaded by that class loader. If not, the System class loader loads the class. If the class has not been loaded before, the class loader loads it into memory and creates a new instance of the Class object that represents the loaded class.

It is important to note that the class loading hierarchy is hierarchical in nature, with each class loader having a parent class loader. This parent-child relationship ensures that each class loader is responsible for loading only its own classes and delegates the loading of parent classes to its parent class loader.

## Different types of classloaders

The class loading mechanist in JVM doesn't use only one class loader. Every Java program has at least three class loaders:

- Bootstrap (Primordial) Class Loader
  This is the root class loader and is responsible for loading core Java classes such as java.lang.Object and other classes in the Java standard library (also known as the Java Runtime Environment or JRE). It is implemented in native code and is part of the JVM itself. Althrough each class loader has its own `ClassLoader` object, there is not such object corresponding to the Bootstrap Class Loader. For example, if you would run this line of code
  `String.class.getClassLoader()`, you would get `null`.
- Extension Class Loader
  This class loader is responsible for loading classes from the extension directories (such as the jre/lib/ext directory in the JRE installation) and is child of Bootstrap Class Loader. You can also specify the locations of the extension directories via the `java.ext.dirs` system property.
- System (Application) class loader
  This is the class loader that loads application-specific classes, usually from the classpath specified when running the Java application. The classpath can include directories, JAR files, and other resources. The classpath can be set using the CLASSPATH environment variable, the -classpath or -cp command-line option. The System/Application Class Loader is also implemented in Java and is a child of the Extension Class Loader.

## Class loaders and related to them changes over time

In the previous section, we've seen class loaders hierarchy that was in Java until Java 9 revised that.

New class loaders hierarchy since Java 9 looks like this:

- Bootstrap (Primordial) Class Loader
  This is the root class loader and is responsible for loading core Java classes such as `java.lang.Object` and other classes in the Java standard library (also known as the Java Runtime Environment or JRE). It is implemented in native code and is part of the JVM itself. Althrough each class loader has its own `ClassLoader` object, there is not such object corresponding to the Bootstrap class loader and, typically represented as `null`, and doesn't have a parent. For example, if you would run this line of code
  `String.class.getClassLoader()`, you would get `null`.
- Platform class loader (former Extension class loader)
  All classes in the Java SE Platform are guaranteed to be visible though the Platform class loader. Just because a class is visible through the platform class loader does not mean the class is actually defined by the platform class loader. Some classes in the Java SE Platform are defined by the platform class loader while others are defined by the bootstrap class loader. Applications should not depend on which class loader defines which platform class.
- System (Application) class loader
  This is the class loader that loads application-specific classes, usually from the classpath specified when running the Java application. The classpath can include directories, JAR files, and other resources. The classpath can be set using the CLASSPATH environment variable, the -classpath or -cp command-line option. The System/Application Class Loader is also implemented in Java and is a child of the Extension Class Loader.

There are even more changes that were introduced in Java 9 which are related to class loaders, namely:

- The Application class loader is no longer an instance of `URLClassLoader` but, rather, of an internal class. Now, there are `ClassLoaders` class that contains in itself implementation of 3 built-in class loaders. Such as:
  1. BootClassLoader
  2. PlatformClassLoader
  3. AppClassLoader
     - However, bootstrap class loader should be used via `BootLoader` class and not via `ClassLoaders` class.
- The Extension class loader has been renamed to Platform class loader.
  Substantial difference between Java 8 Extension class loader and Java 9 Platform class loader is that Platform class loader is no longer instance of `URLClassLoader`.
  But for the most part, the platform class loader is the equivalent of what used to known as the Extension class loader. One motivation for renaming it, is that extension mechanism has been removed, which we will discuss in the next paragraph.
- Removed Extension Mechanism
  In releases before Java 9, the extension mechanism allowed the runtime environment to find and load extension classes without explicitly mentioning them on the classpath. However, in JDK 9, this mechanism has been removed. To use extension classes, ensure that their JAR files are included in the classpath.
- Removed rt.jar and tools.jar
  - `rt.jar` contains all of the compiled class files for the base Java Runtime environment.
  - `tools.jar` contains all tools that are needed by a JDK but not a JRE (javac, javadoc, javap).

## Resources

https://docs.oracle.com/javase/specs/jvms/se20/html/jvms-5.html
https://www.artima.com/insidejvm/ed2/securityP.html
https://blogs.oracle.com/javamagazine/post/how-the-jvm-locates-loads-and-runs-libraries
https://docs.oracle.com/javase/9/docs/api/java/lang/ClassLoader.html

#### Big thanks to Erik Pragt (https://twitter.com/epragt) for reviewing and guiding me
