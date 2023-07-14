# Writting code for the browser

- Using different techniques to create forms with React
- Listening to DOM events and implementing custom handlers
- A way of performing imperative operations on DOM nodes using refs
- Creating simple animations that work across different browsers
- The React way of generating SVG

## Understanding and implementing forms

### Uncontrolled components

- regular HTML form inputs
- you are not able to manage the value
- the DOM will take care of handling the value
- you can get this value by using a React ref.

```jsx
import { useState, ChangeEvent, MouseEvent } from 'react'
const Uncontrolled = () => {
  const [value, setValue] = useState('')
  return (
    <form>
      <input type="text" />
      <button>Submit</button>
    </form>
  )
}
export default Uncontrolled
```

This is an example of an uncontrolled component, where we do not set the value of the input field, but we let the component manage its own internal state.

#### Onchange listener

Most likely, we want to do something with the value of the element when the Submit button is clicked. For example, we may want to send the data to an API endpoint.

We can do this easily by adding an onChange listener

```jsx
const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
  console.log(e.target.value)
}
```

- The event listener is receiving an event object,
- the target represents the field that generated the event
- We are interested in its value.
- In the body of the function we just logging it

Finally, we render the form:

```jsx
return (
  <form>
    <input type="text" onChange={handleChange} />
    <button>Submit</button>
  </form>
)
```

- The handleChange listener is fired every time the value of the input changes.
- Therefore, our function is called once for each typed character.

```sh
R
Re
Rea
Reac
React
```

- The next step is to store the value that's entered by the user and make it available when the user clicks the Submit button.

- We just have to change the implementation of the handler to store it in the state instead of logging it, as follows :

```jsx
const handleChange = (e: ChangeEvent<HTMLInputElement>) => {
  setValue(e.target.value)
}
```

#### OnSubmit listener

```jsx
const handleSubmit = (e: MouseEvent<HTMLButtonElement>) => {
  e.preventDefault()

  console.log(value)
}
```

What if we have multiple fields? Suppose we have tens of different fields?
Our goal is to use a single change handler so that we can add an arbitrary number of fields without creating new listeners.

```jsx
const [values, setValues] = useState({ firstName: '', lastName: '' })
```

```jsx
const handleChange = ({ target: { name, value } }) => {
  setValues({
    ...values,
    [name]: value,
  })
}
```

- the target property of the event we receive represents the input field that has fired the event, so we can use the name of the field and its value as variables

We then have to set the name for each field:

```jsx
return (
  <form onSubmit={handleSubmit}>
    <input type="text" name="firstName" onChange={handleChange} />
    <input type="text" name="lastName" onChange={handleChange} />
    <button>Submit</button>
  </form>
)
```

That's it! We can now add as many fields as we want without creating additional handlers.

### Controlled components

We are going to look at how we can prefill the form fields with some values, which we may receive from the server or as props from the parent

The first example shows a predefined value inside the input field:

```jsx
const Controlled = () => (
  <form>
    <input type="text" value="Hello React" />
    <button>Submit</button>
  </form>
)
```

- If we run this component inside the browser, we realize that it shows the default value as expected, but `it does not let us change the value or type anything else inside it`.

- Now, if we just want the input field to have a default value and we want to be able to change, we can use the defaultValue property:

```jsx
import { useState } from 'react'
const Controlled = () => {
  return (
    <form>
      <input type="text" defaultValue="Hello React" />
      <button>Submit</button>
    </form>
  )
}
export default Controlled
```

- In this way, the field is going to show Hello React when it is rendered, but then the user can type anything inside and change its value.

```jsx
const [values, setValues] = useState({
  firstName: 'Carlos',
  lastName: 'Santana',
})
```

The handlers are the same as the previous ones:

```jsx
const handleChange = ({ target: { name, value } }) => {
  setValues({
    [name]: value,
  })
}

const handleSubmit = (e) => {
  e.preventDefault()

  console.log(`${values.firstName} ${values.lastName}`)
}
```

- In fact, we will use the value attributes of the input fields to set their initial values, as well as the updated one

```jsx
return (
  <form onSubmit={handleSubmit}>
    <input
      type="text"
      name="firstName"
      value={values.firstName}
      onChange={handleChange}
    />
    <input
      type="text"
      name="lastName"
      value={values.lastName}
      onChange={handleChange}
    />
    <button>Submit</button>
  </form>
)
```

- The first time the form is rendered, React uses the initial values from the state as the value of the input fields. When the user types something into the field, the handleChange function is called and the new value for the field is stored in the state

- When the state changes, React re-renders the component and uses it again to reflect the current values of the input fields. We now have full control over the values of the fields, and we call this pattern controlled components

### Handling events

React introduced the concept of the synthetic event. A synthetic event is an object that wraps the original event object provided by the browser, and it has the same properties, no matter where it is created.

```jsx
const Button = () => {}
export default Button
```

Then we define the event handler:

```jsx
const handleClick = (syntheticEvent) => {
  console.log(syntheticEvent instanceof MouseEvent)
  console.log(syntheticEvent.nativeEvent instanceof MouseEvent)
}
```

- we check the type of the event object we receive from React and the type of native event attached to it.

- We expect the first to return false and the second to return true.

- Finally, we define the button with the onClick attribute to which we attach our event listener:

```jsx
return <button onClick={handleClick}>Click me!</button>
```

- suppose we want to attach a second handler to the button that listens to the double-click event

- a common practice is to write a single event handler for each component, which can trigger different actions according to the event type.

```jsx
const handleEvent = (event) => {
  switch (event.type) {
    case 'click':
      console.log('clicked')
      break

    case 'dblclick':
      console.log('double clicked')
      break

    default:
      console.log('unhandled', event.type)
  }
}
```

- The generic event handler receives the event object and switches on the event type to fire the right action. This is particularly useful if we want to call a function on each event (for example, analytics) or if some events share the same logic.

- Finally, we attach the new event listener to the onClick and onDoubleClick attributes:

```jsx
return (
  <button onClick={handleEvent} onDoubleClick={handleEvent}>
    Click me!
  </button>
)
```

- From this point on, whenever we need to create a new event handler for the same component, instead of creating a new method and binding it, we can just add a new case to the switch.

### Exploring refs

Being declarative means that you just describe what you want to be displayed on the screen at any point in time and React takes care of the communications with the browser. This feature makes React very easy to reason about and very powerful at the same time.

`However, there might be some cases where you need to access the underlying DOM nodes to perform some imperative operations.`
This should be avoided because, in most cases, there is a more React-compliant solution to achieve the same result, but it is important to know that we have the option to do it and to know how it works so that we can make the right decision.

Suppose we want to create a simple form with an input element and a button, and we want it to behave in such a way that when the button is clicked, the input field gets focused.
What we want to do is call the focus method on the input node, the actual DOM instance of the input, inside the browser's window

Let's create a component called Focus; you need to import useRef and create an inputRef constant:

```tsx
import { useRef } from 'react'
const Focus = () => {
  const inputRef = useRef(null)
}
export default Focus
```

- Then, we implement the handleClick method:

```jsx
const handleClick = () => {
  inputRef.current.focus()
}
```

As you can see, we are referencing the current attribute of inputRef and calling the focus method on it.

To understand where it comes from, you just have to check the implementation of render:

```jsx
return (
  <>
    <input type="text" ref={inputRef} />
    <button onClick={handleClick}>Set Focus</button>
  </>
)
```

- We create a form with an input element inside it and we define a function on its ref attribute.

- The callback `inputRef` we defined is called when the component gets mounted, and the element parameter represents the DOM instance of the input.It is important to know that, when the component gets unmounted, the same callback is called with a null parameter to free the memory.

- What we are doing in the callback is storing the reference of the element to be able to use it in the future (for example, when the handleClick method is fired). Then, we have the button with its event handler. Running the preceding code in a browser will show the form with the field and the button, and clicking on the button will focus the input field, as expected.

### forwardRef

`forwardRef` lets your component expose a DOM node to parent component with a ref.

```jsx
const SomeComponent = forwardRef(render)
```

#### forwardRef(render)

Call `forwardRef()` to let your component receive a ref and forward it to a child component:

```jsx
import { forwardRef } from 'react'

const MyInput = forwardRef(function MyInput(props, ref) {
  // ...
})
```

#### Parameters

- render:

The render function for your component.

React calls this function with the props and ref that your component received from its parent.

The JSX you return will be the output of your component.

#### Returns

- `forwardRef` :
  returns a React component that you can render in JSX.

Unlike React components defined as plain functions, a component returned by forwardRef is also able to receive a ref prop.

#### Caveats

In Strict Mode, React will call your render function twice in order to help you find accidental impurities.

This is development-only behavior and does not affect production. If your render function is pure (as it should be), this should not affect the logic of your component. The result from one of the calls will be ignored.

### Render function

`forwardRef` accepts a render function as an argument. React calls this function with props and ref:

```jsx
const MyInput = forwardRef(function MyInput(props, ref) {
  return (
    <label>
      {props.label}
      <input ref={ref} />
    </label>
  )
})
```

#### Parameters (render function)

- props: The props passed by the parent component.

- ref: The ref attribute passed by the parent component.

The ref can be an object or a function.

If the parent component has not passed a ref, it will be null.

You should either pass the ref you receive to another component, or pass it to `useImperativeHandle`.

#### Returns (render function)

`forwardRef` returns a React component that you can render in JSX.

`Unlike React components defined as plain functions, the component returned by forwardRef is able to take a ref prop`.

### Exposing a DOM node to the parent component

By default, each component’s DOM nodes are private.

However, sometimes it’s useful to expose a DOM node to the parent—for example, to allow focusing it

To opt in, wrap your component definition into forwardRef():

```jsx
import { forwardRef } from 'react'
// this
const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props
  return (
    <label>
      {label}
      <input {...otherProps} />
    </label>
  )
})
// same as

function MyInput(props, ref) {
  const { label, ...otherProps } = props
  return (
    <label>
      {label}
      <input {...otherProps} />
    </label>
  )
}
const Myform = forwardRef(MyInput)
```

- You will receive a ref as the second argument after props. Pass it to the DOM node that you want to expose:

```jsx
import { forwardRef } from 'react'

const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props
  return (
    <label>
      {label}
      <input {...otherProps} ref={ref} /> // input DOM node will be exposed
    </label>
  )
})
```

- This lets the parent Form component access the `<input>` DOM node exposed by MyInput:

```jsx
function Form() {
  const ref = useRef(null)

  function handleClick() {
    ref.current.focus()
  }

  return (
    <form>
      <MyInput label="Enter your name:" ref={ref} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  )
}
```

1. This Form component passes a ref to MyInput.

2. The MyInput component forwards that ref to the `<input>` browser tag (input element).

3. As a result, the Form component can access that `<input>` DOM node and call focus() on it.

4. Clicking the button will focus the input

Keep in mind that exposing a ref to the DOM node inside your component makes it harder to change your component’s internals later.

You will typically expose DOM nodes from `reusable low-level components` like`buttons` or `text inputs`, but you won’t do it for application-level components like an avatar or a comment.

### Forwarding a ref through multiple components

Instead of forwarding a ref (reference) to a DOM node, you can forward it to your own component like `MyInput`.

```jsx
const FormField = forwardRef(function FormField(props, ref) {
  // ...
  return (
    <>
      <MyInput ref={ref} />
      ...
    </>
  )
})
```

If MyInput component forwards a ref to its `<input>` tag, a ref to FormField will give you that `<input>`:

```jsx
// 1-  The Form component defines a ref and passes it to FormField
function Form() {
  const ref = useRef(null)

  function handleClick() {
    ref.current.focus()
  }

  return (
    <form>
      <FormField label="Enter your name:" ref={ref} isRequired={true} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  )
}
// 2-  The FormField component forwards that ref to MyInput
const FormField = forwardRef(function FormField(props, ref) {
  // ...
  return (
    <>
      <MyInput ref={ref} />
      ...
    </>
  )
})

//3- MyInput forwards it to a browser `<input>` DOM node

const MyInput = forwardRef(function MyInput(props, ref) {
  const { label, ...otherProps } = props
  return (
    <label>
      {label}
      <input {...otherProps} ref={ref} /> // input DOM node will be exposed
    </label>
  )
})

// This is how Form accesses that DOM node and calls focus() on it.
```

### Exposing an imperative handle instead of a DOM node

Instead of exposing an entire DOM node, you can expose a custom object, called an imperative handle, with a more constrained set of methods. To do this, you’d need to define a separate ref to hold the DOM node:

```jsx
const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null)

  // ...

  return <input {...props} ref={inputRef} />
})
```

Pass the ref you received to `useImperativeHandle` and specify the value you want to expose to the ref:

```jsx
import { forwardRef, useRef, useImperativeHandle } from 'react'

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null)
  // #####
  useImperativeHandle(
    ref,
    () => {
      return {
        focus() {
          inputRef.current.focus()
        },
        scrollIntoView() {
          inputRef.current.scrollIntoView()
        }, // The scrollIntoView() method scrolls an element into the visible area of the browser window
      }
    }, // the second argument is function that returns an object {focus, scrollIntoView}
    []
  )
  // ########
  return <input {...props} ref={inputRef} />
})
```

- If some component gets a ref to `MyInput`, it will only receive your { focus, scrollIntoView } object instead of the DOM node.This lets you limit the information you expose about your DOM node to the minimum.

**⚠️Warning⚠️ : Do not overuse refs. You should only use refs for `imperative behaviors` that you can’t express as props: for example, `scrolling to a node`, `focusing a node`, `triggering an animation`, `selecting text`, and so on. If you can express something as a prop, you should not use a ref. For example, instead of exposing an imperative handle like { open, close } from a Modal component, it is better to take isOpen as a prop like `<Modal isOpen={isOpen} />`. Effects can help you expose imperative behaviors via props.**

### Troubleshooting

#### My component is wrapped in forwardRef, but the ref to it is always null

This usually means that you forgot to actually use the ref that you received.

For example, this component doesn’t do anything with its ref:

```jsx
//   ##############
const MyInput = forwardRef(function MyInput({ label }, ref) {
  // ##############
  return (
    <label>
      {label}
      // ##############
      <input />
      // ##############
    </label>
  )
})
```

- To fix it, pass the ref down to a DOM node or another component that can accept a ref:

```jsx
const MyInput = forwardRef(function MyInput({ label }, ref) {
  return (
    <label>
      {label}
      <input ref={ref} />
    </label>
  )
})
```

- The ref to MyInput could also be null if some of the logic is conditional:

```jsx
const MyInput = forwardRef(function MyInput({ label, showInput }, ref) {
  return (
    <label>
      {label}
      {showInput && <input ref={ref} />}
    </label>
  )
})
```

If showInput is false, then the ref won’t be forwarded to any node, and a ref to MyInput will remain empty.

This is particularly easy to miss if the condition is hidden inside another component, like `Panel` in this example:

```jsx
const MyInput = forwardRef(function MyInput({ label, showInput }, ref) {
  return (
    <label>
      {label}
      <Panel isExpanded={showInput}>
        <input ref={ref} />
      </Panel>
    </label>
  )
})
```
