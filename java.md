# Random Java notes
*Notes on the Java language in no particular order*

### [Class Loader](https://www.javaworld.com/article/2077260/learn-java/learn-java-the-basics-of-java-class-loaders.html)
The class loader, loads classes that is referenced in an already running class (special case being the first class, which is why there is a main method).

A class is loaded in two cases

1. When new bytecode is executed i.e. `Class class = new Class()`
2. When the bytecode makes a static reference to a class i.e. **System**.out

There is a predefined class loader in the JVM, called the **premoridal class loader** which is by default responsible for loading classes. There are however hooks in place so you can create user defined class loaders.

#### Static and Dynamic class loading
* Static class loading: This is done when the *new* operator is used. i.e. `Class instance = new Class()`. This is done at compile time

* Dynamic class loading: This technique is used when the class is not known at compile time. The class loader is invoked at run time to return an instance of a class with the given name.
