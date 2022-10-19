# Functions & Callbacks notes



Generalizing Functions => 'Parameters' (placeholders) mean we dont need to decide what data to run our functionality on until we run the function.

* Then provide an actual value ('argument') when we run the function.

Higher order functions follow this same principle.

* We may not want to decide exactly what some of our functionality is until we run out function.

&#x20;\* Get def of execution context, call stack, closure \*

#### Higher order Function:

The outer function that takes in a function is out higher-order function.

Takes in a function or passes out a function.

Just a term to describe these functions - any function that does it we call that - but there's nothing different about them inherently.\


#### Callback Function:&#x20;

The function we insert is out callback function.\


#### Callbacks and higher order functions simplify our code and keep it DRY ( Don't repeat yourself ).

**Declarative readable code:** Map, filter, reduce - the most readable way to write code to work with data.

**Codesmith & pro interview prep:** One of the most popular topics to test in interview both for Codesmith and mid/senior level job interviews.

**Asynchronous JavaScript:** Callbacks are a core aspect of async JavaScript, and are under-the-hood of promises, async/await.

### Asynchronous & arrow functions.

* Improve immediate legibility of the code.
* But at least for our purposes here they are 'syntactic sugar' - we'll see their full effects later.
* Understanding how they're working under-the-hood is vital to avoid confusion.
