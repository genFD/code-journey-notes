# Intro

## Learning Objectives

Main GOAL :

**Develop an under the hood understanding of building UI in the browser.**

PART I

- Understanding how can we build the most minimal UI in the web browser ?

PART II

PART III

- Build in JS a visual representation of what is going to show up on the page. (virtual DOM)

PART IV

- Building with state hooks and diffing algorithm to improve performance

**DOM : Dcoument object Model.**

> An object that contains all the elements that the user sees on the page.

Model of the page(document) stored in an object format in C++ that enables us to write in HTML, code to describe what we want to see on the page.

Under the hood, the render engine will transform that list (model) into a page.

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
5. defining the constant jsInput and assign to it the result of calling the querySelector method passing to it the string input. As a result, an accesor object is created with an hidden link to the input element on the DOM. This object is also full of getters and setters methods that allows us to interact with this DOM element such as `value`, `onInput`

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

We introduced another idea. What if we could also have this function capture all the information we display including the elements that we see on the page?( A visual representation of the DOM) (Functionnal components)

This is going

### Returning to our UI with auto updating view

```html
<input />
<div></div>
<script src="app.js"></script>
```

```js
let post = ''
const jsInput = document.querySelector('input')

const jsdiv = document.querySelector('div')

// could be called render
function dataToView() {
  //affect the view
  jsInput.value = post
  jsDiv.textContent = post
}

function handleInput() {
  post = jsInput.value // update data
}

jsInput.onInput = handleInput
setInterval(dataToview, 15)
```

See previous section for all the steps (skipping to user action)

- Every 15 ms dataToview is going to run
- A new execution context is created. We access the accessor object jsInput and set the value setter property to be the value of post, which is an empty string. and we do the same thing for textContent. The DOM is updated and therefore the UI

1. When the user types something in the input field :

- The DOM input element (value) is updated .

- The handleInput function is triggered

- the variable post is reassigned in JS.

- DatatoView in the background runs again, update jsInput.value and jsInput.textContent

- The DOM is updated again and therefore the UI

### Function that fully creates element and relates data/view - Function component

`Function component` :

```html
<script src="app.js"></script>
```

```js
let post = ''
let jsInput
let jsDiv

// could be called render
function dataToView() {
  //affect the view
  jsInput = document.createElement('input')
  jsDiv = document.createElement('div')
  jsInput.value = post
  jsDiv.textContent = post
  jsInput.onInput = handleInput

  document.body.replaceChild(jsInput, jsDiv)
}

function handleInput() {
  post = jsInput.value // update data
}

setInterval(dataToview, 15)
```

- Document object is loaded into js.

- a place in the global memory is allocated for post, jsInput and jsDiv. post is assigned and empty string, jsInput and jsDIv are uninitialized.

- A place in the global memory is allocated for dataToview

- A place in the global memory is allocated for handleInput

- setInterval is executed it set up an interval of 15ms in the browser and push datatoview in the callback queue

- After 15 ms datatoView is pushed onto the callstack. and then it runs. A new execution context is created

- jsInput in global memory which was uninitialized is assigned a new value. it is an accessor object that contains a link to a newly created DOM element, an input element and a lot of others properties and methods to interact with that element

- Same for jsDiv, but the element is just a div

- The value setter property on jsInput object is assigned the value of post, which is an empty string.

- The textContent setter property of jsDiv is also assigned to the value of post.

- we set up a new handler function for jsInput, handleInput. which means that every time the user types something in the input field handle input is going to be executed

- and finally we set jsInput and jsInput as children of the body element. The DOM is updated and therefore the UI too.

- When a user types something in the input field. it updates the input DOM element and triggers handleInput

- handleInput is executed. It's going to reassigned the value of post which was an empty string to the value entered by the user.

- dataToview runs after 15ms.

- A new unattached input DOM element is created. jsInput loose it's link to the first input element is gets a new link to the newly created input element

- the same happens to jsdiv

DOM

```
[

BEFORE :
- BODY
 - INPUT
 - DIV

 AFTER :
- BODY
 - INPUT
 - DIV

 -------UNATTACHED---
 - INPUT
 - DIV
]
```

- value and textContent are set to the user data

- line 12 the previous link between body and input and div is broken and a new link is created between body and the unattached input and div

_What happens to the previous elements in the DOM attached to the body?_

They get garbage collected, to avoid memory leaks, meaning having elements that have no links in JS but are also, unaatached in the DOM.

**We are constantly recreating element on the DOM anytime data is updated.**

### Could we emulate our HTML with semi-visual coding?

```html
<script src="app.js"></script>
```

```js
let name = 'jo'

const divInfo = ['div', `hi, ${name}!`]

function convert(node) {
  const element = document.createElement(node[0])
  element.textContent = node[1]
  return elem
}
const jsdiv = convert(divInfo)
```

- Document object is loaded into js. It has a hidden link to the DOM. (it's loaded in js whenver the host meaning the place that's run our code is the browser)

- a place in the global memory is allocated for name, divInfo, convert and jsDiv

- name is assigned the string jo
- divInfo is assigned an array with 2 items: the string div and the evaluated result of the template literal hi name, so hi jo
- jsDiv is assigned the result of calling convert with divInfo

- a new execution context is created
- we go into convert :

1. the parameter node is assigned the argument divInfo,
2. a place in the local memory is allocated for a new constant element. element is assigned an accessor object that contains a link to a newly created div DOM element because node[0] evalutes to the first item in the array which is a div. element also contains and a lot of methods to interact with the div element.
3. the textContent setter property is set to the value of second item in the array which is 'hi jo'
4. the element object is returned from that function

- Nothing show up on the screen because the newly created div is unattached. Not attached to the body

`We created a virtual DOM`

1. Blocks of code representing a visual of our view

- JS array with all the details (in practice it is an object) :

```js
const divInfo = ['div', `hi, ${name}!`]
```

- the function that takes in that array and produces a DOM element as an ouput `convert`

```js
function convert(node) {
  const element = document.createElement(node[0])
  element.textContent = node[1]
  return elem
}
```

### Creating JS virtual DOM part II

```html
<script src="app.js"></script>
```

```js
let name = ''
let jsInput
let jsDiv
let vDOM

function createVDOM() {
  return [
    ['input', name, handle],
    ['div', `hello ${name}!`],
  ]
}
function handleInput() {
  name = jsInput.value
} //change data in JS

function updateVDOM() {
  vdom = createVDOM()

  jsInput = convert(vDOM[0]) // new unattached dom element

  jsDiv = convert(vDOM[1]) // new unattached dom element
  document.body.replaceChildren(jsInput, jsDiv) // attach dom elements to the body
}

function convert(node) {
  const element = document.createElement(node[0])
  element.textContent = node[1]
  element.value = node[1]
  element.oninput = node[2]
  return elem
}

setInterval(updateVDOM, 15)
```

- Document object is loaded into js. It has a hidden link to the DOM

- a place in the global memory is allocated for name, jsInput, jsDiv,vDOM, createVDOM, handleInput, updateVDOM and convert

- jsInput, jsDiv,vDOM are uninitialized and name is assigned an empty string.

- setInterval is executed. A new interval of 15ms is setup in the browser and updateVDOM is push into the callback queue.

- after 15ms updateVDOM is pushed onto the callstack, it runs, a new execution context is created :

1. vDOM in the global memory which was uninitialized is assigned the result of calling createVDOM. A new execution context context is created, and we go into createVDOM(). createVDOM() returns a two dimensional array. the array is assigned to VDOM and then createVDOM() gets popped off the callstack

2. we return into updateVDOM execution context, jsInput is assigned the result of calling convert with the item at position 0 in the vDOM array which is an array with 3 items `['input', name, handle]`. a new execution context is created, the param node is assigned the argument `['input', name, handle]`, a place in the local memory is allocated to a new constant element. element is assigned an accessor object which contains a link to newly created `input` DOM element and lots of methods and property to interact with input DOM element. textContent element is assigned name which is an empty string, onInput the handler handleInput. we return the element accessor object and assigned it to `jsInput`. convert is popped off the callstack

3. we return into UpdateVDOM and we do the same jsDIV

4. finally we attach to the body the newly created DOM elements `input` and `div`. the DOM is updated and therefore the UI is also updated

- When the user types something in the input element the DOM element value is updated, and handleInput runs and update the value in JS

- 15ms after updateDOM re-runs, **it recreates a new input and a new div element**, the previous elements get garbage collected, the ui is updated with the user values

Memory in JS

```
`before user input`
vdom : [
  [
    ['input', name, handle],
    ['div', 'hello !'],
  ]
]

`after user input`

vdom : [
  [
    ['input', y, handle],
    ['div', 'hello y!'],
  ]
]
```

- In the next iteration, instead of recreating all the elements from scratch everytime our data in js changes we will compare the `vDOM after user input` and the `vDOM before user input`, so that we will be able to update only the element that changed

### Summary

Declarative UI as a programming paradigm

### New element-flexible code

```html
<script src="app.js"></script>
```

```js
let name = ''
let jsInput
let jsDiv
let vDOM

function createVDOM() {
  return [
    ['input', name, handle],
    ['div', `hello ${name}!`],
    ['div', `great job!`],
  ]
}
function handleInput(e) {
  name = e.target.value
} //change data in JS

function updateVDOM() {
  vdom = createVDOM()

  elems = vdom.map(convert)

  document.element.replaceChildren(...elems)
}

function convert(node) {
  const element = document.createElement(node[0])
  element.textContent = node[1]
  element.value = node[1]
  element.oninput = node[2]
  return elem
}

setInterval(updateDOM, 15)
```

- createVDOM() creates a "js" dom

Memory

```
vdom =  createVDOM();
before user input
vdom : [
 [input, name, handle],
 [div, 'hello,!'],
 [div, "great job!"],
]

elems = vDOM.map(convert)

<!-- elems is an array of accessor object for the corresponding dom elements  -->

elems : [
{'input', ...}
{'div', ...}
{'div', ...}
]

document.element.replaceChildren(...elems) =
document.element.replaceChildren(elems[0], elems[1] elems[2] )



<!-- The dom is updated and therefore the UI  -->


```

We're no longer getting individual labels for our accessor objects like before, we spreading them into the replaceChildren function. So in the handler function we used the `event object` (an object full of information about the user actions) to access the value of the input element

```js
function handleInput(e) {
  name = e.target.value
} //change data in JS
```

so in the execution context of handleInput when the user types something in the input element.
Execution context

```

handleInput(e){name = e.target.value}
e: {
 target : 'input',
 value : 'Hello'
}
name = 'li'
```

- updateDOM Reruns

Memory

```
*BEFORE USER INPUT

vdom =  createVDOM();

vdom : [
 [input, name, handle],
 [div, 'hello,!'],
 [div, "great job!"],
]

elems = vDOM.map(convert)

<!-- elems is an array of accessor object for the corresponding dom elements  -->

elems : [
{'input', ...}
{'div', ...}
{'div', ...}
]

document.element.replaceChildren(...elems) =
document.element.replaceChildren(elems[0], elems[1] elems[2] )



<!-- The dom is updated and therefore the UI  -->

*AFTER USER INPUT
inside updateVDOM()

1. vdom : [
 [input, 'li', handle],
 [div, 'hello, li!'],
 [div, "great job!"],
]

2. elems = vDOM.map(convert)

<!-- elems is an array of accessor object for new unattached corresponding dom elements  -->

elems : [
{'input', ...} => value is (li) in the DOM
{'div', ...} => textContent is (hello li!) in the DOM
{'div', ...} => div is created with text content 'hello great job'
]

3. document.element.replaceChildren(...elems) =
document.element.replaceChildren(elems[0], elems[1] elems[2] )

the three previous dom elements linked to body get garbage collected. and new link is created between body and three new elements


<!-- The dom is updated and therefore the UI  -->

```

### Solving performance problems

Theare 3 performance problems with our current iteration.

1. we auto-updating the dom every 15ms. what if we can simulate auto-updating with state hook

2. we recreating the DOM elements on every change from scratch. we can write a diff algorithm to compare the previous vDOm and the new one and update only the DOM elements that changed.

### State Hook

We're going to try to run updateDOM() only when Data changes. Data changes when the user via an action triggers the handler function. so the best place to run updateDOm() will be inside the handler function.

```js
function handleInput(e) {
  name = e.target.value
  updateDOM()
} //change data in JS
```

or we can create a hook setName()

```js
//1 state store
const data = {name : ''}

//2 hook
function setName(label, value){
 data[label] : value
 updateDom()
}

//handler
function handleInput(e) {
  setName('name', e.target.value)
} //change data in JS
```

- we can make a setName() or setAge() etc.., have it work `with any piece of data` in our app.

- So they call it a `hook`

### Diff algo

- Right now we recreating each elements in full on every data changes.

- But we have an old version of our vDOM in memory.

- We can more efficient by writting some code to compare what elements differ and update that elements.

```js
function findDiff(prev, current) {
  for (let i = 0; i < current.length; i++) {
    if (JSON.stringify(prev[i]) != JSON.stringify(current[i])) {
      //change the DOM element
    }
  }
}
```

- say on first render

```js
vdom1 = [
  [input, name, handleInput],
  [div, `hello, ${name}`],
  [div, 'hello welcome'],
]
```

- When the user interact with the page and types `li` in the input field,

```js
vdom2 = [
  [input, 'li', handleInput],
  [div, `hello, li`],
  [div, 'hello welcome'],
]
```

- on first iteration we will have a comparison between the string version of the item at position 0 of vdom1 and the string version of the item at position 0 of the vdom2 :

```md
'div, 'hello' vs 'div, 'hello name!'

There is a difference so will execute code to change the item at position 1 of the array at position 1 inside vdom 1
```

```js
vdom1[i].value = vdom2[i][1]
//  which evaluates to
vdom1[0].value = vdom2[0][1]
```

- on second iteration we will have a comparison between the string version of the array at position 1 of vdom1 and the string version of the array at position 1 of the vdom2 :

```md
'input, name, handleInput' vs 'input, 'li', handleInput'

There is a difference so will execute code to change the item at position 1 of the array at position 0 inside vdom 1
```

- on the last iteration we will have a comparison between the string version of the array at position 2 of vdom1 and the string version of the array at position 2 of the vdom2 :

```md
'div, hello welcome' vs 'div, 'hello welcome'

There is no difference so no change
```

The full version of our code :

```html
<script src="app.js"></script>
```

```js
let name = ''
let vDOM = createVDOM()
let prevDOM
let elems

function createVDOM() {
  return [
    ['input', name, handle],
    ['div', `hello ${name}!`],
    ['div', `great job!`],
  ]
}
function handleInput(e) {
  name = e.target.value
} //change data in JS

function updateVDOM() {
  if (elems === undefined) {
    elems = vDOM.map(convert)
    document.body.append(...elems)
  } else {
    prevDOM = [...vDOM]
    vDOM = createVDOM()
    findDiff(prevVDOM, vDOM)
  }
}

function convert(node) {
  const element = document.createElement(node[0])
  element.textContent = node[1]
  element.value = node[1]
  element.oninput = node[2]
  return elem
}
function findDiff(prev, current) {
  for (let i = 0; i < current.length; i++) {
    if (JSON.stringify(prev[i]) != JSON.stringify(current[i])) {
      elems[i].textContent = current[i][1]
      elems[i].value = current[i][1]
    }
  }
}

setInterval(updateDOM, 15)
```

- Document object is loaded into js. It has a hidden link to the DOM. (it's loaded in js whenver the host meaning the place that's run our code is the browser)

- a place in the global memory is allocated to name, vDOM, prevVDOM, elems, createVDOM, handleInput, updateVDOM, convert, findDiff

Global memory

```
- name : ''
- vDOM : [
 ['input', name, handle],
 ['div', `hello ${name}!`],
 ['div', `great job!`],
]
- prevVDOM :

elems :
createVDOM : fn
handleInput : fn
updateVDOM : fn
convert : fn
findDiff : fn
```

- after 15ms updateVDOM is pushed onto the callstack, it runs, a new execution context is created. convertDOM will be executed on each element and the elemes will be an array of accessor objects

Global memory

```
elems:
[
 {input, ...},
 {div, ...}
 {'div' ...}
]
```
