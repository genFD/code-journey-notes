# OOP

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

## Notes

- why is this important?

Because it's a design pattern for structuring complex code
When an application has thousands lines of complex code, it's not only difficult to debug but it's also unreadable, OOP gives us some sort of structure

In Short, the idea is to put together your data and your functionnality in one place.

In JS particularly it relies on a feature called the `prototype chain`

We're going to

- try and understand what is the `prototype chain`
- we're also going to see the `new` keyword,
- we're going to see classes etc...
  I'm excited about this one, let's get started

We going to approach this with a problem and we're going to refactor the solution many times until we reached the final version.

**PROBLEM** : Suppose we have an APP where we want to to able to do two things:

- `Store each user in the app with their respective data` :

user 1
name : 'x'
score : 3

user 2
name : 'y'
score : 5

- `have the the ability to increment the score of each user`

### FIRST SOLUTION

We could use an object, we would have an object with 3 properties :

1. name
2. score
3. and the function to increment the score

```js
const user1 = {
  name: 'X',
  score: 3,
  increment: function () {
    user1.score++
  },
}
```

so that if we want to increment the score we could do something like `user1.increment()`

if we check in the browser we can see the score went from 3 to 4
console.log(user1.score)

### SECOND SOLUTION

The first solution is nice and simple. But imagine we had 1000 users to keep track of , doing the same thing 1000 time is going to quickly become tedious.
We need to find an efficient way of solving this problem.
In Js when the code is becoming repetitive, you can wrap it in function, save it once and use the function again and again, the only thing you want to change is the input. This is exactly what we're going to do:

- We creating a function `userCreator` that takes two parameters `name` and `score` our user's data
- Inside the body of the function we creating an object with the properties we need name, score and increment and we returning that object

```js
function userCreator(name, score) {
  const newUser = {}
  newUser.name = name
  newUser.score = score
  newUser.increment = function () {
    newUser.score++
  }
  return newUser
}
```

So creating a new user becomes easier we just have to call the function with the new user's `name` and `score`

```js
const userA = userCreator('A', 3)

console.log('userA', userA)

// we can also increment the user's score'
userA.increment()

console.log('userA', userA)
```

This approach worked, but there is a problem here.
**Note**:
The problem is if we have a `1000 users`, yes the score and the name is going to be different for each users but the function increment doesn't change. With a `1000 users` we're going to have 1000 function increment, which is not ideal we want our code to be efficient and perfomant, having a 1000 times the same function is not optimal.

Is there a better way?

### THIRD SOLUTION

Intuitively,
another solution is to have a standalone increment function, store it in an object for example so that JS knows that when it doesn't find increment on our user's object it can still look up this object, find the increment function and use it. In short, That's the idea of the prototype chain.

Practically it's going to look like this:

- We're using Object.create which is a built in function to create a new object, we passing in userFunctionStore which is another object which contains our standalone increment function. and because we're using this technique, to create a new user, the new user object that we return from the function userCreator is going to somehow have a link to userFunctionStore and therefore to increment

```js
function userCreator(name, score) {
  const newUser = Object.create(userFunctionStore)
  newUser.name = name
  newUser.score = score
  return newUser
}

const userFunctionStore = {
  increment: function () {
    this.score++
  },
}
```

So we can create our users like so :

```js
const user1 = userCreator('user1', 3)
const user2 = userCreator('user2', 5)

console.log(user1)
```

But when we console.log user1 we cannot see increment on user1, what's happening is behind the scenes and the newUser has hidden property **proto** which has a link to userFunctionStore. so when we create user1 the link to userFunctionStore is still there, and we can see that in the browser, if I console.log
console.log(user1.**proto**) deprecated
console.log(Object.getPrototypeOf(user1))
we can see the link to that function which is represented by this arrow here

Js has this prototypal feature that means when if it does not find a given property on an object, it goes straight to the proto property and looks to what it's linking to in the prototype chain.

for ex : user 1 and user2 don't have an individual copy of the increment function, instead we have through the hidden proto property a link up to a single object where the function increment is defined

!--------!##### NEXT VIDEO ####!-------!

Another thing is it turns out that all objects in JS have have a link to big object called Object.prototype, it has a lot of helper functions like hasOwnProperty, valueOF() such that when I run

console.log(userFunctionStore.hasOwnProperty('increment'))
it returns true

!--------!##### NEXT VIDEO ####!-------!

if we take a look at function increment body, we have this.score ++.
So here's the problem the increment function needs to work on user 1, 2, 3 or 1000, so somehow we need to reference these objects in a general way.
the solution to this problem is to use what it's called an implicit parameter, that means a parameter that we don't need to specify, it's automatically there for me to use and that's why we use this here. so in this context that is when we do user1.increment(), this is a placeholder for the object on the left hand side so this evaluates to user1. if it was user2.increment it was going to be user2 etc...

Another thing is let's imagine we wrap this.score ++ in a function call add like this and we call increment

const userFunctionStore = {
increment: function () {
function add() {
this.score++
}
add()
},
}

const user3 = userCreator('user3', 4)
user3.increment()
user3.increment()
user3.increment()
user3.increment()
user3.increment()

we notice in the browser that the score did not change.
and that's because when we use function declaration to wrap a functionnality that uses the this keyword, it defaults to the global window object which is not the object that we care about here
the solution is to use the arrow function, the arrow function is lexically scoped

const userFunctionStore = {
increment: function () {
const add = () => {
this.score++
}
add()
},
}

const user3 = userCreator('user3', 4)
user3.increment()
user3.increment()
user3.increment()
user3.increment()
user3.increment()

console.log(user3)

!--------!##### NEXT VIDEO ####!-------!

############# FOURTH SOLUTION ################
Back to the SOLUTION of our problem, we saw that with Object.Create we were able to link to a single object that contains the function increment. For our newly created user, we did not need to create an indvidual copy of the increment function, we were able to reference that function via the hidden proto property which has link to the object where increment is defined.

This technique is nice and easy to understand but there is an even better way of achieving the same result.
with this technique we won't have
/\*\*

- to create a new object manually using Object.create()
- creating a link to userFunctionStore
- Passing the parameters name and score as value of newUser.name, newUser.score
- returning that object manually
  \*/

we can achieve all this by using the new Keyword
the new keyword behind the scenes

/\*\*

- to create a new object
- create a link to some object full of functions
- set the arguments that we pass in to the function preceded by new keyword as value of newly created object properties
- returning that object automatically
  \*/

function userCreator(name, score) {
const newUser = Object.create(functionStore)
newUser.name = name
newUser.score = score
this.name = name
this.score = score
return newUser
}
functionStore
userCreator.prototype {};
functionStore
userCreator.prototype.increment = function () {
this.score++
}
/ const user1 = new userCreator('Will', 3)

this refers to auto-create object

and to store the single version of the function that we want all objects created with userCreator we use the property .prototype which exists on every functions. because functions are also objects and they automatically have a property called prototype which is just an empty object, we can store in that object the increment function such that

when we create a new user by calling userCreator() and then we increment the score of that user, the increment function will not be on our newly created user but through the hidden proto property, we're no longer going to have a link up to the userFunctionStore object but a link to the prototype object :

function userCreator(name, score) {
this.name = name
this.score = score
}
userCreator.prototype.increment = function () {
this.score++
}
userCreator.prototype.login = function () {
console.log('login')
}
const user1 = new userCreator('user1', 9)
user1.increment()
