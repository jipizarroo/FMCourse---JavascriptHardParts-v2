# Asynchronous JavaScript.

Promises, Async & the Event Loop.\


Asynchronicity is the backbone of modern web development in JS.

JS is:

* Single threaded ( one command runs at a time ).
* Synchronously executed ( each line is run in order the code appears ).

So what if we have a task:

* Accessing Twitters's server to get new tweets that takes a long time.
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
* Event loop, Callback/Task queue and micro task queue
{% endhint %}

Challenge: We want to wait for the tweets to be stored in tweets so that they're there to run displayTweets on - but no code can run in the meantime.

### ES5 solution: Introducing 'callback functions', and Web Browser APIs.

Web browsers functions that we can use in JS:\
\
JS / Web Browers\
console => console\
xhr/fetch => Network request.\
document => HTML DOM\
setTimeout => Timer\
This are functions that come from the browser and not from JS.&#x20;

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



