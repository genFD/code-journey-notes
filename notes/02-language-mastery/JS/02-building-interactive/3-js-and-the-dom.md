# JAVASCRIPT AND THE DOM

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)
- [The Document Keyword](#the-document-keyword)
- [Tweak an Element](#tweak-an-element)
- [Select and Modify Elements](#select-and-modify-elements)
- [Style an Element](#style-an-element)
- [Traversing the DOM](#traversing-the-dom)
- [Create and Insert Elements](#create-and-insert-elements)
- [Remove an Element](#remove-an-element)
- [Add Click Interactivity](#add-click-interactivity)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

## Notes

_TLDR_:

- The document keyword `grants access to the root of the DOM in JavaScript`.

- The `DOM Interface` allows you to `select a specific element with CSS selectors by using the .querySelector() method.`

- You can access an element directly by its `ID` with the `.getElementById()` method which `returns a single element`.

- You can access an `array of elements` with the `.getElementsByClassName`() and `.getElementsByTagName()` methods, then call a single element by referencing its placement in the array.

- The `.innerHTML` and `.style` properties allow you to `modify an element by changing its contents or style respectively`.

- You can create, append, and remove elements by using the `.createElement()`,`.appendChild()` and `.removeChild()` methods respectively.

- The `.onclick` property can `add interactivity` to a DOM element based on a `click event`.

- The `.children` property `returns a list of an element’s children` and the `.parentNode` property `returns the element’s closest connected node` in the direction towards the root

### The Document Keyword

**[⬆ back to top](#table-of-contents)**

This lesson will focus on some of the most useful methods and properties of the DOM interface in JavaScript.

The `document` object in JavaScript is the door to the DOM structure.
Before you can access a specific element in the page, first you must access the document structure itself. The document object allows scripts to access children of the DOM as properties.

For example, if you want to access the `<body>` element from your script, you can access it as a property of the document object by using `document.body`. This property will `return the body element of that DOM`.

Similarly, you could access the `<title>` element with the `.title property`.
Here is a ![comprehensive list ](https://developer.mozilla.org/en-US/docs/Web/API/Document) of all document properties.

![Diagram](/notes/02-language-mastery/assets/domTreeEx2.svg)

The diagram illustrates that the document keyword points to the root node of the Document Object Model (DOM). The document.body and document.head properties act as though you were directly accessing the html DOM element

### Tweak an Element

**[⬆ back to top](#table-of-contents)**

When using the DOM in your script to access an HTML element, `you also have access to all of that element’s properties.`
This includes the ability to

- modify the contents of the element
- as well as its attributes and properties, which can range from modifying the text inside a `<p>` element to assigning a new background color to a `<div>`

For example, the `.innerHTML` property allows you to `access and set the contents of an element`

Let’s take a look at how we can `reassign the contents` of the `<body>` element to the text 'The cat loves the dog':

```js
document.body.innerHTML = 'The cat loves the dog.'
```

The `.innerHTML` property can also `add any valid HTML elements`. The following example replaces the contents of the `<body>` element by assigning an `<h2>` element as a child inside the `<body>` element:

```js
document.body.innerHTML = '<h2>This is a heading</h2>'
```

### Select and Modify Elements

**[⬆ back to top](#table-of-contents)**

In the previous exercise, we accessed the `<body>` element with the document keyword!
What if we wanted to select a specific element besides the entire `<body>` element?
The DOM interface allows us to access a specific element with CSS selectors.

`CSS selectors` define the elements to which a set of CSS rules apply, but we can also use these same selectors to access DOM elements with JavaScript! Selectors can include a tag name, a class, or an ID.

The `.querySelector()` method allows us to `specify a CSS selector as a string and returns the first element that matches that selector`. The following code would return the first paragraph in the document.

```js
document.querySelector('p')
```

Along with `.querySelector()`, JavaScript has more targeted methods that select elements based on their class, id, or tag name.

```js
document.getElementById('bio').innerHTML = 'The description'
```

In this example, we’ve selected the element with an ID of 'bio' and set its `.innerHTML` to the text 'The description'. Notice that the ID is passed as a string, wrapped in quotation marks (' ').

There are also the `.getElementsByClassName()` and `.getElementsByTagName()` methods which return an array of elements, instead of just one element. You can use bracket notation to access individual elements of an array:

```js
// Set first element of .student class as 'Not yet registered'
document.getElementsByClassName('student')[0].innerHTML = 'Not yet registered'

// Set second <li> tag as 'Cedric Diggory'
document.getElementsByTagName('li')[1].innerHTML = 'Cedric Diggory'
```

In the above example code, the first element with the 'student' class and the second `<li>` element have had their inner HTML changed.

### Style an Element

**[⬆ back to top](#table-of-contents)**

Another way to modify an element is by changing its CSS style. The `.style` property of a DOM element provides access to the inline style of that HTML tag.

The syntax follows an `element.style.property` format, with the property representing a CSS property.

For example, the following code selects the first element with a class of blue and assigns blue as the background-color:

```js
let blueElement = document.querySelector('.blue')
blueElement.style.backgroundColor = 'blue'
```

Unlike CSS, the DOM `.style` property does not implement a hyphen such as background-color, but rather camel case notation, backgroundColor.

The following chaining syntax would also work:

```js
document.querySelector('.blue').style.fontFamily = 'Roboto'
```

### Traversing the DOM

**[⬆ back to top](#table-of-contents)**

- A parent node is any node that is a direct ancestor of another node.

- A child node is a direct descendant of another node, called the parent node.

These relationships follow the nested structure of the HTML code. Elements nested within one HTML element are children of that parent element.

Each element has a `.parentNode` and `.children property`. The `.parentNode` property `returns the parent of the specified element in the DOM hierarchy`

**Note**: The document element is the root node so its `.parentNode` property will return null.

The `.children` property `returns an array of the specified element’s children.` If the element does not have any children, it will return null.

```html
<ul id="groceries">
  <li id="must-have">Toilet Paper</li>
  <li>Apples</li>
  <li>Chocolate</li>
  <li>Dumplings</li>
</ul>
```

In the HTML code above, we have an `<ul>` element with the ID of groceries with four `<li>` elements inside.

```js
let parentElement = document.getElementById('must-have').parentNode // returns <ul> element
let childElements = document.getElementById('groceries').children // returns an array of <li> elements
```

```js
const first = document.body.children[0]
first.innerHTML = 'BROWN BEARS ARE AWESOME!'
first.parentNode.style.backgroundColor = 'beige'
```

### Create and Insert Elements

**[⬆ back to top](#table-of-contents)**

Just as the `DOM allows scripts` to `modify existing elements`, it also `allows for the creation of new ones`.

The `.createElement()` method creates a new element based on the specified tag name passed into it as an argument. However, it does not append it to the document. It creates an empty element with no inner HTML.

```js
let paragraph = document.createElement('p')
```

the `.createElement()` method takes `'p'` as its argument which creates an empty `<p>` element and stores it as the paragraph variable.

We can assign values to the properties of the newly created element like how we’ve done previously with existing elements.

```js
paragraph.id = 'info'
paragraph.innerHTML = 'The text inside the paragraph'
```

Above, we use the `.id` property to assign `'info'` as ID and the `.innerHTML` property to set 'The text inside the paragraph' as the content of the `<p>` element.

In order to create an element and add it to the web page, you must assign it to be the child of an element that already exists on the DOM, referred to as the parent element. We call this process appending. The `.appendChild()` method will add a child element as the parent element’s last child node. The following code appends the `<p>` element stored in the paragraph variable to the document body.

```js
document.body.appendChild(paragraph)
```

The `.appendChild()` method does not replace the content inside of the parent, in this case, body. Rather, it appends the new element as the last child of that parent.

### Remove an Element

**[⬆ back to top](#table-of-contents)**

the DOM also allows for the removal of an element. The `.removeChild()` method removes a specified child from a parent.

```js
let paragraph = document.querySelector('p')
document.body.removeChild(paragraph)
```

In the above example code, the `.querySelector()` method returns the first paragraph in the document. Then, the paragraph element is passed as an argument of the `.removeChild()` method chained to the parent of the paragraph—document.body. This removes the first paragraph from the document body.

If you want to hide an element rather than completely deleting it, the `.hidden property` allows you to hide it by setting the property as true or false:

```js
document.getElementById('sign').hidden = true
```

The code above did not remove the element with ID of 'sign' from the DOM but rather, hid it.

### Add Click Interactivity

**[⬆ back to top](#table-of-contents)**

Events are `user interactions` and `browser manipulations` on the document object model.
Events can include anything from a `click` to a user mousing over an element.
The `.onclick` property allows you to assign a function to run on when a click event happens on an element:

```js
let element = document.querySelector('button')

element.onclick = function () {
  element.style.backgroundColor = 'blue'
}
```

You can also assign the `.onclick` property to a function by name:

```js
let element = document.querySelector('button')

function turnBlue() {
  element.style.backgroundColor = 'blue'
}

element.onclick = turnBlue
```

In the above example code, when the `<button>` element detects a click event, the backgroundColor will change to 'blue'.
