# JavaScript notes
*Random javascript knowledge that doesn't fit in anywhere else currently*

**Function declaration:**
```javascript
function doSomething(){
  // ...do something...
};
```

**Function Expression:**
```javascript
var doSomething = function(){
  // ...do something...
};
```
**Hoisting:** A javascript default behavior, where declarations are moved to the top of the scope. This means a variable can be declared after it has been used. Like this
```javascript
x = 5; // Assign 5 to x

elem = document.getElementById("demo"); // Find an element
elem.innerHTML = x;                     // Display x in the element

var x; // Declare x
```
Initializations are not hoisted, here `y` is undefined because only `var y` is moved to the top.
```JavaScript
var x = 5; // Initialize x

elem = document.getElementById("demo"); // Find an element
elem.innerHTML = x + " " + y;           // Display x and y

var y = 7; // Initialize y
```
Save yourself the trouble by always declaring and initializing before you use the variable.

[Source](https://www.w3schools.com/js/js_hoisting.asp)

**Immediately-Invoked Function Expression:**
A javascript function that runs as soon as it is defined that is used to prevent the rest of the program from accessing the variables with in it and not pollute the global name space as the function cannot be called by name. Using an IIFE also avoids any potential security implications.

An IIFE has the following syntax:
```javascript
(function() {}) ();
```
The closing parenthesis around `function() {}` means that the function is parsed as a function expression not a function declaration.
The last parenthesis's are used to immediately invoke the function (hence the name). This is the same as calling a `sayHi()` function except for `sayHi` it's an anonymous function `function() {}`.


[Source1](https://codeburst.io/javascript-what-the-heck-is-an-immediately-invoked-function-expression-a0ed32b66c18) ---
[Source2](https://developer.mozilla.org/en-US/docs/Glossary/IIFE)
