# JS Interview Prep

## Hoisting

JavaScript only hoists declarations, not initializations.

Example 1 gives same result as Example 2:

```
x = 5; // Assign 5 to x

elem = document.getElementById("demo"); // Find an element
elem.innerHTML = x;                     // Display x in the element

var x; // Declare x
```

```
var x; // Declare x
x = 5; // Assign 5 to x

elem = document.getElementById("demo"); // Find an element
elem.innerHTML = x;                     // Display x in the element
```

`let` & `const` are not hoisted

## Difference between Arrow functions & Regular functions

### Arguments Binding

Arrow functions do not have an arguments binding. However, they have access to the arguments object of the closest
non-arrow parent function. Named and rest parameters are heavily relied upon to capture the arguments passed to
arrow functions.

### Use of `this` keyword

Unlike regular functions, arrow functions do not have their own `this`. The value of `this` inside an arrow function
remains the same throughout the lifecycle of the function and is always bound to the value of `this` in the closest
non-arrow parent function.

```
let me = {
 name: "Ashutosh Verma",
 thisInArrow:() => {
 console.log("My name is " + this.name); // no 'this' binding here
 },
 thisInRegular(){
 console.log("My name is " + this.name); // 'this' binding works here
 }
};
me.thisInArrow();
me.thisInRegular();
```

### Using `new` keyword

Regular functions created using function declarations or expressions are constructible and callable. Since regular functions are constructible, they can be called using the `new` keyword.
However, the arrow functions are only callable and not constructible, i.e arrow functions can never be used as constructor functions. Hence, they can never be invoked with the `new` keyword

```
let add = (x, y) => console.log(x + y);
new add(2,3);
```

Above will throw: `Uncaught TypeError: add is not a constructor at...`

## Difference between `let` & `var`

Main difference is scoping rules. Variables declared by var keyword are scoped to the immediate function body (hence the function scope) while let variables are scoped to the immediate enclosing block denoted by { } (hence the block scope).

```
function run() {
  var foo = "Foo";
  let bar = "Bar";

  console.log(foo, bar);

  {
    let baz = "Bazz";
    console.log(baz);
  }

  console.log(baz); // ReferenceError
}

run();
```

## Is a `const` variable immutable?

The const declaration creates a read-only reference to a value. It does not mean the value it holds is immutable, just that the variable identifier cannot be reassigned.

## Event Bubbling

When you click on a button, the event passes from inner event target to Document. Click event pass in the following order:

1. `button`
2. `div`
3. `body`
4. `HTML`
5. `Document`

`event.stopPropagation()` will stop event bubbling from occurring.

## Event Delegation

```
const character = document.getElementById("disney-character");
character.addEventListener('click', showCharactersName);
```

In our code above, on page load, the event listener finds an HTML element with the id disney-character and sets a click event listener on that HTML element.
This works fine if the element exists on the page when the page is loaded. However what happens to the event listener when the element is added to the DOM (webpage) after the initial page load?

The whole idea behind event delegation is that instead of listening for a change on the inputs directly, we should look for an HTML element that is going to be on the page when the page initially loads.

```
<ul class=”characters”>
</ul>
<script>
  function toggleDone (event) {
    console.log(event.target)
  }
  const characterList = document.querySelector('.characters')
  characterList.addEventListener('click', toggleDone)
</script>
```

The event.currentTarget identifies the current target for the event, as the event traverses the DOM. It always refers to the element to which the event listener has been attached. In our case the event listener was attached to the unordered list, characters, so that is what we see in our console.

#### Writing Event Delegation in JavaScript

Because we now know that the EVENT.TARGET identifies the HTML elements on which the event occurred, and we also know what element we want to listen for (the input element), solving this in JavaScript is relatively easy.

```
//Event Delegation
function toggleDone (event) {
  if (!event.target.matches(‘input’)) return
  console.log(event.target)
  //We now have the correct input - we can manipulate the node here
}
```

Basically the code above states, if the event target that was clicked DOES NOT match the input element, exit the function.
If the event target that was clicked DOES match the input element, console.log the event.target and execute subsequent JavaScript code on that child node.
This is important, now we can be confident that a user clicked the correct child node, even though the inputs were added to the DOM after the initial page load.

## What is Type Safety?

Type safety means that the compiler will validate types while compiling, and throw an error if you try to assign the wrong type to a variable.

## What's the difference between strongly types and weakly typed languages?

The main difference, roughly speaking, between a strongly typed language and a weakly typed one is that a weakly typed one makes conversions between unrelated types implicitly, while a strongly typed one typically disallows implicit conversions between unrelated types

## What is software coupling?

- Coupling is the indication of the relationships between modules.
- Coupling shows the relative dependence/interdependence among the modules.
- Coupling is a degree to which a component / module is connected to the other modules.
- While designing you should strive for low coupling i.e. dependency between modules should be less
- Making private fields, private methods and non public classes provides loose coupling.
- Coupling is Inter-Module Concept.

## Functional Programming & Currying

This function takes three numbers, multiplies the numbers and returns the result.

```
function multiply(a, b, c) {
    return a * b * c;
}
multiply(1,2,3); // 6
```

See, how we called the multiply function with the arguments in full. Let’s create a curried version of the function and see how we would call the same function (and get the same result) in a series of calls:

```
function multiply(a) {
    return (b) => {
        return (c) => {
            return a * b * c
        }
    }
}
log(multiply(1)(2)(3)) // 6
```

We could separate this multiply(1)(2)(3) to understand it better:

```
const mul1 = multiply(1);
const mul2 = mul1(2);
const result = mul2(3);
log(result); // 6
```

## What does `bind()` do?

The bind() method creates a new function that, when called, has its this keyword set to the provided value, with a given sequence of arguments preceding any provided when the new function is called.

```
var Button = function(content) {
  this.content = content;
};
Button.prototype.click = function() {
  console.log(this.content + ' clicked');
}

var myButton = new Button('OK');
myButton.click();

var looseClick = myButton.click;
looseClick(); // not bound, 'this' is not myButton - it is the global object

var boundClick = myButton.click.bind(myButton);
boundClick(); // bound, 'this' is myButton
```

Response:

```
OK clicked
undefined clicked
OK clicked
```

## What is the difference between a promise and a callback?

Promises are cleaner way for running asynchronous tasks to look more like synchronous and also provide catching mechanism which are not in callbacks. Promises are built over callbacks. Promises are a very mighty abstraction, allow cleaner and better, functional code with less error-prone boilerplate. I would recommend to get the flow of how stuff works and think in terms of promises way.

#### Callbacks

```
function myFetchData(url, callback) {
  makeNetworkCall( url, (response) => {
    if (response.success) {
      callback(response.data, nil)
    } else {
      callback(nil, response.error)
    }
  });
}
/* Imagine makeNetworkCall makes a get call to the url which it accepts as the first param. On executing the call, it fills its response in the second argument it takes */
let url = "https://jsonplaceholder.typicode.com/todos/1";
myFetchData(url, function(data, error) {
  if (error != nil) {
    console.log("data", data)
  } else {
    console.log("error", error)
  }
});
```

We would do the above same task with promise below.

#### Promises

```
let myFetchData = (url) => {
  return new Promise((resolve, reject) => {
    makeNetworkCall( url , (response) => {
      if (response.success) {
        resolve(response.data)
      } else {
        reject(response.error)
      }
    });
  })
}
/* Imagine makeNetworkCall makes a get call to the url which it accepts as the first param. On executing the call, it fills its response in the second argument it takes */
let url = "https://jsonplaceholder.typicode.com/todos/1";
myFetchData(url)
  .then(data) {
    console.log("data", data)
  }
  .catch(error) {
    console.log("error", error)
  }
```

```
// Here's how you create a promise:
var promise = new Promise(function(resolve, reject) {
  // do a thing, possibly async, then…

  if (/* everything turned out fine */) {
    resolve("Stuff worked!");
  }
  else {
    reject(Error("It broke"));
  }
});
```

The promise constructor takes one argument, a callback with two parameters, resolve and reject. Do something within the callback, perhaps async, then call resolve if everything worked, otherwise call reject.
Here’s how you use that promise:

```
promise.then(function(result) {
  console.log(result); // "Stuff worked!"
}, function(err) {
  console.log(err); // Error: "It broke"
});
```

What’s ‘.error’ then in promises?
As we saw earlier, then() takes two arguments, one for success, one for failure (or fulfill and reject, in promises-speak):

```
get('supplyData.json').then(function(response) {
  console.log("Success!", response);
}, function(error) {
  console.log("Failed!", error);
})
```

Now with .catch ()

```
get('supplyData.json').then(function(response) {
  console.log("Success!", response);
}).catch(function(error) {
  console.log("Failed!", error);
})
```

There’s nothing special about catch(), it's just sugar for then(undefined, func), but it's more readable. Note that the two code examples above do not behave the same, the latter is equivalent to:

```
get('supplyData.json').then(function(response) {
  console.log("Success!", response);
}).then(undefined, function(error) {
  console.log("Failed!", error);
})
```

Dive deep into chaining.

```
new Promise(function(resolve, reject) {
  setTimeout(() => resolve(1), 1000); // (Zone 1)
  })
  .then(function(result) { // (Zone 2)
    alert(result); // 1
    return result * 2;
  })
  .then(function(result) { // (Zone 3)
    alert(result); // 2
    return result * 2;
  })
  .then(function(result) {
    alert(result); // 4
    return result * 2;
});
```

The whole thing works, because a call to promise.then returns a promise, so that we can call the next .then on it. In Zone 2, the return value 2 is wrapped in Promise and sent further. Thats why we can call .then in Zone 3.

Browsers are pretty good at downloading multiple things at once, so we’re losing performance by downloading supplydata one after the other. What we want to do is download them all at the same time, then process them when they’ve all arrived. Thankfully there’s an API for this:

```
Promise.all(arrayOfPromises).then(function(arrayOfResults) {
  //...
})
```
