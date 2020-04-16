# JS Interview Prep

## Advantages of OOP?

- OOP provides a clear modular structure for programs
- Great for defining abstract data types
- Easy to maintain and modify existing code as new objects
  can be created with small differences to existing ones

## Encapsulation

The core idea of OOP — creating a blueprint, a template that has a class, encapsulating a
bunch of (a little package of an object) data and functionality together.

### Why is encapsulation important?

Encapsulation helps in isolating implementation details from the behavior exposed to clients of a class (other classes/functions that are using this class), and gives you more control over coupling in your code.

```
public class Car
{
  //...
  public float GetFuelPercentage() { /* ... */ };

  //...

  private float gasoline;
  //...
}
```

Note that the client using the function which gives you the amount of fuel in the car doesn't care what type of fuel does the car use. This abstraction separates the concern (Amount of fuel) from unimportant (in this context) detail: whether it is gas, oil or anything else.

The second thing is that author of the class is free to do anything they want with the internals of the class, for example changing gasoline to oil, and other things, as long as they don't change its behaviour. This is thanks to the fact, that they can be sure that no one depends on these details, because they are private. The fewer dependencies there are in the code the more flexible and easier to maintain it is.

## Inheritance

Once you have made a class and you want a class to inherit properties or functions, but modified
with its own custom data.

## Polymorphism

```
class Particle {

}

class Confetti extends Particle {

}

Confetti c = new Confetti();

// Allowed because Confetti extends Particle
Particle c = new Confetti();
```

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

Regular functions created using function declarations or expressions are constructible and callable. Since regular functions are constructible, they can be called using the new keyword.
However, the arrow functions are only callable and not constructible, i.e arrow functions can never be used as constructor functions. Hence, they can never be invoked with the new keyword

```
let add = (x, y) => console.log(x + y);
new add(2,3);
```

Above will throw: `Uncaught TypeError: add is not a constructor at...`

## Difference between let & var
