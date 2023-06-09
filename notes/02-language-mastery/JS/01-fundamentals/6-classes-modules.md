# CLASSES - MODULES

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Classes](#classes)
  - [Modules](#modules)
    - [Runtime Environment](#runtime-environment)
    - [Node JS module exports](#nodejs-moduleexports)
    - [Node JS Require](#nodejs-require)
    - [ES6 Import](#es6-import-statement)
    - [ES6 Export](#es6-export-statement)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

## Notes

## `Classes`

**[⬆ back to top](#table-of-contents)**

- Classes are templates for objects

Classes make it easy to create multiple objects that share property names and methods.

Each instance of a class has the same properties, getters, setters, and methods. Only the property values change.

- Javascript calls a constructor method when we create a new instance of a class

It's called to set the property values for each instance.
-> Objects have properties -> properties needs values

- Inheritance is when we create a parent class with properties and methods that we can extend to child classes

The purpose of inheritance is : Sharing data and methods between objects.

- We use the `extends` keyword to create a subclass

the subclass automatically inherits the parent methods, getters and setters, you need to use the `super` keyword to set the parent properties.

- The `super` keyword calls the `constructor()` of a parent class

In the subclass constructor method, `super` should be called before `this` otherwise Javascript throws an error.

- `Static methods` are called on the class, but not on instances of the class

We use static methods when we want a class to have methods that aren’t available in individual instances, but that you can call directly from the class.

The `Date` class, for example — you can both

1- create Date instances to represent whatever date you want,

```js
const myDate = new Date()
```

2- and call static methods, like Date.now() which returns the current date, directly from the class. The .now() method is static, so you can call it directly from the class, but not from an instance of the class

```js
const today = Date.now()
```

## `Modules`

**[⬆ back to top](#table-of-contents)**

**Note : The words `module` and `file` are often used interchangably**

### Runtime environment

**[⬆ back to top](#table-of-contents)**

A runtime environment is where your program will be executed. JavaScript code may be executed in one of two runtime environments:

1. a browser’s runtime environment
2. the Node runtime environment

In each of these environments, different data values and functions are available, and these differences help distinguish front-end applications from back-end applications.

- Front-end JavaScript applications are executed in a browser’s runtime environment and have access to the window object.

- Back-end JavaScript applications are executed in the Node runtime environment and have access to the file system, databases, and networks attached to the server.

### `Node.js module.exports`

**[⬆ back to top](#table-of-contents)**

- How to use the `Node.js module.exports` object to export code from a file ?

```js
/* converters.js */
function celsiusToFahrenheit(celsius) {
  return celsius * (9 / 5) + 32
}

module.exports.celsiusToFahrenheit = celsiusToFahrenheit

module.exports.fahrenheitToCelsius = function fahrenheit {
  return (fahrenheit - 32) * (5 / 9)
}
```

1. the already-defined function celsiusToFahrenheit() is assigned to `module.exports.celsiusToFahrenheit`.

2. a new function expression is declared and assigned to `module.exports.fahrenheitToCelsius`.

### `Node.js require()`

**[⬆ back to top](#table-of-contents)**

- How to use the `Node.js require()` function to import code from another module ?

`The require()` function accepts a string as an argument. That string provides the `file path` to the module you would like to import.

```js
/* water-limits.js */
const converters = require('./converters.js')
const freezingPointC = 0
const boilingPointC = 100

const freezingPointF = converters.celsiusToFahrenheit(freezingPointC)
const boilingPointF = converters.celsiusToFahrenheit(boilingPointC)
```

- or using Object Destructuring to be more Selective :

```js
/* celsius-to-fahrenheit.js */
const { celsiusToFahrenheit } = require('./converters.js')

const celsiusInput = process.argv[2]
const fahrenheitValue = celsiusToFahrenheit(celsiusInput)
```

### `ES6 export statement`

**[⬆ back to top](#table-of-contents)**

- How to use the ES6 export statement to export code from a file ?

- named export syntax :

```js
const resourceToExportA = () => {}
const resourceToExportB = () => {}
export { resourceToExportA, resourceToExportB, ...}

```

- individual values may be exported as named exports

```js
/* dom-functions.js */
export const toggleHiddenElement = (domElement) => {
  // logic omitted...
}

export const changeToFunkyColor = (domElement) => {
  // logic omitted...
}
```

- default export syntax

```js
const resources = {
  valueA,
  valueB,
}
export { resources as default }
```

- You may also see this shorthand syntax where the value follows export default is the default value to be exported:

```js
const resources = {
  valueA,
  valueB,
}
export default resources
```

### `ES6 import statement`

**[⬆ back to top](#table-of-contents)**

- How to use the ES6 import statement to import code from another module ?

- The ES6 syntax for importing `named resources` from modules is similar to the export syntax:

```js
import { exportedResourceA, exportedResourceB } from '/path/to/module.js'
```

Now, you must also update your html file:

```html
<!-- secret-messages.html -->
<html>
  <head>
    <title>Secret Messages</title>
  </head>
  <body>
    <script type="module" src="./secret-messages.js"></script>
  </body>
</html>
```

the only thing that changes is the addition of the attribute `type='module'` to the `<script>` element.

- ES6 includes syntax for renaming imported resources by adding in the keyword `as` to avoid naming collisions:

```js
import { exportedResource as newlyNamedResource } from '/path/to/module'
```

- The syntax for importing default exports looks like this:

```js
import importedResources from 'module.js'
```

the imported default value may be given any name the programmer chooses.

- It should be noted that if the default export is an object, the values inside cannot be extracted until after the object is imported, like so:

```js
// This will work...
import resources from 'module.js'
const { valueA, valueB } = resources

// This will not work...
import { valueA, valueB } from 'module.js'
```
