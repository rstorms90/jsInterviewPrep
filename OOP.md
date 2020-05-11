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

Basically, it is the act of redefining a method inside a derived child class of a parent.

```
class Animal {
  constructor (name) {
    this.name = name;
  }

  makeSound() {
    console.log("Generic animal sound!");
  }
}

class Dog extends Animal {
  constructor(name) {
    super(name) //Call constructor of parent
  }

  makeSound() {
    console.log("Woof! Woof!")
  }
}

const a1 = new Animal("Dom");
const a2 = new Dog("Jeff");

a1.makeSound(); //"Generic animal sound!"
a2.makeSound(); //"Woof! Woof!"
```

#### So, what is happening here?

`const a2` is checking on the Dog class if it has the `makeSound()` method and overrides the value. If it doesn't, then it checks the parent class and uses that method. This is polymorphism.

You can also call the parent method inside the derived method:

```
class Dog extends Animal {
  constructor(name) {
    super(name) //Call constructor of parent
  }

  makeSound() {
    super.makeSound(); //"Generic animal sound!"
    console.log("Woof! Woof!")
  }
}
```

```
class Particle {

}

class Confetti extends Particle {

}

Confetti c = new Confetti();

// Allowed because Confetti extends Particle
Particle c = new Confetti();
```

## Closure

A closure is the combination of a function bundled together (enclosed) with references to its surrounding state (the lexical environment). In other words, a closure gives you access to an outer function’s scope from an inner function. In JavaScript, closures are created every time a function is created, at function creation time.

```
var outerNumber = 30
function outer() {
  var innerNumber = 20
function inner() {
  return outerNumber + innerNumber
}
return inner()
}
outer() //prints 50
```

Here the function inner is considered a closure. That’s it.

Closure in ES6:

```
const outer = () => {
  let i = 0;
return () => {
    return i++;
  }
}
let increment = outer();
increment() //prints 0
increment() //prints 1
```

#### Why is this important?

```
var outerNumber = 30
function outer() {
  var innerNumber = 20
function inner() {
  return outerNumber + innerNumber
}
return inner()
}
outer() //prints 50
```

What is unique is that the inner function inner has access to variables in outer function, outer's scope, which in our case is the variable outerNumber.
Having said that, an important facet of a closure is that the inner function still has access to the outer function variables even after the outer function has returned.

#### What in tarnation does that mean?

```
function fullName(firstName) {
  const intro = 'Hello'

  function combineNames(lastName) {
    return `${intro} ${firstName} ${lastName}!`
  }
  return combineNames
}
const russFullName = fullName('Russ')
russFullName(); //prints `Hello Russ undefined!
russFullName('Storms'); //prints `Hello Russ Storms!`
```

In our case the inner function is combineNames. It still has access to the outer function variables, `firstName === 'Russ'`, despite the fact that fullName, the outer function has been called and returned.
So when I wrote in my console `const russFullName = fullName('Russ')`, the russFullName variable nows contains the inner function combineNames, and a reference to the variables of the outer function, firstName.
Therefore when I call russFullName() I see back the following text:
Hello Russ undefined!

RECAP: The VARIABLE russFullName now contains a reference to the following 3 things:

- Variable of the outer function intro === 'Hello'
- Variable of the outer function firstName === 'Russ'
- The inner function combineNames

Continuing on… now when we call russFullName('Storms') the following will print out:
Hello Russ Storms.

So we call russFullName with the string Storms, it passes the the string Storms as a parameter to the inner function.
As stated above, the inner function ALSO has access to the variables of the outer function intro == 'Hello'& firstName === 'Russ' despite the fact that the outer function fullName has already been called and returned.
Having everything the inner function needs, the inner function executes its code:
return `${intro} ${firstName} ${lastName}!`;
AND the result is:
Hello Russ Storms!
Boom!

When you have a closure, the inner function, the closure, still has access to the outer function’s variables. It does this by storing references to the outer function’s variables. It does not store the actual value.
