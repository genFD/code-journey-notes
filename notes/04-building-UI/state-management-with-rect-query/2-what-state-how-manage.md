# What Is State and How Do We Manage It?

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

State is a mutable data source that can be used to store data in a React application and can change over time and be used to determine how your component renders.

This chapter

- will refresh your existing knowledge about state in the React ecosystem. We will review what it is and why it is needed, and understand how it helps you build React applications.

We’ll also

- review how you can manage state natively in React by using the useState hook, the useReducer hook, and React Context.

Finally,

- we’ll offer brief descriptions of the common state management solutions such as Redux, Zustand, and MobX and understand why they have been created and the main common concept they all share.

By the end of this chapter,

- you will have either learned or remembered everything about state necessary to proceed in this book.

- You will also notice a pattern in how state is managed between different state management solutions and meet or get reacquainted with a familiar term. Spoiler alert: it is global state.

In this chapter, we’ll be covering the following topics:

## Notes

### What is state in React?

I challenge you to try to build a React application without any type of state. You’d probably be able to do something, but you would soon conclude that props cannot do everything for you and get stuck.

As mentioned in the introduction, state is a mutable data source used to store your data.

State is mutable, which means that it can be changed over time. When a state variable changes, your React component will re-render to reflect any changes that the state causes to your UI.

Okay, now, you might be wondering, “What will I store in my state?” Well, the rule of thumb that I follow is that if your data fits into any of the following points, then it’s not state:

- Props
- Data that will always be the same
- Data that can be derived from other state variables or props

Anything that doesn’t fit this list can be stored in state. This means things such as data you just fetched through a request, the light or dark mode option of a UI, and a list of errors that you got from filling a form in the UI are all examples of what can be state.

Let’s look at the following example:

```jsx
const NotState = ({ aList = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10] }) => {
  const value = 'a constant value'
  const filteredList = aList.filter((item) => item % 2 === 0)
  return filteredList.map((item) => <div key={item}>{item}</div>)
}
```

Here, we have a component called NotState. Let’s look at the values we have in there and use our rule of thumb.

The aList variable is a component prop. Since our component will receive this, it doesn’t need to be state.

Our value variable is assigned a string value. Since this value will always be constant, then it doesn’t need to be state.

Finally, the filteredList variable is something that can be derived from our aList prop; therefore, it doesn’t need to be state.

### Managing state in React

What is React Query?

React Query is a protocol-agnostic collection of hooks for fetching, caching, and updating server state in React.

Let’s consider the following scenario.

I want to build an application that allows me to count something. In this application, I want to be able to do the following:

- See the current counter value
- Increment my counter
- Decrement my counter
- Reset the counter

Let’s imagine that we have a React component called`App` :

```jsx
const App = () => {
  ...
  return (
    <div className="App">
      <div>Counter: {count}</div>
      <div>
        <button onClick={increment}>+1</button>
        <button onClick={decrement}>-1</button>
        <button onClick={reset}>Reset</button>
      </div>
    </div>
)
```

This app provides the UI needed to handle our counter needs, such as a div that we should use to display our count and three buttons with an onClick event waiting for a callback function to perform each of the following actions needed.

We are just missing the heart of this component, which is the state. Natively, React gives us two ways to hold state in our applications: `useState` and`useReducer`.

Let’s start by looking at useState.

#### Managing state with useState

useState is a React Hook that `allows you to hold a stateful value`. When calling this hook, it `will return the stateful value and a function to update it`.

Let’s look at an example of how to leverage useState to build the counter application:

```jsx
const App = () => {
  const [count, setCount] = useState(0)
  const increment = () => setCount((currentCount) => currentCount + 1)
  const decrement = () => setCount((currentCount) => currentCount - 1)
  const reset = () => setCount(0)
  return (
    <div className="App">
            <div>Counter: {count}</div>
            <div>
                <button onClick={increment}>+1</button>
                <button onClick={decrement}>-1</button>
                <button onClick={reset}>Reset</button>
      </div>
          
    </div>
  )
}
```

The preceding snippet leverages the useState hook to hold our counter state. When we first `call useState`, two things happenned :

- The state value is initiated as 0
- The count state variable is destructured;
- then, the same is done to the state updater function, called setCount

After this, we declare functions where we use the state updater function, setCount, to either increment, decrement, or reset our state variable.

Finally, we assign our state variable to the respective UI section and pass the callbacks to our buttons’ onClick events.

With that, we have built a simple counter application. Our application will start `rendering our count as 0`. `Every time we click` on the buttons, it will

- execute the respective state update,
- re-render our application,
- and display the new count value.

useState is the answer most of the time when you need any state in your React applications. Just don’t forget to apply the “what will I store in my state?” rule of thumb before!

Now, let’s see an example of how to manage state and build the same counter application by using the useReducer hook.

#### Managing state with useReducer

`useReducer` is the preferred option when we have a `more complex state`.
Before using the hook, we need to do some setup so that we have everything we need to send to our useReducer hook:

```jsx
const initialState = { count: 0 }

const types = {
  INCREMENT: 'increment',
  DECREMENT: 'decrement',
  RESET: 'reset',
}
const reducer = (state, action) => {
  switch (action) {
    case types.INCREMENT:
      return { count: state.count + 1 }
    case types.DECREMENT:
      return { count: state.count - 1 }
    case types.RESET:
      return { count: 0 }
    default:
      throw new Error('This type does not exist')
  }
}
```

we created three things:

- An initialState object. This object has a property count with 0 as its value.
- A types object that describes all the action types we will support.
- A reducer. This reducer is responsible for receiving our state and action. By matching that action with the expected type, we’ll be able to update the state

Now that the setup is done, let’s create our counter:

```jsx
const AppWithReducer = () => {
  const [state, dispatch] = useReducer(reducer, initialState)
  const increment = () => dispatch(types.INCREMENT)
  const decrement = () => dispatch(types.DECREMENT)
  const reset = () => dispatch(types.RESET)
  return (
    <div className="App">
            <div>Counter: {state.count}</div>
      <div>
        <button onClick={increment}>+1</button>
        <button onClick={decrement}>-1</button>
        <button onClick={reset}>Reset</button>
              
      </div>
          
    </div>
  )
}
```

The preceding snippet leverages the `useReducer` hook to hold our counter state. When we first call `useReducer`, three things are happened:

- We indicate to our hook what reducer should be used.
- We initialize our state with the initialState object.
- We destructure the state object and then the dispatch function, which allows us to dispatch an action from the useReducer hook.

Now, let’s picture the following scenario: what if you need your counter state to be accessible in other components?

You could pass them by props. But what if this state needs to be sent to five other components and different levels on the tree? Would you be prop-drilling it and passing it to every component?

To deal with this scenario and improve your code readability, React Context was created.

#### Sharing state with React Context

Context allows you to natively `share values between components without having to prop drill` them. Let’s learn how to build a context to handle our counter:

```jsx
import { useState, createContext } from 'react'
export const CountContext = createContext()
export const CountStore = () => {
  const [count, setCount] = useState(0)
  const increment = () => setCount((currentCount) => currentCount + 1)
  const decrement = () => setCount((currentCount) => currentCount - 1)
  const reset = () => setCount(0)
  return {
    count,
    increment,
    decrement,
    reset,
  }
}
const CountProvider = (children) => {
  return <CountContext.Provider value={CountStore()} {...children} />
}
export default CountProvider
```

We are doing three things:

- Using the `createContext function` to create our context.

- `Creating a store`. This store will be responsible for holding our state. Here, you can see we leverage the useState hook. At the end of the store, we return an object that contains the functions to do the state updates and create our state variable.

- `Creating a CountProvider`. This provider is responsible for creating a provider that will be used to wrap a component. This will `allow every component that is inside of that provider to access our CountStore values.`

Once this setup has been done, we need to make sure our components can access our context:

```tsx
root.render(
  <CountProvider>
        
    <App />
      
  </CountProvider>
)
```

The preceding snippet leverages `CountProvider`, which we created in the previous snippet, to wrap up our App component. This allows every component inside App to consume our context:

```tsx
import { CountContext } from './CountContext/CountContext'
const AppWithContext = () => {
  const { count, increment, decrement, reset } = useContext(CountContext)
  return (
    <div className="App">
            <div>Counter: {count}</div>
            <div>
                <button onClick={increment}>+1</button>
                <button onClick={decrement}>-1</button>
                <button onClick={reset}>Reset</button>
      </div>
          
    </div>
  )
}
```

Finally, in this snippet, we leverage the useContext hook to consume our CountContext. Since our component is rendered inside our custom provider, we can access the state held inside our context.

Every time the state updates inside of our context, React will make sure that every component that is consuming our context will re-render, as well as receive the state updates. This can often lead to unnecessary re-renders because if you are consuming only a variable from the state and for some reason another variable changes, then the context will force all consumers to re-render.

One of the downsides of context is that often, unrelated logic tends to get grouped. As you can see from the preceding snippets, it comes at the cost of a bit of boilerplate code.

Now, Context is still great, and it’s how React enables you to share state between components. However, it was not always around, so the community had to come up with ideas on how to enable state sharing. To do so, state management libraries were created.

### What do different state management libraries have in common?

One of the freedoms React offers you is that it does not impose any standards or practices for your development. While this is great, it also leads to different practices and implementations.

To make this easier and give developers some structure, state management libraries were created:

- Redux promotes an approach focused on stores, reducers, and selectors. This leads to needing to learn specific concepts and filling your project with a bunch of boilerplate code that might impact the code’s readability and increase code complexity.

- Zustand promotes a custom hook approach where each hook holds your state. This is by far the simplest solution and currently my favorite one. It synergizes with React and fully embraces hooks.

- MobX doesn’t impose an architecture but focuses on a functional reactive approach. This leads to more specific concepts, and the diversity of practices can lead the developer to run into the same struggles of code structure that they might already suffer from with React.

One common thing in all these libraries is that all of them are trying to solve the same type of issues that we tried to solve with React Context: a way to manage our shared state.

The state that is accessible to multiple components inside a React tree is often called global state. Now, global state is often misunderstood, which leads to the addition of unnecessary complexity to your code and often needing to resort to the libraries mentioned in this section.
