# Intro

DOM :
An object that contains all the elements that the user sees on the page.

Model of the page(document) stored in an object format in C++ that enables us to write in HTML, code, to describe what we want to see on the page and under the hood the render engine will transform that list(model) into a page.

## A list of elements in C++ to add to the page

`app.html`

```html
What's on your mind?
<input />
<button>
  Publish!<button>
    <div>
      <video src="/carpool.mp4">
        <p>Love les mis!</p>
        <p>♥️7</p>
      </video>
    </div>
  </button>
</button>
```

⤵️

```c++
DOM - Model (representation) of the page
[

 - 'Whats on your mind?'
 - input
 - button
   -- 'publish'
 - division
  --video -> `carpool.mp4`
  --p
    --'Love les mis!'
  --p
    --'♥️ 7'
]
```

⤵️ Layout and render engine

```webpage
What's on your mind?

[      input              ]

[Button]

Love les mis!
♥️7
```

- `DOM`

Ordered list of elements that we can add to using HTML

- Layout engine

Works out page layout for specific browser/screen

- Render engine

Produces the composite 'image' for graphic cards

## Data (and code to change it) is only available in JS

```html
<script src="app.js"></script>
```

```js
// app.js

let post = 'Hey'
post = 'hi!'
console.log(post)
```

- `Web browser API` : features of the browser that we interface with in JS

- `console` : An object full of functions that enables us to interact with the web browser

- `document` : Just like the `console` object the `document` object is an object (full of features like querySelector) in JS that has a link to the `DOM in c++`. document allows us to have access to the C++ runtime from within JS.

To recap :

- Js script is added using html
- Js is an entire runtime (thread of execution, memory for data, call stack)
- Thanks to js we have Data. But how do we display it? With WebIDL and Webcore (DOM(c++ list) + Browser)

## Webcore gives JS access to the DOM & Pixels

Let's access the DOM!

```html
<input />
<div></div>
<script src="app.js"></script>
```

1. Html code gets loaded into the parser

2. three nodes are added to the DOM

⤵️

```c++
DOM - Model (representation) of the page
[
NODES :
 - input
 - div
 - script --> <App.js>
]
```

3. Adding the `script node` triggers JS engine

```js
let post = 'hi!'

1 // define variable post and assign it the string 'hi

const jsdiv = document.querySelector('div')

2 // define the constant jsdiv and assign it the result of calling the querySelector method which is on the document object passing in the string 'div'
// 3 things happen when we do this:
// #1 : access the link to the dom list, the hidden property on the doc obj
// #2 : Query(search) the first element in the list of type div
// #3 : Normally we would bring back that element in JS but because we cannot bring back in JS something that was written in C++, What really happens is querySelector is going to create an object and that object is going to have an hidden link the div element in C++. This object is also going to be populated with a lot of functions and properties to interact with that div, like textContent

// the output of jsdiv in memory is an object (technically an accessor object) that has a lot of functions and properties to interact with the selected div element
//

jsdiv.textContent = post

//access jsdiv object , especially the textContent property and set the textContent property (technically the textContent of the div dom element) to be the value of post. textContent is technically a setter
console.log(jsdiv)
```

Memory

```
- document = { an object with hidden link to the DOM and getter setter properties and methods(getElement, querySelector, createElement) that allow us to interact from within JS with the DOM}

- post = 'hi'


- jsDiv = {'div', textContent, innerHTML, ... }
```

4. DOM (technically the render engine) display content on the page

⤵️ Layout and render engine

```webpage
What's on your mind?

[      input              ]

hi!

```

If we think about it, our Data is in one runtime (js) and the DOM and web browser responsible for the structure and the rendering of the page is in another runtime C++

## How to enable user to add data on the page that is going to be visible in javascript and use it to re-update the DOM and therefore display it on the screen

```html
<input />
<div></div>
<script src="app.js"></script>
```

```js
let post = ''
const jsInput = document.querySelector('input')

const jsdiv = document.querySelector('div')

function handleInput() {
  post = jsInput.value // affect js
  jsDiv.textContent = post
}

jsInput.onInput = handleInput
```

1. Html gets loaded into the parser
2. three nodes are added to the DOM(list)
3. Adding the `script node` triggers JS engine
4. defining the variable post and assign to it an empty string
5. defining the constant jsInput and assign to it the result of calling the querySelector method passing in the string input. As a result, an accesor object is created with an hidden link to the input element on the DOM. This object is also full of getters and setters methods that allows us to interact with this DOM element such as `value`, `onInput`

6. defining the constant jsDiv and assign to it the result of calling the querySelector method passing in the string div. As a result, an accesor object is created with an hidden link to the first div element on the DOM. This object is also full of getters and setters methods that allows us to interact with the DOM element such as , `textContent`

7. defining a function called handleInput

8. Access the accessor object jsInput that has link to our input in C++, set the setter property `onInput` to be the function handleInput. As a result `handleInput` is set (technically a reference) to be the handler of the input element to handle the user input, such that anytime the user types something in the input that function will be call back into js and executed.

DOM in C++

```
[

- input :
  * Handler : handleInput
  * value : ''
- div
]
```

9. When the user type 'hi' in the input element, the value prop of input element is set to 'hi'.

DOM in C++

```
[

- input :
  * Handler : handleInput
  * value : 'hi'

]
```

10. the DOM API, the event API is going to trigger the function `handleInput` to be call back into js. The function goes into the cb queue and then is pushed onto the call stack. A brand new execution context is created.

````handleinput()

```memory
post = 'hi' (evaluation of jsInput.value)
jsdiv.textContent = 'hi' (evaluation of post)
```

````

- post (in the global memory) is assigned the result of calling the `value` getter property on jsInput accessor object. As a result post is assigned the string 'hi'

- textContent setter property of the jsDiv accessor object is assigned the value of post which is 'hi'

11. As a result the DOM is updated with the new values from JS

DOM in C++

```
[

- input :
  * Handler : handleInput
  * value : 'hi'

- div
 * textContent : 'hi'
]
```

12. The render engine re-render the page with new values

Page

```
[Input : Hi]

[div: Hi]

```

**Note: Every change of data is going to required a back and forth between C++ and JavaScript. There's no permanent bond data in JS memory and data in C++ memory. Every single change is going to require a manual running of handler function (jsinput.onInput = handleInput) that's going to run code in JS to change the data in JS and a manual execution of a setter property to change data in C++ (jsDiv.textContent = post).**

### Summary

1. HTML and DOM work together for 1st display

2. Apps lets users interact (change what the user sees)

3. Event and handler let users run Javascript

- Data/view consistency

Displaying data and also let the user change it which implies underlying data changing but also what they see changing.

Let's introduce the click handler

## One-way data binding

The goal is change our view based on multiple user interactions. In practice there will be 1000 that can affect the data/view

```html
<input />
<div></div>
<script src="app.js"></script>
```

```js
let post = ''
const jsInput = document.querySelector('input')

const jsdiv = document.querySelector('div')

jsInput.value = 'hi' //affect the value

function handleInput() {
  post = jsInput.value
  jsDiv.textContent = post //affect the view
}

function handleClick() {
  jsInput.value = '' // affect the view
}

jsInput.onInput = handleInput
jsInput.onClick = handleClick
```

See previous chapter steps

DOM C++

```
[

- INPUT
 * handler : handleInput, handleClick
 * value : 'hi'

- DIV

- SCRIPT

]
```

1. When the user clicks on the input it triggers the handleClick function, which is added to the callback queue and then pushed onto the callstack. A new execution context is created

Execution context

```
memory
jsinput.value = ''

```

- Access the jsInput object acessor and set the value setter property to an empty string. as a result the DOM is updated and the input element on the page too(it's cleared)

2. When the user types 'hi' in the input it triggers the handleInput function, which is added to the callback queue and then pushed onto the callstack. A new execution context is created

Execution context

```
memory
post = 'hi' (evaluation of jsInput.value)
jsdiv.textcontent = 'hi' (evaluation of post)

```

- post (in the global memory) is assigned the result of calling the value getter property on jsInput accessor object. as a result post is assigned the string 'hi'

- textContent setter property of the jsDiv accessor object is to the value of post which is 'hi'. as a result the DOM is updated and our page shows 'hi'

There is one to one relationship between the data that the user sees and what's saved in our app

Instead of having multiple functions that affect the view, let's write the same but with one function that's going to run manually to affect the view.

```html
<input />
<div></div>
<script src="app.js"></script>
```

```js
let post = undefined
const jsInput = document.querySelector('input')

const jsdiv = document.querySelector('div')

function dataToView() {
  //affect the view

  jsInput.value = post == undefined ? "what's up" : post
  jsDiv.textContent = post
}

function handleInput() {
  post = jsInput.value // update data
  dataToview() //convert data to view
}

function handleClick() {
  post = '' // update data
  dataToview() //convert data to view
}

jsInput.onInput = handleInput
jsInput.onClick = handleClick
dataToview()
```

see previous steps

`DataToview()`

- When Js engine gets to that line, a new execution context is created, inside we go and

1. we set the value setter to the result of the ternery ops, which is `what's up` because post is `undefined`. as a result the DOM input element is updated and `the entire page re-render`.
2. we set the textContent setter property to the value of post which is undefined.Conveniently textContent converted undefined to an empty string. The dom div element is updated and `the entire page re-render`

- User actions

1. When the user clicks on the input it triggers the handleClick function, which is added to the callback queue and then pushed onto the callstack. A new execution context is created. Inside we go,

- we reassigning the value of post to an empty string
- we calling dataToView(), that's going to create a new execution context. inside we go

- accessing the jsInput accessor object and we set the value setter property to the result of the ternary ops which is going to be an empty string. As a result the DOM input element is updated to an empty string and page rerender(input cleared)
- accessing the jsDiv accessor object, we set the value of the textContent setter property to the value of post which is an empty string. As a result the DOM div element is updated and shows nothing

2. Same steps for when user start typing

We have one function that affects the view and two functions to change data in JS.

We have everything in JS that we need if we wanted to map out what's going to show up on the C++ dom

memory

```
document : {}
post : 'hi'
jsInput: {}
jsDiv: {}
dataToview ()
handleClick()
handleInput()
```

What if we don't have to`manually` run dataToview? What if it was possible to run dataToview Automatically with a function like `setInterval()?`

```html
<input />
<div></div>
<script src="app.js"></script>
```

```js
let post = undefined
const jsInput = document.querySelector('input')

const jsdiv = document.querySelector('div')

// could be called render
function dataToView() {
  //affect the view

  jsInput.value = post == undefined ? "what's up" : post
  jsDiv.textContent = post
}

function handleInput() {
  post = jsInput.value // update data
}

function handleClick() {
  post = '' // update data
}

jsInput.onInput = handleInput
jsInput.onClick = handleClick
setInterval(dataToview, 15)
```

### summary

Two goals of UI development.

1. Display content
2. Allow user to change what they see, to change content.

The second goal implies some underlying data changes as well(30 likes that we see on the page is the final representation of some data that exist in our app)

While our data lives in JS, the DOM, the list of elements that allows us to display content is in another runtime C++.

We have to join up these two separate worlds.

And we do this thanks to handlers that we attach to DOM elements such that when the user interacts with the web page, it triggers JS, and we're able to retrieve data added by the user, and use that data to re-update the DOM and therefore the page.

To reduce complexity and add have predictable code we created a single function (dataToview(render)) to pipe that data to the page via the DOM.

We also see that we could use setInterval() to run that function automatically every 15ms instead of runnning it manually.

We introduced another idea. What if we could also have this function capture all the information we display including the elements that we see on the page?( A visual representation of the DOM)
