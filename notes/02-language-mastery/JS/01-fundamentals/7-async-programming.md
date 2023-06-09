# ASYNC PROGRAMMING

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)

  - [Promises](#promises)

    - [Definition](#whats-a-promise)
    - [Promise Object](#constructing-a-promise-object)
    - [Consuming promises](#consuming-promises)
    - [Success And Failure](#success-and-failure-callback-functions)
    - [Using Catch](#using-catch-with-promises)
    - [Chaining multiple promises](#chaining-multiple-promises)
    - [Avoid common mistakes](#avoid-common-mistakes)
    - [Promise All](#using-promiseall)

  - [Async-Await](#async-await)
    - [The async keyword](#the-async-keyword)
    - [The await operator](#the-await-operator)
    - [Writing Async functions](#writing-async-functions)
    - [Handling dependant promises](#handling-dependant-promises)
    - [Handling errors](#handling-errors)
    - [Handling independant promises](#handling-independant-promises)
    - [Await Promise all()](#await-promise-all)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

Learn :

## Notes

### `Promises`

#### What's a promise?

**[⬆ back to top](#table-of-contents)**

- Promises are JavaScript objects that represent the eventual result of an asynchronous operation (i.e `time-consuming operations` such as making a `network request` or `querying a database` can be time-consuming).

- Promises can be in one of three states:

`pending` : The initial state— the operation has not completed yet.

`resolved` : The operation has completed successfully and the promise now has a resolved value.(a request’s promise might resolve with a JSON object as its value.)

`rejected` : The operation has failed and the promise has a reason for the failure. This reason is usually an Error of some kind.

- A promise is settled if it is either resolved or rejected.

All promises eventually settle, enabling us to write logic for what to do if the promise fulfills or if it rejects.

#### Constructing a promise object

**[⬆ back to top](#table-of-contents)**

- We construct a promise by using the `new` keyword and passing an executor function to the Promise constructor method.

```js
const executorFunction = (resolve, reject) => {}
const myFirstPromise = new Promise(executorFunction)
```

The executor function generally starts an asynchronous operation and dictates how the promise should be settled.
It has two function parameters, usually referred to as the `resolve()` and `reject()` functions.

`resolve()` : Under the hood, resolve(), will

- change the promise’s status from pending to fulfilled : `pending -> fulfilled`
- the promise’s resolved value will be set to the argument passed into resolve().: `resolve(promiseResolvedValue)`

`reject` : Under the hood, resolve(), will

- change the promise’s status from pending to rejected : `pending -> rejected`
- the promise’s rejection reason will be set to the argument passed into reject() : `resolve(promiseRejectionReason)`

```js
const executorFunction = (resolve, reject) => {
  if (someCondition) {
    resolve('I resolved!')
  } else {
    reject('I rejected!')
  }
}
const myFirstPromise = new Promise(executorFunction)
```

_Technical communication_:

Let’s break down what’s happening above:

- We declare a variable myFirstPromise
- myFirstPromise is constructed using new Promise() which is the Promise constructor method.
- executorFunction() is passed to the constructor and has two functions as parameters: resolve and reject.
- If someCondition evaluates to true, we invoke resolve() with the string 'I resolved!'
- If not, we invoke reject() with the string 'I rejected!'

In our example, `myFirstPromise` resolves or rejects based on a simple condition, but, in practice, promises settle based on the results of asynchronous operations. For example, a database request may fulfill with the data from a query or reject with an error thrown.

#### Consuming promises

**[⬆ back to top](#table-of-contents)**

Knowing how to construct a promise is useful, but most of the time, knowing how to consume, or use, promises will be key. Rather than constructing promises, you’ll be handling Promise objects returned to you as the result of an asynchronous operation.

#### Success and failure callback functions

**[⬆ back to top](#table-of-contents)**

- We use `.then()` with a success handler callback containing the logic for what should happen if a promise resolves.

Promise objects come with an aptly named `.then() method`. It allows us to say, “I have a promise, when it settles, then here’s what I want to happen…

`.then()` is a higher-order function— it takes two callback functions as arguments. We refer to these callbacks as handlers.

The first handler, sometimes called `onFulfilled`, is a success handler, and it should contain the logic for the promise resolving.

The second handler, sometimes called `onRejected`, is a failure handler, and it should contain the logic for the promise rejecting.

We can invoke .then() with one, both, or neither handler!

One important feature of .then() is that it always returns a promise.

To handle a “successful” promise, or a promise that resolved, we invoke .then() on the promise, passing in a success handler callback function:

```js
const prom = new Promise((resolve, reject) => {
  resolve('Yay!')
})

const handleSuccess = (resolvedValue) => {
  console.log(resolvedValue)
}

prom.then(handleSuccess) // Prints: 'Yay!'
```

Let’s break down what’s happening in the example code:

_technical communication_:

- prom is a promise which will resolve to 'Yay!'.
- We define a function, `handleSuccess()`, which prints the argument passed to it.
- We invoke prom‘s `.then()` function passing in our `handleSuccess()` function.
- Since prom resolves, `handleSuccess()` is invoked with prom‘s resolved value, 'Yay', so 'Yay' is logged to the console.

With typical promise consumption, we won’t know whether a promise will resolve or reject, so we’ll need to provide the logic for either case. We can pass both a success callback and a failure callback to .then().

```js
let prom = new Promise((resolve, reject) => {
  let num = Math.random()
  if (num < 0.5) {
    resolve('Yay!')
  } else {
    reject('Ohhh noooo!')
  }
})

const handleSuccess = (resolvedValue) => {
  console.log(resolvedValue)
}

const handleFailure = (rejectionReason) => {
  console.log(rejectionReason)
}

prom.then(handleSuccess, handleFailure)
```

Let’s break down what’s happening in the example code:

- prom is a promise which will randomly either resolve with 'Yay!' or reject with 'Ohhh noooo!'.

- We pass two handler functions to .then(). The first will be invoked with 'Yay!' if the promise resolves, and the second will be invoked with 'Ohhh noooo!' if the promise rejects.

#### Using catch() with promises

**[⬆ back to top](#table-of-contents)**

- We use `.catch()` with a failure handler callback containing the logic for what should happen if a promise rejects.

One way to write cleaner code is to follow a principle called separation of concerns, which means organizing code into distinct sections each handling a specific task.

because `.then()` will return a promise with the same settled value as the promise it was called on, we can separate our resolved logic from our rejected logic. Instead of passing both handlers into one `.then()`, we can chain a second `.then()` with a `failure handler` to a first .then() with a success handler and both cases will be handled.

```js
prom
  .then((resolvedValue) => {
    console.log(resolvedValue)
  })
  .then(null, (rejectionReason) => {
    console.log(rejectionReason)
  })
```

to create even more readable code, we can use a different promise function: `.catch()`.
the `.catch()` function takes only one argument, onRejected. In the case of a rejected promise, this `failure handler` will be invoked with the reason for rejection.

`.catch()` is syntactic sugar for `.then(null, onReject)`.

```js
prom
  .then((resolvedValue) => {
    console.log(resolvedValue)
  })
  .catch((rejectionReason) => {
    console.log(rejectionReason)
  })
```

- prom is a promise which randomly either resolves with 'Yay!' or rejects with 'Ohhh noooo!'.
- We pass a success handler to .then() and a failure handler to .catch().
- If the promise resolves, .then()‘s success handler will be invoked with 'Yay!'.
- If the promise rejects, .then() will return a promise with the same rejection reason as the - original promise and .catch()‘s failure handl

#### Chaining multiple promises

**[⬆ back to top](#table-of-contents)**

- Promise composition enables us to write complex, asynchronous code that’s still readable. We do this by chaining multiple .then()‘s and .catch()‘s.

One common pattern we’ll see with asynchronous programming is multiple operations which depend on each other to execute or that must be executed in a certain order.

We might make one request to a database and use the data returned to us to make another request and so on!

This process of chaining promises together is called `composition`. Promises are designed with composition in mind! Here’s a simple promise chain in code:

```js
firstPromiseFunction()
  .then((firstResolveVal) => {
    return secondPromiseFunction(firstResolveVal)
  })
  .then((secondResolveVal) => {
    console.log(secondResolveVal)
  })
```

_technical communication_:

Let’s break down what’s happening in the example:

- We invoke a function firstPromiseFunction() which returns a promise.
- We invoke .then() with an anonymous function as the success handler.
- Inside the success handler we return a new promise— the result of invoking a second function, secondPromiseFunction() with the first promise’s resolved value.
- We invoke a second .then() to handle the logic for the second promise settling.
- Inside that .then(), we have a success handler which will log the second promise’s resolved value to the console.

In order for our chain to work properly, we had to return the promise `secondPromiseFunction(firstResolveVal)`. This ensured that the return value of the first `.then()` was our second promise rather than the default return of a new promise with the same settled value as the initial.

```js
checkInventory(order)
  .then((resolvedValueArray) => {
    return processPayment(resolvedValueArray)
  })
  .then((resolvedValueArray) => {
    return shipOrder(resolvedValueArray)
  })
  .then((successMessage) => {
    console.log(successMessage)
  })
  .catch((errorMessage) => {
    console.log(errorMessage)
  })
```

#### Avoid common mistakes

**[⬆ back to top](#table-of-contents)**

**TLDR**:

1. To use promise composition correctly, we have to remember to return promises constructed within a .then().

2. We should chain multiple promises rather than nesting them.

Promise composition allows for much more readable code than the nested callback syntax that preceded it. However, it can still be easy to make mistakes. In this exercise, we’ll go over two common mistakes with promise composition.

`Mistake 1: Nesting promises instead of chaining them`

```js
returnsFirstPromise().then((firstResolveVal) => {
  return returnsSecondValue(firstResolveVal).then((secondResolveVal) => {
    console.log(secondResolveVal)
  })
})
```

_technical communication_:

Let’s break down what’s happening in the example:

- We invoke returnsFirstPromise() which returns a promise.
- We invoke .then() with a success handler.
- Inside the success handler, we invoke returnsSecondValue() with firstResolveVal which will return a new promise.
- We invoke a second .then() to handle the logic for the second promise settling all inside the first then()!
- Inside that second .then(), we have a success handler which will log the second promise’s resolved value to the console.

`Mistake 2: Forgetting to return a promise`

```js
returnsFirstPromise()
  .then((firstResolveVal) => {
    returnsSecondValue(firstResolveVal)
  })
  .then((someVal) => {
    console.log(someVal)
  })
```

_technical communication_:

Let’s break down what’s happening in the example:

- We invoke returnsFirstPromise() which returns a promise.
- We invoke .then() with a success handler.
- Inside the success handler, we create our second promise, but we forget to return it!
- We invoke a second .then(). It’s supposed to handle the logic for the second promise, but since we didn’t return, this .then() is invoked on a promise with the same settled value as the original promise!

#### Using promise.all()

**[⬆ back to top](#table-of-contents)**

- To take advantage of concurrency, we can use Promise.all().

`Promise composition` is a great way to handle situations where asynchronous operations depend on each other or execution order matters.

`Promise concurrency` is when we’re dealing with multiple promises, but we don’t care about the order, we can do this with the function `Promise.all()`.

Promise.all() accepts an array of promises as its argument and returns a single promise. That single promise will settle in one of two ways:

- If every promise in the argument array resolves, the single promise returned from Promise.all() will resolve with an array containing the resolve value from each promise in the argument array.

- If any promise from the argument array rejects, the single promise returned from Promise.all() will immediately reject with the reason that promise rejected. This behavior is sometimes referred to as failing fast.

```js
let myPromises = Promise.all([
  returnsPromOne(),
  returnsPromTwo(),
  returnsPromThree(),
])

myPromises
  .then((arrayOfValues) => {
    console.log(arrayOfValues)
  })
  .catch((rejectionReason) => {
    console.log(rejectionReason)
  })
```

_technical communication_:

Let’s break down what’s happening in the example:

- We declare myPromises assigned to invoking Promise.all().
- We invoke `Promise.all()` with an array of three promises— the returned values from functions.
- We invoke `.then()` with a `success handler` which will log the array of resolved values if each promise resolves successfully.
- We invoke `.catch()` with a `failure handler` which will log the first rejection message if any promise rejects.

`The Promise.all() method runs many promises in parallel, waits for all of them to resolve, and returns an array of results as its fulfilled value.`

### `Async-Await`

**[⬆ back to top](#table-of-contents)**

**TLDR**:

- async...await is syntactic sugar built on native JavaScript promises and generators.
- We declare an async function with the keyword async.
- Inside an async function we use the await operator to pause execution of our function until an asynchronous action completes and the awaited promise is no longer pending .
- await returns the resolved value of the awaited promise.
- We can write multiple await statements to produce code that reads like synchronous code.
- We use try...catch statements within our async functions for error handling.
- We should still take advantage of concurrency by writing async functions that allow asynchronous actions to happen in concurrently whenever possible.
  >

JavaScript is non-blocking: instead of stopping the execution of code while it waits, JavaScript uses an `event-loop` which allows it to efficiently **execute other tasks while it awaits the completion of these asynchronous actions.**

JavaScript is continually improving, and `ES8` provides a new syntax for handling our asynchronous action, `async...await`. The `async...await` syntax allows us to write asynchronous code that reads similarly to traditional synchronous, imperative programs.

The `async...await` syntax is `syntactic sugar`— it doesn’t introduce new functionality into the language, but rather introduces a new syntax for using promises and generators.

#### The Async Keyword

it's used to write functions that handle asynchronous actions. We wrap our asynchronous logic inside a function prepended with the async keyword. Then, we invoke that function.

```js
async function myFunc() {
  // Function body here
}

myFunc()
```

`async` functions always return a promise. This means we can use traditional promise syntax, like `.then()` and `.catch` with our async functions. An async function will return in one of three ways:

- If there’s nothing returned from the function, it will return a promise with a resolved value of undefined.
- If there’s a non-promise value returned from the function, it will return a promise resolved to that value.
- If a promise is returned from the function, it will simply return that promise

```js
async function fivePromise() {
  return 5
}

fivePromise().then((resolvedValue) => {
  console.log(resolvedValue)
}) // Prints 5
```

In the example above, even though we return 5 (`a non-promise value, it will return a promise resolved to that value.`) what’s actually returned when we invoke fivePromise() is a promise with a resolved value of 5.

#### The Await operator

`async` functions are almost always used with the additional keyword `await` inside the function body.

The `await` keyword can only be used inside an async function.

`await` is an operator: it returns the resolved value of a promise. Since promises resolve in an indeterminate amount of time, await halts, or pauses, the execution of our `async` function until a given promise is resolved.

In most situations, we’re dealing with promises that were returned from functions. Generally, these functions are through a library.

In the example below, `myPromise()` is a function that returns a promise which will resolve to the string "I am resolved now!".

```js
async function asyncFuncExample() {
  let resolvedValue = await myPromise()
  console.log(resolvedValue)
}

asyncFuncExample() // Prints: I am resolved now!
```

Within our async function, asyncFuncExample(), we use await to halt our execution until `myPromise()` is resolved and assign its resolved value to the variable resolvedValue.
We could have a million `log` after `console.log(resolvedValue)` will execute before all of them.

#### Writing Async functions

```js
async function getBeans() {
  console.log(`1. Heading to the store to buy beans...`)
  let value = await shopForBeans()
  console.log(`3. Great! I'm making ${value} beans for dinner tonight!`)
}

// output
// 1. Heading to the store to buy beans...
// 2. I bought kidney beans..
// 3. Great! I'm making kidney beans for dinner tonight!
```

_technical communication_:

its intended functionality:

- Log '1. Heading to the store to buy beans...' to the console.
- Capture the resolved value of the promise returned when we invoke shopForBeans().
- The promise returned from shopForBeans() prints a string in the format '2. I bought [the resolved type of beans] beans because they were on sale.'
- Finally, the function prints a string in the format '3. Great! I'm making [the bean type] beans for dinner tonight!' to the console.

#### Handling dependant promises

The true beauty of async...await is when we have a series of asynchronous actions which depend on one another. For example, we may make a network request based on a query to a database. In that case, we would need to wait to make the network request until we had the results from the database. With native promise syntax, we use a chain of .then() functions making sure to return correctly each one:

```js
function nativePromiseVersion() {
  returnsFirstPromise()
    .then((firstValue) => {
      console.log(firstValue)
      return returnsSecondPromise(firstValue)
    })
    .then((secondValue) => {
      console.log(secondValue)
    })
}
```

Here’s how we’d write an async function to accomplish the same thing:

```js
async function asyncAwaitVersion() {
  let firstValue = await returnsFirstPromise()
  console.log(firstValue)
  let secondValue = await returnsSecondPromise(firstValue)
  console.log(secondValue)
}
```

Let’s break down what’s happening in our asyncAwaitVersion() function:

- We mark our function as async.
- Inside our function, we create a variable firstValue assigned to invoking await returnsFirstPromise(). This means `firstValue is assigned the resolved value of the awaited promise`.
- Next, we log firstValue to the console.
- Then, we create a variable secondValue assigned to await returnsSecondPromise(firstValue).

- Therefore, secondValue is assigned this promise’s resolved value.
- Finally, we log secondValue to the console.

Though using the `async...await` syntax can save us some typing, the length reduction isn’t the main point. Given the two versions of the function, the `async...await` version more closely resembles synchronous code, which helps developers maintain and debug their code. The `async...await` syntax also makes it easy to `store and refer to resolved values from promises further back in our chain` which is a much more difficult task with native promise syntax.

```js
const { shopForBeans, soakTheBeans, cookTheBeans } = require('./library.js')

async function makeBeans() {
  const type = await shopForBeans()
  const isSoft = await soakTheBeans(type)
  const dinner = await cookTheBeans(isSoft)
  console.log(dinner)
}
makeBeans()
// output
// I bought black beans because they were on sale.
// Time to soak the beans.
// ... The black beans are softened.
// Time to cook the beans.
// ... The beans are cooked!

// Dinner is served!
```

- We require in three functions: shopForBeans(), soakTheBeans(), and cookTheBeans(). These functions each return a promise.

- shopForBeans() expects no arguments and returns a promise which will resolve to a string of a bean type.

- soakTheBeans() expects a bean type string as an argument and returns a promise which resolves to a boolean indicating whether or not the beans are softened.

- cookTheBeans() expects a boolean as an argument and, if that value is true, returns a promise which will resolve to a string announcing that dinner is ready.

#### Handling errors

With `async...await`, we use try...catch statements for error handling. By using this syntax, not only are we able to handle errors in the same way we do with synchronous code, but we can also catch both synchronous and asynchronous errors. This makes for easier debugging!

```js
async function usingTryCatch() {
  try {
    let resolveValue = await asyncFunction('thing that will fail')
    let secondValue = await secondAsyncFunction(resolveValue)
  } catch (err) {
    // Catches any errors in the try block
    console.log(err)
  }
}

usingTryCatch()
```

#### Handling independant promises

What if our async function contains multiple promises which are not dependent on the results of one another to execute?

```js
async function waiting() {
  const firstValue = await firstAsyncThing()
  const secondValue = await secondAsyncThing()
  console.log(firstValue, secondValue)
}

async function concurrent() {
  const firstPromise = firstAsyncThing()
  const secondPromise = secondAsyncThing()
  console.log(await firstPromise, await secondPromise)
}
```

In the `waiting()` function, we pause our function until the first promise resolves, then we construct the second promise. Once that resolves, we print both resolved values to the console.

In our `concurrent()` function, both promises are constructed without using `await`. We then await each of their resolutions to print them to the console.

Within our async functions we should still take advantage of concurrency, the ability to perform asynchronous actions at the same time.

#### Await Promise all()

Another way to take advantage of concurrency when we have multiple promises which can be executed simultaneously is to await a `Promise.all()`.

We can pass an array of promises as the argument to Promise.all(), and it will return a single promise. This promise will resolve when all of the promises in the argument array have resolved. This promise’s resolve value will be an array containing the resolved values of each promise from the argument array.

```js
async function asyncPromAll() {
  const resultArray = await Promise.all([
    asyncTask1(),
    asyncTask2(),
    asyncTask3(),
    asyncTask4(),
  ])
  for (let i = 0; i < resultArray.length; i++) {
    console.log(resultArray[i])
  }
}
```

In our above example, we await the resolution of a Promise.all(). This Promise.all() was invoked with an argument array containing four promises (returned from required-in functions). Next, we loop through our resultArray, and log each item to the console. The first element in resultArray is the resolved value of the asyncTask1() promise, the second is the value of the asyncTask2() promise, and so on.

Promise.all() allows us to take advantage of asynchronicity— each of the four asynchronous tasks can process concurrently.

Promise.all() also has the benefit of failing fast, meaning it won’t wait for the rest of the asynchronous actions to complete once any one has rejected. As soon as the first promise in the array rejects, the promise returned from Promise.all() will reject with that reason. As it was when working with native promises, Promise.all() is a good choice if multiple asynchronous tasks are all required, but none must wait for any other before executing.
