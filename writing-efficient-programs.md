# Notes from 'Writing efficient programs' - Jon Bentley


### Making programs more efficient by trading space for time
Programs sometimes run faster if you use more space to store information.

* **Rule 1 - Data structure augmentation**: Adding extra information to a data structure can reduce the runtime. This extra information can often be a pointer to the data described in the data structure.
* **Rule 2 - Store precomputed results**: Storing the result of a computation that is done multiple times can reduce runtime.
* **Rule 3 - Caching**: Data that is accessed often should be cheapest to access. This is usually already implemented in your computers hardware.
* **Rule 4 - Lazy evaluation**: Never evaluate an item before you need to use it. This strategy avoids unnecessary evaluation of items.

### Making programs more efficient by trading time for space
Save the space used by a program by increasing it's runtime.

* **Rule 1 - Packing**: By increasing time used to store and retrieve item, an item can be stored more space efficiently. For example not using 32 bits to store the integer 1, but use a type that only uses the space it needs.
* **Rule 2 - Interpreters**: Use an interpreter to compactly represent common sequences of operations. This rule is mostly useful in large systems where readability is more important than efficient code. An interpreter can then take the verbose code and make it more efficient.


Many of these rules can also be applied the other way around. So trying to reduce the information included in data structures, recompute results to save space, use eager evaluation (evaluate everything at once) to avoid checking if something has already been evaluated.


### Reducing the cost of loops
As loops are usually where the bulk of computation is done in a program, this is where lots of time can be saved.

* **Rule 1 - Code motion out of loops**: Avoid performing computations inside loops that could have been performed outside. This is especially valid for computations involving square roots or division.

Ex.

``` java
//No go
for(int i = 0; i < n; i++) {
  x[i] = x[i] * Math.sqrt(Math.PI/2);
}

//Yes!
double factor = Math.sqrt(Math.PI/2);
for(int i = 0; i < n; i++) {
  x[i] = x[i] * factor;
}
```
* **Rule 2 - Combining tests**: A loop should contain as few tests as possible (preferably only one). In while loops, try to only have one stop condition.
* **Rule 3 - Loop Unrolling**: In small loops, the most expensive operation can be incrementing the index and comparing it to the stop condition, so it is sometimes beneficial to unroll the loop. Where unrolling is getting rid of the loop completely.
* **Rule 4 - Transfer-Driven loop unrolling**: Remove trivial assignment from a loop.
* **Rule 5 - Loop fusion**: If two loops close to one another operate on the same set of elements, combine their operational parts.

### Logic rules

Logic rules often sacrifice clarity and robustness for speed. Use with care and comments.
* **Rule 1 - Exploit algebraic identities**: If an evaluation of a logical expression is costly and can be changed to an algebraically equivalent expression that is cheaper to evaluate, it should be.
De Morgans rules can be used to change for example `!A || !B` into `!(A || B)`. This especially holds for math functions, such as square root or logarithms, if they have an algebraically equivalent that is cheaper to compute, use that instead.
* **Rule 2 - Short-circuiting monotone functions**: Stop evaluating when you've found what you're looking for. For example if you want to know if there are negative numbers in an array, stop looping over the array once you find a negative number. `break` is the keyword here. Monotone means that the order is preserved, so if x is bigger than y, after applying the monotone function f f(x) will still be bigger than f(y).
* **Rule 3 - Reordering tests**: Test should be arranged to inexpensive test and often successful tests precede expensive and rarely successful tests. Used mostly in else if statements.
* **Rule 4 - Precompute logical functions**: A logical function that works over a finite domain can be replaced by a look up table that represents the domain. So instead of having a long chain of if else if for perhaps the characters of the alphabet, the input can instead be given to a table that returns the correct value.
* **Rule 5 - Boolean variable elimination**: Remove boolean variables by using an if else statement for the two values of the boolean value (true/false).

*Methods should handle all cases correctly and common cases efficiently*

### Expression rules
Reduce time devoted to evaluating expressions.
* **Rule 1 - Compile time initialization**: As many variables should be Initialized before program execution.
* **Rule 2 - Exploit algebraic identities**: If an evaluation of an expression is costly replace it by and algebraically equivalent expression that is cheaper to evaluate.
* **Rule 3 - Common subexpression elimination**: If the same expression is evaluated twice without it's variables being altered between evaluations then the second evaluation can be avoided by storing the result of the first and using that in place of the second. This only works for expressions that do not have side effects.
* **Rule 4 - Pairing computation**: If two similar expressions are often evaluated together then a new method that evaluated them together should be made. For example finding the minimum and the maximum of an array can be done in the same iteration through the array.

### Applying the techniques

Fundamental rules that underlie any application of the previous rules
* **Rule 1- Code Simplification**: Most fast programs are simple, so keep it simple stupid ;) Sources of harmful complexity includes: A lack of understanding the task and premature optimization.
* **Rule 2 - Problem Simplification**: To increase the efficiency of a program, simplify the problem it solves. Why store all values when you only need a few of them?
* **Rule 3 - Relentless suspicion**: Question that necessity of each instruction in a time critical piece of code and each field in a space critical data structure.
* **Rule 4 - Early binding**: Move work forward in time. So, do work now just once in hope of avoiding doing it many times over later on.

Most important point about efficiency, it is never the first step on making a program and never make changes for efficiency's sake that convert clean slow code to messy fast code.

Use statistics and analysis tools to determine where the hotspots are in your program. Focus your work on these.

The longer you work on trying to optimize a piece of code the less dramatic time improvements you'll see.

Rules should only be applied if:
* It is certain that efficiency is an important problem in the system.
* The particular piece of code being modified is a major bottleneck in the overall system performance.
* All major speedups achievable by changing the underlying algorithms and data structures have been incorporated.

When the hot spots of a system have been isolated, these are the steps to take:
* Identify the code to be changed
* Choose a rule to apply to it. Find out which of the rules best apply.
* Measure the effect of the modification. It might not have made a significant change, or potentially made it worse.
* Document the resulting program. Include a description of the clean code and of the modification made.

When modifying an already correct program, test extensively to ensure that your changes did not introduce bugs. Also every rule can backfire and decrease the efficiency.

Be willing to backtrack and try other rules, or if you have to choose between two rules, pick the one with the greatest performance gain. Be careful to not overuse them. If adding more speedups don't yield that much extra performance and mess up the code significantly, get rid of them.

For problem solving, remember that you can stand on the shoulders of many smart people and use one of their solutions instead of inventing the well every time. Chances are, the experts have come up with a better solution.

Only use these tools when they are needed, not because you want to show off your skills, the results could be disastrous. 
