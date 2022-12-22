# Prototype Chain

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

We check if hasOwnPropety exists in user1, no => we go to the `userFunctionStore` to check it, no => we move to the `object.prototype`, yes, we find it and execute it.

&#x20;     &#x20;

