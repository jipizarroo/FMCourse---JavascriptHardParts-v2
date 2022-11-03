# Closure Introduction

* Most esoteric of JavaScript concepts.
* Enables powerful pro-level functions like 'once' and 'memoize'
  * once => Functions that run one and just once.
  * memoize => core function to optimize code.
* Many JS design patterns including the module patter use closure
* Build iterators, handle partial applications and maintain state in an asynchronous world.

When we invoke a brand new function, it creates a new local store of data ( local memory/variable environment/state ), and when its done it deletes everything ( the local memory, expect for the value returned ), the function does not remember what happen in the last invoke ( previous running ).

#### Functions with memories.

It all starts with us returning a function from another function. ( First step to understand closure )

```
function createFunction () {
    function multiplyBy2 (num){
        return num * 2;
    }
    return multiplyBy2;
}

const generatedFunc = createFunction();
const result = generatedFunc(3); // 6
```

Is important to understand that a function can be a VALUE, this is why we can return it.\
It is important that this result = generatedFunc(3), it means that it WAS the result of createFunction().\
generatedFunction as no relation with createFunction. It was only created by it, but no relationship with it.\
\
**Why not save the function as a global function, why would we create it inside another function and return it?** \


Calling a function in the same function call as it was defined.

```
function outer (){
    let counter = 0;
    function incrementCounter (){
        counter ++;
    }
    incrementCounter();
}
outer();
```

To understand this => [https://frontendmasters.com/courses/javascript-hard-parts-v2/nested-function-scope/](https://frontendmasters.com/courses/javascript-hard-parts-v2/nested-function-scope/)

\
It's important to understand how the call stack is called when the function is running.\
Do we get access to counter inside of incrementCounter because incrementCounter is been called inside the local memory of outer? or is it because we defined the code of incrementCounter inside the local memory of outer that we get access?\
The answer to this, is on understanding what closure is.\


```
function outer (){
    let counter = 0;
    function incrementCounter(){ counter ++; }
    return incrementCounter;
}
const myNewFunction = outer();
newFunction();
newFunction();
```

incrementCounter got the connected data (counter in this case) with him into the global memory.\
so when the function is trying to reach counter in newFunction, it goes into the global memory, accesses its value and it finds counter = 0 (on the first run), modifying it to 1.\
\
How does the function, incrementCounter, gets to grab the surrounding data and store it?\
It gets a hidden property \[\[ scope ]], this is how it brings its hidden property to the global memory.\
The only way of reaching the scope of incrementCounter is by calling the function.\


### Multiple Closure Instances.

```
function outer (){
    let counter = 0;
    function incrementCounter(){ counter ++; }
    return incrementCounter;
}
const myNewFunction = outer();
newFunction();
newFunction();

const anotherFunction = outer();
anotherFunction();
anotherFunction();
```

\*Would this mean that when we call either myNewFunction or anotherFunction, counter will increase either way? would it be 4 at the end ??\*\


The answer is no, its a scope for each local memory of each functions, it would mean that we get a backpack for each function, so we would get in console: 1, 2  --- 1, 2.\


Closure gives our functions **persistent memories** and entirely new toolkit for writing professional code.

* **Helper functions:** Everyday professional helper functions like 'once' and 'memoize'.
* **Iterators and generators:** Which use lexical scoping and closure to achieve the most contemporary patterns for handling data in JS.
* **Module pattern:** Preserve state for the life of an application without polluting the global namespace.
* **Asynchronous JS**: Callbacks and Promises rely on closure to persist state in an asynchronous environment&#x20;

