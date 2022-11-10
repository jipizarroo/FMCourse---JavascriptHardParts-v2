# Asynchronous JavaScript.

Promises, Async & the Event Loop.\


Asynchronicity is the backbone of modern web development in JS.

JS is:

* Single-threaded ( one command runs at a time ).
* Synchronously executed ( each line is run in the order the code appears ).

So what if we have a task:

* Accessing Twitters's server to get new tweets takes a long time.
* Code we want to run using those tweets.

{% hint style="info" %}
Before we take on this task:\
Our core JS engine has 3 main parts:

* Thread of execution.
* Memory/variable environment.
* Call Stack

We need to add some new components:&#x20;

* Web browser APIs/Node background APIs
* Promises
* Event loop, Callback/Task queue, and micro task queue
{% endhint %}

Challenge: We want to wait for the tweets to be stored in tweets so that they're there to run displayTweets on - but no code can run in the meantime.

### ES5 solution: Introducing 'callback functions', and Web Browser APIs.

Web browsers functions that we can use in JS:\
\
JS / Web Browers\
console => console\
XHR/fetch => Network request.\
document => HTML DOM\
setTimeout => Timer\
These are functions that come from the browser and not from JS.&#x20;

```
// We are defining the function in the global memory.
function printHello() { console.log("Hello"); }

// We set a command to the web browser to set a timer that calls the printHello 
// function back in JS.

setTimeout(printHello,1000);

// The only job JS did here was to set out the timer on the web browser, 
// and thanks to this we can move to the next line of code.

console.log("Me first!");

// after 1000ms the web browser calls the function so that it can run on the
// call stack

CONSOLE LOG SCREEN
Me first!
Hello

```

### We are interacting with a world outside of JS now, so we need a new set of rules.

#### Callback Queue & Event loop.

```
function printHello() { console.log("Hello"); }
function blockFor1Sec() { // block in the JS thread for 1s }

// Even tho this is 0ms, it still sends the command so that it sets up a time in the
// WEB BROWSER, that after 0ms it calls our function in our JS code.
setTimeout(printHello, 0);


blockFor1Sec()

console.log("Me first!");

CONSOLE TERMINAL 
-----------------

return of blockFor1Sec
Me first!
Hello

```

Why if it's 0ms, and being the first function, the Hello is been printed last ??\


So this is the set of new rules we have when JS is interacting with the outside world ( In this case the Timer of the Web-Browser )\
\
This is the Callback Queue:\
After the function is ready on the Web-browser, remember that the function commanded to the Web-browser is the Time one, not printHello, printHello is never ever run in the Web-browser.\
We send printHello to the Callback Queue so it can be sent back into the JS thread. \
\
For a function to get out of the callback queue, there has to be nothing in the call stack ( Inside JS ), not even a global function running for it to be moved into it and finally run.\
\
The thing that checks if the call stack is empty and nothing, no function is running, is called an event loop, this is what grabs functions in the callback queue and it puts them into the call stack so that they can be run.
