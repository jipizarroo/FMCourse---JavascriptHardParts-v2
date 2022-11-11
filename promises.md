# Promises

ES6+ Solution.

* Initiate background web browser work and
* return a placeholder object ( promise ) immediately in JS

When you trigger something in the background, not only throw it into the web browser but have some sort of consequence in JS so that you can keep track of what's going on in the web browser in JS.

```
function display(data){
    console.log(data)
}

// fetch is a pretend function for JS, the web browser does most of the job, 
// but unlike setTimeout, it does leave something in the JS memory ( promise obj).
// { value: ...., unfulfilled: []}
const futureData = fetch('URL)
// at the moment at 0ms futureData = {value: undefined, unfulfilled: {}}, and at
// the web browser we send a network request.
// at 0ms the GET (network request) is not complete, is asking somewhere for the
// info we need. When we get the data after Xms, it will send the response into 
// futureData.value

// we set the display function to the unfulfilled object.
futureData.then(display)

console.log("Me first!")

at 270ms we get the response from the network and it's a string "Hi".
this gets the futureData.value = "Hi", and since the value got updated we get to run
the function saved in unfulfilled. Now it runs display("Hi") in the call stack.


CONSOLE TERMINAL
----------------
1ms => Me first!
270ms => Hi
```

But how do we trigger the function when the value gets updated? it could take a second, or even a day, this is where the unfulfilled function is very important, this is the function that runs when the value is finally updated.

But what is the command that pushes the function into the unfulfilled object? we can't do a simple push because is a hidden property. This is where `.then` shows up.&#x20;

When does exactly that function, the one that gets to run after we update the value info, gets to run? What rules does promise have?

{% hint style="info" %}
_then_ method and functionality to call on completion

Any code we want to run on the returned data must also be saved on the promise object.

Added using `.then` the method to the hidden property 'onFulfiltment'

Promise object will automatically trigger the attached function to run ( with its input being the returned data )
{% endhint %}



But we need to know how our promise-deferred functionality gets back into JS to be run.

<pre><code>
function display(data) { console.log(data); }
function printHello() { console.log("Hello"); }
function blockFor300ms() { // blocks JS thread for 300ms }

// sets a timer on the browser so that after 0ms it runs on JS the printHello function
<strong>0ms => setTimeout(printHello,0)
</strong>// at 0ms the timer is complete and it sends the printHello function to the
// callback queue

// we send a network request to the URL
// JS consequence => {value: undefined, unfulfilled: []} 
// Web browser consequence => Network request => futureData.value === undefined
1ms => const futureData = fetch(URL)

// we set the function in futureData hidden property unfulfilled
futureData.then(display);

// we run this function, brand new executing context, and it's on top of the
// call stack. 
// * As for this exercise, we get in the middle of this function, the response 'Hi' *
// this means the futureData.value is to be updated, and run the display function.
2ms => blockFor300ms()

302ms => console.log("Me first!");

CONSOLE TERMINAL
----------------
302ms => Me first!
303ms => Hi
304ms => Hello</code></pre>

Why did the fetch show up first? Well, the reason is there is another queue different from the _Callback queue,_ its called the _MicroTask Queue,_ the event loop checks that there is nothing in the Call Stack nor in the global memory that needs to be running, after that it checks **FIRST**, the _MicroTask Queue_, and then the _Callback queue._

### Promises

Problems:

* 99% of devs have no idea how they're working under the hood
* Debugging becomes super-hard as a result
* Devs fail technical interviews.

Benefits:

* Cleaner readable style with pseudo-synchronous style code.
* Nice error-handling process => there is another hidden property in the promise array and it is called: onRejection\[Array], it logs what happens when we get an error from the network request ex => futureData.catch() or => futureData.then(display, error), the second argument gets set up on onRejection.

__\
__

{% hint style="info" %}
_**We have rules for the execution of our asynchronously delayed code.**_

Hold promise-deferred functions in a microtask queue and callback function in a task queue ( Callback queue ) when the Web Browser Feature (API) finishes

Add the function to the Call stack ( i.e. run the function ) when:\


* Call stack is empty & all global code run ( Have the Event Loop check this condition )

Prioritize function in the _Microtask queue_ over the _Callback queue_
{% endhint %}

Promises, Web APIs, the Callback & MicroTask Queues, and Event loop enable:&#x20;

**Non-blocking applications:** This means we don't have to wait in the single thread and don't block further code from running.

**However long it takes:** We cannot predict when our Browser feature's work will finish so we let JS handle automatically running the function on its completion.

**Web applications:** Asynchronous JS is the backbone of the modern web - letting us build fast 'non-blocking' applications.
