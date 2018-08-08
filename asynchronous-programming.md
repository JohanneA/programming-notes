Asynchronous programming is done when functions dealing with I/O do not block and wait for the IO to finish before continuing. So functions usually do not return any values as these could be
undefined when the program is run.

## [Understanding Async Programming in Node.js](https://blog.risingstack.com/node-hero-async-programming-in-node-js/)
#### By Gergely Nemeth

Asynchronous programming can be achieved with higher order functions (functions
that can be passed around like variables). In node, when dealing with asynchronous
functions they all have a *callback* function, which is a function that is run when the I/O is done.

Error first callback functions are at the heart of Node.js. This means checking
for an error as the first thing in the callback function.
```javascript
fs.readFile('file.md', 'utf-8', function callback (err, content){
  if error throw error;
  console.log(content);
});
```
So callbacks usually have two arguments, *error* and *result*. These will contain
the returned value from the IO. If it ends in error, the *error* argument is
filled with the error info and if the IO is successful then the result is stored in
result or content or whatever you choose to call it.

As callbacks are the only way to access the result value from an asynchronous function
you potentially have to do more asynchronous programming inside the callback function.
Doing this can lead to what's called *callback hell*; having asynchronous calls nested
inside callback functions.

To solve this, use async.js or Promises to void nesting asynchronous calls.

## [Asynchronous programming (background)](http://exploringjs.com/es6/ch_async.html#ch_async)

There are two common patterns for receiving results asynchronously, *events* and *callbacks*

* **Asynchronous results via events**

``` javascript
var req = new XMLHttpRequest();
req.open('GET', url);

req.onload = function () { //For success
    if (req.status == 200) {
        processData(req.response);
    } else {
        console.log('ERROR', req.statusText);
    }
};

req.onerror = function () { // For failure
    console.log('Network Error');
};

req.send();
```
  An object is created for each request and event handlers are registered with the
  object. One for successful computation and another for handling errors. This approach is
  okay if you receive results multiple times, for a single result it is a bit too verbose.
  Hence the use of callbacks.

* **Asynchronous results via callbacks**
``` javascript
fs.readFile('myfile.txt', { encoding: 'utf8' },
    function (error, result) { // (A)
        if (error) {
            // ...
        }
        console.log(result);
    });
```
With callbacks you pass a callback function as a parameter to an asynchronous method. This also
has parameters for success or failure. If an error happens the error is passed to the first parameter,
and if successful then the result of the asynchronous function is passed into the result parameter.
This style of programming is called *continuation-passing style* (CPS) because the next step is explicitly
passed as a parameter. This gives the invoked function more control as the control flow continues within the callback
function.
However error handling becomes more difficult with callbacks as you have to deal with errors from both
callbacks and exceptions. CPS all so makes the code harder to understand, the separation between input and output
is no longer clear. Composition is more complicated.

## [Promises for asynchronous programming](http://exploringjs.com/es6/ch_promises.html)
Promises are an alternative to callbacks, they require a bit more effort but provide several benefits.
A promise is both a container for a value and an event emitter.

Returning a result with Promises
```javascript
function asyncFunc() {
    return new Promise(
        function (resolve, reject) {
            ···
            resolve(result);
            ···
            reject(error);
        });
}
```
and you can control what should happen after the asynchronous call finishes
```javascript
asyncFunc()
.then(result => { ··· })
.catch(error => { ··· });
```
`.then()` runs sequentially
`Promise.all()` allows you to run asynchronous functions in parallel and be notified
when all are done.

```javascript
Promise.all([
    asyncFunc1(),
    asyncFunc2(),
])
.then(([result1, result2]) => {
    ···
})
.catch(err => {
    // Receives first rejection among the Promises
    ···
});
```

A promise has one of three states
1. *Pending:* The result hasn't been computed yet (initial state).
2. *Fulfilled:* The result was computed successfully.
3. *Rejected:* A failure occurred during computation.

So a promise is *settled* once it is either fulfilled or rejected and it cannot be settled again.
Chaining promises won't starve other tasks of processing time.
