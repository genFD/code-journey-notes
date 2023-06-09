# Key Takeaways

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Arrays](#arrays)
  - [Loops](#loops)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

## Notes

## Arrays

**[⬆ back to top](#table-of-contents)**

- Arrays are lists that store data in JavaScript.

- Each item inside of an array is at a numbered position, or index, starting at 0.

- We can access one item in an array using its index, with syntax like: `myArray[0].`

```js
const cities = ['Chicago', 'San Francisco', 'New York']

console.log(cities[3])
// output : There’s no element currently in index 3, so cities[3] returns undefined.
```

- We can also change an item in an array using its index, with syntax like myArray[0] = 'new string';

```js
const fruits = ['Apples', 'Oranges', 'Pears', 'Mangos']
fruits[2] = 'Bananas'
console.log(fruits)
// output: ['Apples', 'Oranges', 'Bananas', 'Mangos']
```

- Variables declared with the `const` keyword cannot be reassigned. However, elements in an array declared with const remain mutable. We can change the contents of a const array, but cannot reassign a new array or a different value.

- Arrays have many methods that perform different tasks, such as `.slice()` and `.shift()`, `.push()` and `.pop()` etc..

```js
const countries = ['Japan', 'Denmark', 'Mexico', 'Morocco']
countries.shift()
console.log(countries) // output : ['Denmark', 'Mexico', 'Morocco']
countries = ['England', 'Mozambique', 'Cambodia', 'Peru']
console.log(countries) // output : TypeError: assignment to constant variable
```

```js
const myArray = ['item 0', 'item 1', 'item 2']

myArray.push('item 3')
myArray.pop()

console.log(myArray) // output : ['item 0', 'item 1', 'item 2']
```

## Loops

**[⬆ back to top](#table-of-contents)**

- it's a programming tool that repeats a set of `instructions` until a specified condition, called a `stopping condition` is reached.

- In situations when we want a loop to execute an undetermined number of times, `while loops` are the best choice

- In situations when we want a loop to execute a determined number of times, `for loops` are the best choice
