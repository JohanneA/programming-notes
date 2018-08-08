# Random Python notes
*Notes on the python language in no particular order*

#### Magic methods
Magic methods are pre defined methods in python that can be used to make object more Pythonic. These include
* \__init__ : Initialize, kind of like a constructor in java
* \__iter__ : Make an object iterable
* \__next__ : Define how to iterate over the object

#### Lambda expressions
A Lambda expression is an anonymous function, that can be used whenever it doesn't make sense to define a separate method.

The syntax is `lamda <inputs>: <operations on input>`

Ex.
```Python
lambda x: x*x
```
Here we have a lambda expression that takes an input *x* and returns x multiplied by itself.

```Python
lambda x, y: x * y
```
Take two inputs and multiply them by one another.

Lambdas are often used in conjunction with:
* `map(function, iterable)`: Change the values of each element in the iterable as defined by the function
* `reduce(function, iterable)`: Reduce the iterable to one value as defined by the function.
* `filter(function, iterable)`: Filter elements from the elements of the iterable by applying the function to it. If it returns False the element is removed and if it returns True nothing happens.

List comprehensions are preferred, or any other type of comprehension.

#### Higher order functions
Functions that can take other functions as parameters. Makes it easier to deal with not having variables.
