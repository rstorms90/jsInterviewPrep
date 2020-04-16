# JS Interview Prep

## Encapsulation

The core idea of OOP — creating a blueprint, a template that has a class, encapsulating a
bunch of (a little package of an object) data and functionality together.

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
