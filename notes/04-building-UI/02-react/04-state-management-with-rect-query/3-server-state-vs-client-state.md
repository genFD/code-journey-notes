# Server State versus Client State

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

- Understand why we refer mostly to our state as global state and why we should adjust our mental models to include client and server states instead.

- Review what each type of state is responsible for and how to differentiate them in an application and understand the challenges that led to the creation of React Query.

- Split global state into the client state and the server state by applying the mental models you will have just learned

- Understand all the challenges created by having a server state in your application and prepare to overcome them with React Query.

### What is global state?

When you lift the state to the nearest parent because you want to make it accessible to the children of that component, or find a common place where this state can exist and be accessible everywhere by all the components we want. This state is often called global state.

This snippet shows an example of what some typical global state can look like.

```jsx
const theme = {
  DARK: 'dark',
  LIGHT: 'light',
}
export const GlobalStore = () => {
  const [selectedTheme, setSelectedTheme] = useState(theme.LIGHT)
  const [serverData, setServerData] = useState(null)
  const [isLoadingData, setIsLoadingData] = useState(false)
  const toggleTheme = () => {
    setSelectedTheme((currentTheme) =>
      currentTheme === theme.LIGHT ? theme.DARK : theme.LIGHT
    )
  }
  const fetchData = (name = 'Daniel') => {
    setIsLoadingData(true)
    fetch(`<insert_url_here>/${name}`)
      .then((response) => response.json())
      .then((responseData) => {
        setServerData(responseData)
      })
      .finally(() => {
        setIsLoadingData(false)
      })
      .catch(() => setIsLoadingData(false))
  }
  useEffect(() => {
    fetchData()
  }, [])
  return {
    selectedTheme,
    toggleTheme,
    serverData,
    isLoadingData,
    fetchData,
  }
}
```

We have a store responsible for managing theme selection, fetching data, and tracking the loading state of this fetching request.

- A state variable called selectedTheme to manage the selected theme
- A state variable called serverData to display the data that was returned from our API request
- A state variable called isLoadingData to display whether the current loading state of our API request is still loading
- A function called toggleTheme to allow us to toggle between light and dark modes
- A fetchData function to allow us to fetch the given data and set our loading state as true or false depending on the state of the request
- A useEffect hook that will trigger the initial data fetching to provision our serverData state

`useEffect` is a React hook that allows you to perform side effects in your components.

All these are returned from our store so that the consumers of this context can access them from all over our application as long as they subscribe to our context.

Now, from a first look, there seems to be nothing wrong with this state, and it might be enough for most applications. The thing is, most of the time, this state will end up growing due to new development needs.

Imagine we need a secondary theme, and we need to add another state variable called secondaryTheme. Our code would look a lot like this:

```jsx
const [selectedTheme, setSelectedTheme] = useState(theme.LIGHT);
const [secondaryTheme, setSecondaryTheme] = useState(theme.LIGHT);
…
  const toggleSecondaryTheme = () => {
    setSecondaryTheme((currentTheme) =>
      currentTheme === theme.LIGHT ? theme.DARK :
        theme.LIGHT
    );
  };
  const toggleTheme = () => {
    setSelectedTheme((currentTheme) =>
      currentTheme === theme.LIGHT ? theme.DARK :
        theme.LIGHT
    );
  };
```

In this snippet, we added our `secondaryTheme` state variable, and it works very much like `selectedTheme`.

Every time we trigger a state update, any component that consumes this state will be forced to re-render to receive the new state updates. What does this mean to us?

Let’s imagine

- We have two components (let’s call them Component A and Component B) consuming this context.

- Component B only destructures the selectedTheme state while Component A destructures everything.

- If Component A triggers a state update on secondaryTheme, then Component B will also re-render because React noticed an update inside the context that both of them share.

```md
`memo `is a function that you can wrap your component in to define a memoized version of it. This will guarantee that your component doesn’t re-render unless its props have changed.

`useMemo` is a React hook that allows you to memoize a value. Usually, the value we want to memoize is the result of an expensive calculation.
```

This is how React Context works, and we can’t change this. We could argue that we could either split the context, split the subscribing component into two components and wrap the second one with memo, or just wrap our return with the useMemo hook. Sure, this would probably fix our issue, but we are only dealing with the changes in one type of state that creates the global state.

Now, imagine we needed to add another API request context. Once again, the context grows and we end up with the same issue we had with the themes.

As you may already understand by now, state organization can be a nightmare sometimes.

Our global state is often a mix between client state and server state. In the upcoming sections, you will understand what each one of these states is, and we’ll focus on server state to finally understand why React Query has become so popular and made our lives so much easier as a developer.

### What is client state?

Client state is the state that is owned by your application.

Here are a couple of things that help define your client state :

- This state is synchronous, which means you can access it without any waiting time and by using synchronous APIs.
- It is local; therefore, it only exists in your application.
- It is temporary, so it may get lost upon a page reload and is generally non-persistent between sessions.

In our GlobalStore, `selectedTheme` falls into that category.

- We don't need to wait to get its value. It is synchronous.
- electedTheme only exist in our application
- If we don’t persist it in local storage or check the browser preferences, then its value will be lost between page reloads.

To manage this type of state, we can use anything from React Context to third-party libraries such as Redux, Zustand, or MobX when things start to become harder to organize and maintain.

`serverData` falls into the server state category :

- It does not exist only in our application. It exists on a database somewhere.

- It won't be lost on page reload. The database still keeps the data, so when we reload, it will be fetched once again.

- We need to wait to get it. We need to call fetch to get this data

### What is server state?

Server state is the type of state that is stored on your server. Here are a couple of things that help identify your server state:

- This state is asynchronous, which means you need to use asynchronous APIs for fetching and updating it.
- It is persisted remotely – most of the time on a database or external place you don’t own or control.
- There are no guarantees that this state is up to date in your application because most often, you have shared ownership of it, and it might be changed by others that are consuming it as well.

Now, you might be looking back at `GlobalStore`and asking the following: if selectedTheme is client state and data is server state, then what is the `isLoadingData` state variable?

Well, this is a `derived state variable`. This means that its state `will always depend on the current status of our serverData fetching request`.

If we fetch data, then isLoadingData will be true; once we are done fetching data, then isLoadingData will go back to false.

Now imagine needing one of these derived state variables for every type of server state variable you have in your application, or a scenario in which you needed to handle errors. That would be untenable. This is one of the challenges that the server state brings to your application. Caching data is another one.

### Understanding common challenges with server state

#### Caching

Being able to reuse data that you previously fetched to avoid refetching it from the server once again.

Now, you might be thinking that this sounds simple, but consider the following things:

- While keeping your application responsive, you need to update your cache in the background.
- You need to be able to evaluate when your cache data has become stale and needs to be updated.
- Once data has not been accessed for a while, you must garbage-collect this data.
- You might want to initialize your cache with some template data before data is fetched.

As you can see, caching comes with its fair share of issues, and imagine having to solve all of these by yourself.

#### Optimistic updates

We all hate filling out a form and ending up watching a loading spinner while our application in the background performs the mutation, refetches the data, and updates the UI.

To make the user experience better, we can resort to optimistic updates.

An optimistic update is when during an ongoing mutation, we update our UI to show how it will look after that mutation is complete, although that mutation is still not confirmed as complete. Basically, we are optimistic that this data will change and be what we are expecting it to be after the mutation, so we save our users some time and give them a UI that they will end up seeing earlier.

Now, imagine implementing this. While doing a mutation, you would need to update the server state in your application with the way we expect it to be after the mutation is successful. This would make the UI more responsive for the user and they can start interacting with it earlier. After the mutation is successful, you need to retrigger a manual refetch for the server state so that you actually have the updated state in your application. Now, picture a scenario in which the mutation fails. You would need to manually roll back your state to the previous version before your optimistic update.

Optimistic updates give an amazing user experience to your users but having to manage all the success and error scenarios, plus keeping your server date updated, can be a hard thing to do.

#### Deduping requests

When fetching for the exact same type of data, if we trigger multiple requests for the same data, we want only one of those requests to be sent and avoid polluting our user network with unnecessary requests.

Now, imagine having to implement this by yourself. You would need to be aware of all the requests currently being done in your application. When one of those requests exactly matches another one, then you would need to cancel that second, third, or fourth request.

#### Performance optimization

Here are some common patterns that you might need for specific optimization of your server state management.

- Lazy loading: You might only want a specific data fetching request to be done once a certain condition is met.

- Infinite scrolling: When dealing with huge lists, infinite scrolling is a very common pattern where you just progressively load more data into your server state.

- Paginated data: To help structure large datasets, you can opt to paginate your data. This means that whenever a user decides to move from page 1 to page 2, you will need to fetch the corresponding data for this page.
