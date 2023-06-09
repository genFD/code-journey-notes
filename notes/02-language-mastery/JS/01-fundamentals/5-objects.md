# OBJECTS

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Fundamentals](#object-fundamentals)
  - [Advanced Objects](#advanced-objects)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

## Notes

1. Objects fundamentals

   - Creating object literal
   - Accessing properties
   - Bracket notation
   - Property assignement
   - Methods
   - Pass By reference
   - Looping through objects

2. Advanced Objects :

   - The this keyword
   - Arrow functions and This
   - Privacy
   - Getters
   - Setters
   - Factory functions
   - Property value shorthand
   - Destructuring assignment
   - Built-in Object methods

## `Object fundamentals`

**[⬆ back to top](#table-of-contents)**

- Objects store collections of key-value pairs.

- Each key-value pair is a property—when a property is a function it is known as a method.

- An object literal is composed of comma-separated key-value pairs surrounded by curly braces.

We use curly braces, {}, to designate an object literal:

```js
let spaceship = {} // spaceship is an empty object
```

```js
// An object literal with two key-value pairs
let spaceship = {
  'Fuel Type': 'diesel',
  color: 'silver',
}
```

- You can access, add or edit a property within an object by using dot notation or bracket notation.

```js
let spaceship = {
  homePlanet: 'Earth',
  color: 'silver',
}
spaceship.homePlanet // Returns 'Earth',
spaceship.color // Returns 'silver',
```

```js
let spaceship = {
  'Fuel Type': 'Turbo Fuel',
  'Active Duty': true,
  homePlanet: 'Earth',
  numCrew: 5,
}
spaceship['Active Duty'] // Returns true
```

**Note**: We must use bracket notation when accessing keys that have numbers, spaces, or special characters in them.

- We can add methods to our object literals using key-value syntax with anonymous function expressions as values or by using the new ES6 method syntax.

`Using anonymous function expression`

```js
const alienShip = {
  invade: function () {
    console.log(
      'Hello! We have come to dominate your planet. Instead of Earth, it shall be called New Xaculon.'
    )
  },
}
```

`Es6 Syntax`

```js
const alienShip = {
  invade() {
    console.log(
      'Hello! We have come to dominate your planet. Instead of Earth, it shall be called New Xaculon.'
    )
  },
}
```

- We can navigate complex, nested objects by chaining operators.

- Objects are mutable—we can change their properties even when they’re declared with const.

- Objects are passed by reference— when we make changes to an object passed into a function, those changes are permanent.

- We can iterate through objects using the For...in syntax.

```js
let spaceship = {
  crew: {
    captain: {
      name: 'Lily',
      degree: 'Computer Engineering',
      cheerTeam() {
        console.log('You got this!')
      },
    },
    'chief officer': {
      name: 'Dan',
      degree: 'Aerospace Engineering',
      agree() {
        console.log('I agree, captain!')
      },
    },
    medic: {
      name: 'Clementine',
      degree: 'Physics',
      announce() {
        console.log(`Jets on!`)
      },
    },
    translator: {
      name: 'Shauna',
      degree: 'Conservation Science',
      powerFuel() {
        console.log('The tank is full!')
      },
    },
  },
}

// for...in
for (let crewMember in spaceship.crew) {
  console.log(`${crewMember}: ${spaceship.crew[crewMember].name}`)
}
```

## `Advanced Objects`

**[⬆ back to top](#table-of-contents)**

- The object that a method belongs to is called the `calling object`.

The this keyword refers to the `calling object` and `can be used to access properties of the calling object`

```js
const goat = {
  dietType: 'herbivore',
  makeSound() {
    console.log('baaa')
  },
  diet() {
    console.log(dietType)
  },
}
goat.diet()
// Output will be "ReferenceError: dietType is not defined"
```

But if we use the `this` keyword:

```js
const goat = {
  dietType: 'herbivore',
  makeSound() {
    console.log('baaa')
  },
  diet() {
    console.log(this.dietType)
  },
}

goat.diet()
// Output: herbivore
```

- Methods do not automatically have access to other internal properties of the calling object.

- The value of this depends on where the this is being accessed from.

- We cannot use arrow functions as methods if we want to access other internal properties.

```js
const goat = {
  dietType: 'herbivore',
  makeSound() {
    console.log('baaa')
  },
  diet: () => {
    console.log(this.dietType)
  },
}

goat.diet() // Prints undefined
```

Arrow functions inherently bind, or tie, an already defined this value to the function itself that is NOT the calling object.

- JavaScript objects do not have built-in privacy, rather there are conventions to follow to notify other developers about the intent of the code.

One common convention is to place an underscore `_` before the name of a property to mean that the property should not be altered. Here’s an example of using `_` to prepend a property.

```js
const bankAccount = {
  _amount: 1000,
}
```

- The usage of an underscore before a property name means that the original developer did not intend for that property to be directly changed.

- Setters and getter methods allow for more detailed ways of accessing and assigning properties.

`Getters are methods that get and return the internal properties of an object`.
But they can do more than just retrieve the value of a property!

```js
const person = {
  _firstName: 'John',
  _lastName: 'Doe',
  get fullName() {
    if (this._firstName && this._lastName) {
      return `${this._firstName} ${this._lastName}`
    } else {
      return 'Missing a first name or a last name.'
    }
  },
}

// To call the getter method:
person.fullName // 'John Doe'
```

`Setter methods reassign values of existing properties within an object.`

```js
const person = {
  _age: 37,
  set age(newAge) {
    if (typeof newAge === 'number') {
      this._age = newAge
    } else {
      console.log('You must assign a number to age')
    }
  },
}
```

Then to use the setter method:

```js
person.age = 40
console.log(person._age) // Logs: 40
person.age = '40' // Logs: You must assign a number to age
```

- Factory functions allow us to create object instances quickly and repeatedly.

```js
const monsterFactory = (name, age, energySource, catchPhrase) => {
  return {
    name: name,
    age: age,
    energySource: energySource,
    scare() {
      console.log(catchPhrase)
    },
  }
}
```

- There are different ways to use object destructuring: one way is the property value shorthand and another is destructured assignment.

we can use a destructuring technique, called `property value shorthand`, to save ourselves some keystrokes.

```js
const monsterFactory = (name, age) => {
  return {
    name,
    age,
  }
}
```

we don’t have to repeat ourselves for property assignments!

we can also take advantage of a destructuring technique called `destructured assignment` to save ourselves some keystrokes.

```js
const { residence } = vampire
console.log(residence) // Prints 'Transylvania'
```
