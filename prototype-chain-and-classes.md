# Prototype Chain & Classes

Store the function in just one object and have the interpreter, if it doesn't find the function on user1, look up to that object to check if it's there.

Link user1 and functionStore so the interpreter, on not finding .increment, makes sure to check up in functionStore where it would find it.

Make the link with `Object.create()` technique.

```
// Solution: Using the prototype chain.

function userCreator(name, score) {
    const newUser = Object.create(userFunctionStore);
    newUser.name = name;
    newUser.score = score;
    return newUser;
}

const userFunctionStore = {
    increment: function(){this.score++},
    login: function(){console.log("Logged in");}
}

const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);
user1.increment(); // return user.score = 4;
```

<figure><img src=".gitbook/assets/Screen Shot 2022-12-22 at 14.59.18.png" alt=""><figcaption><p>See how we have a link (__proto__) to the useFunctionStore global function from user1.</p></figcaption></figure>

&#x20;                                   What happens when we run the increment function then?\


<figure><img src=".gitbook/assets/Screen Shot 2022-12-22 at 15.10.19.png" alt=""><figcaption></figcaption></figure>

What if we want to confirm our user1 has a property score? => We can use the `hasOwnPoperty` method, but where is it? \
Well there is a headline object in JS called `object.prototype`, and it has a bunch of functions that are available to all of our objects, that means that our `userFunctionStore` has its own proto property and that means that the proto inside userFunctionStore is connected to the `object.prototype` meaning that when we call `user1.hasOwnProperty("user"):`

We check if hasOwnPropety exists in user1, no => we go to the `userFunctionStore` to check it, no => we move to the `object.prototype`, yes, we find it and execute it.\
\


#### Arrow functions override the normal _`this`_ rule.



```
function userCreator(name, score) {
    const newUser = Object.create(userFunctionStore);
    newUser.name = name;
    newUser.score = score;
    return newUser;
}

const userFunctionStore = {
    increment: function (){
    const add1 = () => { this.score++ }
    add1();
    }
}

const user1 = userCreator("Will", 3);
const user2 = userCreator("Tim", 5);
user1.increment(); // return user.score = 4;
```

<figure><img src=".gitbook/assets/Screen Shot 2022-12-23 at 19.32.27.png" alt=""><figcaption><p>now our inner function gets its this set by where it was saved - it's a lexically scoped this.</p></figcaption></figure>

What if we declare the increment function as an arrow function??\
If we run `increment()` and its an arrow function, it would be determined by where it was stored ( the global store ) and the whole thing would fall apart.

### Introducing the keyword that automates the hard work: new

When we call the function that returns an object with `new` in front, we automate 2 things.

* Create a new user object `const user1 = new userCreator("Eva", 9)`
* Return the new user object `const user2 = new userCreator("Tim", 5)`

But now we need to adjust how we write the body of `userCreator` - how can we:

* Refer to the auto-created object? => this
* Know where to put our single copies of functions? => prototype object &#x20;

```javascript
// The new keyword automates a lot of out manual work.
function userCreator(name, score) {
    this.name = name;
    this.score = score;
}

userCreator.prototype // {};
userCreator.prototype.increment = function(){
    this.score++
}
const user1 = new userCreator("Will", 3)
```

### Interlude => Functions are both objects and functions!

```javascript
function multiplyBy2(num){
    return num*2
}

multiplyBy2.stored = 5
multiplyBy2(3) // 6
multiplyBy2.stored // 5
multiplyBy2.prototype // {}
```

<figure><img src=".gitbook/assets/Screen Shot 2022-12-23 at 19.55.57.png" alt=""><figcaption><p>We could use the fact that all functions have a default property 'prototype' on their object version, (itself an object) - to replace our 'functionStore' object. </p></figcaption></figure>

The new keyword automates a lot of our manual work.

```
function userCreator(name, score){
    this.name = name;
    this.score = score;
}

userCreator.prototype.increment = function(){ this.score++; };
userCreator.prototype.login = function(){ console.log("login"); };

const user1 = new userCreator("Eva", 9)

user.increment();
```

<figure><img src=".gitbook/assets/Screen Shot 2022-12-25 at 22.30.47.png" alt=""><figcaption><p>This happens when we declare a function in JS.</p></figcaption></figure>

<figure><img src=".gitbook/assets/Screen Shot 2022-12-25 at 22.38.15.png" alt=""><figcaption><p>This is what happens when we declare user1 with the new keyboard.</p></figcaption></figure>

When we call for increment in user1, we must remember that the function increment it is not saved inside the user1 object, but it is referenced to the global function increment inside of the userCreator, this is the beauty of the prototype inside functions.

Introducing the keyword that automates the hard work: new.

Benefits:&#x20;

* Faster to write. Often used in practice in professional code.

Problems:&#x20;

* 95% of developers have no idea how it works and therefore can't debug.

&#x20;                                      Solution 4: The class 'syntactic sugar'

We're writing our shared methods separately from out object 'constructor' itself ( off in the userCreator.prototype object )\
\
Other languages let us do this all in one place. ES2015 lets us do so too.

```
class userCreator {
    constructor (name, score){
        this.name = name;
        this.score = score;
    }
    increment(){
        this.score++;
    }
    login(){ console.log("login"); }
}

const user1 = new UserCreator("Eva", 9);
user1.increment();
```

Nothing has changed under the hood with this compared to solution 3. A class is the same function + object combo. This makes it more readable for other devs, but it's just the same!

Benefits:

* Emerging as a new standard
* Feels more like style of other languages (e.g. Python)

Problems:&#x20;

* 99% of devs have no idea how it works.
