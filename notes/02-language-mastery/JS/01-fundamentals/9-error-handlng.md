# ERRORS

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
  - [Thinking about errors differently](#thinking-about-errors-differently)
    - [Errors in Your Code Mean You’re Trying To Do Something Cool](#errors-in-your-code-mean-youre-trying-to-do-something-cool)
    - [Tools to tackle errors](#tools-to-tackle-errors)
      - [Dissect the Error](#dissect-the-error)
      - [Ask Yourself, Is the Solution In the Error](#ask-yourself-is-the-solution-in-the-error)
      - [Search on the Web for Instances of This Error](#search-on-the-web-for-instances-of-this-error)
      - [Compare Different Use Cases to Yours](#compare-different-use-cases-to-yours)
      - [Try to implement the solution](#try-to-implement-the-solution)
      - [If It Doesn’t Work, Repeat Steps 2-4.](#if-it-doesnt-work-repeat-steps-2-4)
  - [Debugging JS code](#debugging-js-code)
    - [Error Stack Traces](#error-stack-traces)
    - [Reading Error Stack Traces](#reading-error-stack-traces)
    - [JavaScript Error Types](#javascript-error-types)
    - [Debugging Errors](#debugging-errors)
  - [Handling Errors](#error-handling)
    - [Runtime errors](#runtime-errors)
    - [Constructing an Error](#constructing-an-error)
    - [Throw an error](#the-throw-keyword)
    - [The try...catch Statement](#the-trycatch-statement)
    - [Handling with try...catch](#handling-with-trycatch)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Understand how web browsers communicate via HTTP.
- Create calls to various APIs using requests.
- Make asynchronous requests using async/await syntax.

## Notes

### Thinking about errors differently

**[⬆ back to top](#table-of-contents)**

**Learning goal**:

- How errors in your code aren’t a bad thing, but rather an opportunity to learn.
- How to identify and fix these errors?

It’s important to understand that making and debugging errors is part of the process and you won’t always know the solution right away.

#### Errors in Your Code Mean You’re Trying To Do Something Cool

**[⬆ back to top](#table-of-contents)**

![Error graph](/notes/02-language-mastery/assets/error-handling.webp)

As your code increases in complexity, the number of errors you’ll encounter rises at a similar rate. An error means you’re trying to do something that might be a little complicated (or very complicated), and it doesn’t quite work yet, but by no means is it a sign that you should stop trying!

In fact, there are entire engineering roles built around finding and fixing errors. A `site reliability engineer` finds and report errors in web platforms. A `test engineer` builds automated tests to discover errors in software and make sure that it meets a companies standards.

Bugs show you where the weaknesses are, make you really consider what you want your code to accomplish, and then guide you towards building more reliable and secure products.

#### Tools to tackle errors

**[⬆ back to top](#table-of-contents)**

Being willing to tackle an error is a great first step, and having some tools at your disposal to debug is a great way to feel comfortable tackling even the gnarliest of bugs in the future.

Let’s outline a few steps you can take to solve any compiler errors that you might encounter – these types of errors print out to the console.

##### Dissect the Error

**[⬆ back to top](#table-of-contents)**

When an error first appears on your screen, find the line in the error specific to your code.
You want to find the part of the error message that gives you insight about what happened. `That line is generally at the top of the error stack trace`.

let's look at an example:
Given the list, you might want to filter for all of the honor roll students by looking at the gpa property.

```js
const GPA_BENCHMARK = 3.5
let students = {
  1: {
    name: 'Egill Vignission',
    gpa: 3.4,
  },
  2: {
    name: 'Bianca Pargas',
    gpa: 3.8,
  },
  3: {
    name: "Aisling O'Sullivan",
    gpa: 3.4,
  },
  4: {
    name: 'Sameer Fares',
    gpa: 3.9,
  },
}

let honorRoll = students.filter((student) => {
  return student.gpa >= GPA_BENCHMARK
})

console.log(honorRoll)
```

After running that code, a stack trace that contains a lot of error information will appear:

```sh
TypeError: students.filter is not a function
    at /home/runner/FearlessNewDev/index.js:21:26
    at Script.runInContext (vm.js:130:18)
    at Object.<anonymous> (/run_dir/interp.js:209:20)
    at Module._compile (internal/modules/cjs/loader.js:999:30)
    at Object.Module._extensions..js (internal/modules/cjs/loader.js:1027:10)
    at Module.load (internal/modules/cjs/loader.js:863:32)
    at Function.Module._load (internal/modules/cjs/loader.js:708:14)
    at Function.executeUserEntryPoint [as runMain] (internal/modules/run_main.js:60:12)
    at internal/main/run_main_module.js:17:47
```

So, which line has the most important information? The first two lines seem pretty promising.

```sh
TypeError: students.filter is not a function
    at /home/runner/FearlessNewDev/index.js:21:26
```

- The first line says that the JavaScript engine doesn’t think that . `filter()` is a function — but it should be, so there might be an issue in `how it’s being used`

- The second line points to the line of code that is throwing the error, which is line 21 and at the 26th character: `let honorRoll = students.filter(student => {....`

The code is calling `.filter()` on the `students` variable. Checking back in on the code, you can see that `students is an object`. This seems like a good place to start.

##### Ask Yourself, Is the Solution In the Error?

**[⬆ back to top](#table-of-contents)**

Often, you’ll encounter syntax errors that will show exactly where the error occurred and what the error was. When you get these types of errors, you can go directly back to your code and fix them. Here’s an example of code that will throw a syntax error:

```js
for (let x = 0; x < 10, x++) {
  console.log(x)
}
```

Running this code will output:

```sh
/home/runner/FearlessNewDev/index.js:1
for (let x = 0; x < 10, x++) {
                           ^

SyntaxError: Unexpected token ')'
```

Notice that in this case, the compiler is pointing to the syntax issue with the `^` symbol, which sometimes makes it easier to debug. However, the issue is that there’s accidentally a comma `(,)` instead of a semicolon `(;)`. Error messages can help, but you still need to use your developer knowledge to fix the error!

##### Search on the Web for Instances of This Error

**[⬆ back to top](#table-of-contents)**

Copy and paste the important part of the error message into a search engine and look through the results until you find someone else who has also run into that issue. To get better search results, you can include relevant keywords and modify the error message to be more generic.

In the error message `TypeError: students.filter is not a function`, it’s highly unlikely that someone else is having a `.filter()` issue on a students variable with the same data types, so that’s the type of thing that could be more generic.

Instead of searching for “TypeError: students.filter is not a function”, you can try something like “TypeError: `object` filter() is not a function” and add a keyword for the language. The final search might be: `“TypeError: object filter() is not a function JavaScript”`.

After searching, one of the first results looks promising: `“filter is a method on arrays. Since the code you posted contains an object, you’re seeing this error. You may want to apply filter after…”`

Aha! If you consult the `.filter()` documentation on MDN Web Docs you can confirm that `.filter()` is only for arrays. On the Stack Overflow page, it recommends using `Object.keys(data)` to turn the object into an iterable array so you can use the `.filter()` Array method on the new array.

Google is always a good place to check, but you can also reread documentation on MDN Web Docs or search directly on Stack Overflow, which is a wonderful community of programmers sharing knowledge and building cool stuff.

##### Compare Different Use Cases to Yours

**[⬆ back to top](#table-of-contents)**

Often you will not find someone who was trying to do the exact same thing you were trying to do, but who still encountered the same error. Read through their code a bit and see if it is comparable to yours. Even if their code is wildly different, the one or two lines that threw the error might be very similar to your code, so the solution may end up being the same.

Consider the result found for the TypeError.

![Stackoverflow](/notes/02-language-mastery/assets/typeerror_stackoverflow.webp)

Even though the solution didn’t involve a students object, the answer does provide a quick way to make an Object work with the .filter() method. It’s worth a shot to try it!

##### Try to implement the solution

**[⬆ back to top](#table-of-contents)**

Every solution you implement is a new tool you can add to your programmer’s toolbox, and another error you will know how to solve in the future.

Thankfully, 31piy‘s solution was able to solve the issue using the `Object.values()` method. Take a look at the updated code:

```js
let honorRoll = Object.values(students).filter((student) => {
  return student.gpa >= GPA_BENCHMARK
})
console.log(honorRoll)
```

After reading through the documentation and a few more Stack Overflow responses, it also seemed like a good idea to try changing my code in a different way, by making the students object be an array instead of an object in the first place. That way the `.filter()` method can be used directly on the array instead of first turning an object into an array.

Take a look at the updated code to reflect that change:

```js
const GPA_BENCHMARK = 3.5
let students = [
  {
    name: 'Egill Vignission',
    gpa: 3.4,
  },
  {
    name: 'Bianca Pargas',
    gpa: 3.8,
  },
  {
    name: "Aisling O'Sullivan",
    gpa: 3.4,
  },
  {
    name: 'Sameer Fares',
    gpa: 3.9,
  },
]
let honorRoll = students.filter((student) => {
  return student.gpa >= GPA_BENCHMARK
})
console.log(honorRoll)
```

In the process of figuring out this compiler error, you were able to learn how to use Array methods on objects, and how to think about your use of data types. See, every error is an opportunity to learn

##### If It Doesn’t Work, Repeat Steps 2-4

**[⬆ back to top](#table-of-contents)**

Keep searching through Google and Stack Overflow. The answer will be there! Sometimes it’s helpful to Google parts of the error message, use `console.log()` to see the outputs, or to read about methods and data types at the website MDN Web Docs.

The solutions to your errors are out there, and the process of finding them will make you a stronger and more confident programmer. As you grow and learn, expect to encounter countless errors, and expect each one to be its own unique learning opportunity.

##### Ask The Question Yourself

**[⬆ back to top](#table-of-contents)**

Okay, maybe the answer isn’t out there… yet. There is a wealth of information out on the web, but you might be the first one doing something completely new and shiny. It’s ok to ask for help. On the same sites and forums that you found help from, post a question. Ideally, you supply all the background information and provide relevant code that others can look at and do their best to help you out.

### Debugging JS code

**[⬆ back to top](#table-of-contents)**

**TLDR**:

- Is your code throwing errors? If so, read the error stack trace for the type, description, and location of the error. Go to the error’s location and try to fix.

- Is your code broken but not throwing errors? Walk through your code using console.log() statements. When unexpected results occur, isolate the bug and try to fix it.

- Did you locate the bug using steps 1 and 2, but can’t fix the bug? Consult documentation to make sure you are using all JavaScript functionality properly. If you are still stuck, Google your issue and consult Stack Overflow for help. Read solutions or post your own Stack Overflow question if none exist on the topic.

**Learning goal**:

- demonstrating how to implement those skills in real-life JavaScript situations.

#### Error Stack Traces

**[⬆ back to top](#table-of-contents)**

![Stack Traces](/notes/02-language-mastery/assets/js-error-diagram.svg)

A piece of software, called a compiler, is trying to translate your code so that your computer can understand and run it.

However, the compiler is coming across a piece of code that it can’t interpret. As a result, it throws an error back to you to let you know that it has to stop and why.

this information is logged as an error stack trace — a printed message containing information about

- where the error occurred.
- what type of error was thrown.
- and a description of the error.

#### Reading Error Stack Traces

**[⬆ back to top](#table-of-contents)**

```sh
/home/ccuser/workspace/learn-javascript-debugging-code/app.js:1
myVariable;
^

ReferenceError: myVariable is not defined
...
```

Using this stack trace, let’s answer three questions you should ask yourself every time you want to debug an error:

- In what line did the error occur? You can almost always find this information on the first line of the stack trace in the following format `<file path>/<file name>:<line number>`. In this example, the location is `app.js:1`. This means that the error was thrown in the file named app.js on line 1

- What type of error was thrown? The first word on the fifth line of the stack trace is the type of error that was thrown. In this case, the type of error was ReferenceError. We will discuss this error type in the next exercise.

- What is the error message? The rest of the fifth line after the error type provides an error message, describing what went wrong. In this case, the description is myVariable is not defined.

#### JavaScript Error Types

**[⬆ back to top](#table-of-contents)**

there are three common error types:

- `SyntaxError` : This error will be thrown when a typo creates invalid code — code that cannot be interpreted by the compiler. scan your code to make sure you properly opened and closed all brackets, braces, and parentheses and that you didn’t include any invalid semicolons.

- `ReferenceError`: This error will be thrown if you try to use a variable that does not exist. When this error is thrown, make sure all variables are properly declared.

- `TypeError`: This error will be thrown if you attempt to perform an operation on a value of the wrong type. For example, if we tried to use a string method on a number, it would throw a TypeError.

#### Debugging Errors

**[⬆ back to top](#table-of-contents)**

Here’s a process for efficiently working through your code’s errors one by one:

- Run your code. Using the first error’s stack trace, identify the error’s type, description, and location.
- Go to the file name and line number indicated by the error stack trace. Using the error type and description, identify the bug in your code.
- Fix the bug and re-run your code.
- Repeat steps 1-3 until your code no longer throws any errors.

#### Locating Silent Bugs

**[⬆ back to top](#table-of-contents)**

Errors thrown by the computer are really useful because :

- they identify the bug type
- and location for you right away.

However, even if your code runs error-free, it is not necessarily bug-free.

You may find that your code is consistently returning incorrect values without throwing any errors. A lack of thrown errors does not mean your code logic is completely correct.

An incredibly powerful tool for locating bugs is a method you likely learned very early on in your JavaScript journey: console.log()!

By adding print statements to our code, we can identify where things have gone wrong.

### Error Handling

**[⬆ back to top](#table-of-contents)**

**Learning goal**:

- How mistakes in programming leads to errors.

- Why errors are useful for developers.

- Errors will prevent a program from executing unless it is handled.

- How to create an error using the Error() function.

- How to throw an error object using the throw keyword.

- How to use the try...catch statement to handle thrown errors.

- Evaluating code in a try block to anticipate errors.

- Catching the error in a catch block to allow our program to continue running.

- Why the try...catch statement would be useful in a program.

**[⬆ back to top](#table-of-contents)**

Sometimes, we’ve written code that successfully returns a value but a different value from what we expected. Our program continues running, and we might not even realize anything went wrong until much later. It’s like making soup and accidentally adding sugar instead of salt. In the end we still have soup, but it might not be soup that we want to eat. We will not be focusing on these mistakes.

Rather, we’re going to focus on the errors that pop up when we’ve written code that causes our program to stop running, e.g. trying to reassign a `const` variable.

For example, what if we tried to move our soup to the table but dropped it because it was too hot? Then our soup-making process is over— there would be no soup.

We can’t always stop errors before they occur, but we can include a backup plan in our program to anticipate and respond to the errors to ensure that our program continues running. `Error handling is the process of programmatically anticipating and addressing errors`.

. In JavaScript, we handle errors using the keywords `try and catch`. We try to move the soup to the table, making sure there’s someone or something nearby to catch the soup in case we drop it.

### Runtime Errors

**[⬆ back to top](#table-of-contents)**

When we execute code and a line of code throws an error, that error is referred to as a `runtime error`.

In JavaScript, there are built-in error objects that have a `name` and `message` property which tell us what went wrong. Examples of built-in runtime errors include:

`ReferenceError`: when a variable or function cannot be found.

`TypeError`: when a value is not a valid type.

### Constructing an Error

**[⬆ back to top](#table-of-contents)**

But, what if we need an error message that isn’t covered by the built-in errors? Let’s say we need to inform a user that a string passed in as an argument is too short with a custom message. Such a message isn’t covered by a built-in error, but we could use the Error function to make our own error object with a message that is unique to our program!

```js
console.log(Error('Your password is too weak.'))
// Prints: Error: Your password is too weak.
```

The `Error` function takes an argument of a string which becomes the value of the error’s message property. In the code snippet above, we used the Error function to create an error object that has the message 'Your password is too weak.'

You might also see errors created with the new keyword. Both methods will lead to the same functionality. Take a look:

```js
console.log(new Error('Your password is too weak.'))
// Prints: Error: Your password is too weak.
```

Keep in mind that creating an error is not the same as throwing an error. `A thrown error will cause the program to stop running`.

### The throw Keyword

**[⬆ back to top](#table-of-contents)**

Creating an error doesn’t cause our program to stop — remember, an error must be thrown for it to halt the program.

To throw an error in JavaScript, we use the throw keyword like so:

```js
throw Error('Something wrong happened')
// Error: Something wrong happened
```

When we use the throw keyword:

- `the error is thrown and code after the throw statement will not execute`

  Take for example:

```js
throw Error('Something wrong happened')
// Error: Something wrong happened

console.log('This will never run')
```

After throw Error('Something wrong happened'); is executed and the error object is thrown, the console.log() statement will not run (just like when a built-in JavaScript error was thrown!).

### The try...catch Statement

**[⬆ back to top](#table-of-contents)**

thrown errors have caused our program to stop running. But, we have the ability to anticipate and handle these errors by writing code to address the error and allow our program to continue running.

In JavaScript, we use try...catch statement to anticipate and handle errors.

```js
try {
  throw Error('This error will get caught')
} catch (e) {
  console.log(e)
}
// Prints: This error will get caught

console.log('The thrown error that was caught in the try...catch statement!')
// Prints: 'The thrown error that was caught in the try...catch statement!
```

_technical communication_:

- We have code that follows try inside curly braces {} known as the try block.
- Inside the try block we insert code that we anticipate might throw an error.
- Since we want to see what happens if an error is thrown in the try block, we throw an error with the message 'This error will get caught'
- Following the try block is the catch statement which accepts the thrown error from the try block . The e represents the thrown error.
- he curly braces that follow catch(e) is known as the catch block and `contains code that executes to handle the error`.
- Since the error is caught, our console.log() after the try...catch statement prints 'The thrown error that was caught in the try...catch statement!'

Generally speaking, in a try...catch statement:

- `we evaluate code in the try block`

- and if the code throws an error, `the code inside the catch block will handle the error for us`.

### Handling with try...catch

**[⬆ back to top](#table-of-contents)**

we can also use a try...catch statement to handle built-in errors that are thrown by the JavaScript engine that is reading and evaluating our code.

```js
const someVar = 'Cannot be reassigned'
try {
  someVar = 'Still going to try'
} catch (e) {
  console.log(e)
}
// Prints: TypeError: Assignment to constant variable.
```
