# DOM EVENTS WITH JAVASCRIPT

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
- [What is an Event?](#what-is-an-event)
- ["Firing" Events](#firing-events)
- [Event Handler Registration](#event-handler-registration)
- [Adding Event Handlers](#adding-event-handlers)
- [Removing Event Handlers](#removing-event-handlers)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

## Notes

_TLDR_:

- You can `register events to DOM elements` using the `addEventListener()` method.

- The addEventListener() method `takes two arguments`: an `event type` and an `event handler function`.

- When an event is triggered on the event target, the `registered event handler function executes`.

- Event handler functions can also be `registered as values` of onevent properties of their event target.

- Event object properties like `.target`, `.type`, and .timeStamp are `used to provide information about the event`.

- The addEventListener() method can be used to `add multiple event handler functions to a single event`.

- The removeEventListener() method `stops specific event handlers from “listening” for specific events firing`.

### What is an Event?

**[⬆ back to top](#table-of-contents)**

Events on the web are `user interactions and browser manipulations that you can program to trigger functionality`.

Some other examples of events are:

- A mouse clicking on a button
- Webpage files loading in the browser
- A user swiping right on an image

When a user does any of the above actions, they’re causing the event to be fired or be triggered. Being able to respond to these events makes your website interactive and therefore dynamic.

### "Firing" Events

**[⬆ back to top](#table-of-contents)**

![Events](/notes/02-language-mastery/assets/event-illustr.svg)

After a specific event fires on a specific element in the document object model (or DOM), an event handler function can be created to run as a response.

### Event Handler Registration

**[⬆ back to top](#table-of-contents)**

Using the .addEventListener() method :

- we can have a DOM element listen for a specific event
- execute a block of code when the event is detected.

The DOM element that listens for an event is called `the event target` and the block of code that runs when the event happens is called `the event handler`.
Let’s take a look at the code below:

```js
let eventTarget = document.getElementById('targetElement')

eventTarget.addEventListener('click', function () {
  // this block of code will run when click event happens on eventTarget element
})
```

_technical communication_:

- We selected our event target from the DOM using `document.getElementById('targetElement')`.
- We used the `.addEventListener()` method on the eventTarget DOM element.
- The `.addEventListener()` method takes two arguments:

1. an event name in string format
2. and an event handler function.

We will learn about different values we can use as event names in a later lesson

- In this example, we used the `'click' event`, which fires when the user clicks on eventTarget.

- The code block in the event handler function will execute when the 'click' event is detected.Instead of using an anonymous function as the event handler, it’s best practice to create a named event handler function. Why? Your code will `remain organized` and `reusable` this way, even if your code gets complex.

```js
function eventHandlerFunction() {
  // this block of code will run when click event happens
}

eventTarget.addEventListener('click', eventHandlerFunction)
```

The named function `eventHandlerFunction` is passed as the second argument of the .`addEventListener()` method instead of defining an anonymous function within the method!

### Adding Event Handlers

**[⬆ back to top](#table-of-contents)**

We looked at registering event handlers using the .addEventListener() method, but there is also another way!

Event Handlers can also be registered by `setting an .onevent property on a DOM element (event target).` The pattern for registering a specific event is to append an element with `.on` followed by the lowercased event type name.

```js
eventTarget.onclick = eventHandlerFunction
```

we’ll be using the .addEventListener() syntax, but we wanted to also introduce the .onevent syntax because both are widely used.

### Removing Event Handlers

**[⬆ back to top](#table-of-contents)**

The `.removeEventListener()` method is used to reverse the .`addEventListener()` method.
This method `stops the event target from “listening” for an event to fire when it no longer needs to.`

.removeEventListener() also takes two arguments:

1. The event type as a string
2. The event handler function

Check out the syntax of a `.removeEventListener()` method with a click event:

```js
eventTarget.removeEventListener('click', eventHandlerFunction)
```

Because there can be multiple event handler functions associated with a particular event, .removeEventListener() needs both the exact event type name and the name of the event handler you want to remove.
