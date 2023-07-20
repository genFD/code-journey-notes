# React Query – Introducing, Installing, and Configuring It

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

`RQ`makes it easier for developers to overcome all the challenges that come with server state while making their applications faster, easier to maintain, and reducing many lines in their code.

- you will be introduced to React Query and understand why it was created.

- You will also get to know the main concepts of React Query – queries and mutations

- You will know all about React Query Devtools so that you can have a better developer experience while using React Query

## Notes

### What is React Query?

it's a library made of collection of hooks for fetching, caching, and updating server state in React.

React Query leverages queries and mutations to handle your server state

#### Query

A query is a request you make to an asynchronous source to fetch your data. Queries can be performed in React Query as long as you have a function that triggers the data-fetching request.

By allowing us to wrap our requests inside of promise-returning functions, React Query supports REST, GraphQL, and any other asynchronous data-fetching clients.

In React Query, the `useQuery custom hook` allows you to subscribe to queries.

#### Mutation

An operation that allows you to create, update, or delete your server state.

Like queries, as long as you have a function that triggers a mutation, React Query supports REST, GraphQL, and any other asynchronous data-fetching clients.

In React Query, the `useMutation custom hook` allows you to perform a mutation.

#### How does React Query solve my server state challenges?

Out of the box and with zero configurations, React Query supports all the following amazing features:

- `Caching`: After each query, data will be cached during a configurable time and can be reused throughout your application.

- `Query cancelation`: Your queries can be canceled, and you can perform an action after this cancelation

- `Optimistic updates`: During a mutation, you can easily update your state so that you can provide a better user experience to your users. You are also able to easily revert to a previous state if the mutation fails

- `Parallel queries`: If you need to execute one or more queries simultaneously, you can do so without any difficulty or impacting your cache

- `Dependent queries`: Sometimes, we need to execute a query after another one finishes. React Query makes this simple and avoids chaining promises

- `Paginated queries`: This UI pattern is made easier with React Query.You’ll find that using a paginated API, changing pages, and rendering the fetched data is super simple

- `Infinite queries`: Another UI pattern that is made easier by React Query. You can implement infinite scrolls into your UI and trust React Query to make your life easier when fetching data.

- `Scroll restoration`: Have you ever navigated from a page and, when navigated back, found that the page was scrolled to the exact point where you were before you navigated away? This is scroll restoration, and as long as your query results are cached, it will work out of the box.

- `Data refetching`: Need to trigger a refetch for your data? React Query allows you to do this with pretty much a line of code.

- `Data prefetching`: Sometimes, you can identify ahead of time what the needs and next actions of your users are. When this happens, you can trust React Query to help you prefetch that data ahead of time and cache it for you. This way, your user experience will be improved, and you will have happier users.

- `Tracking network mode and offline support`: Have you ever had to deal with scenarios where your user lost their internet connection while using your application? Well, don’t worry because React Query can track the current state of your network, and if a query fails because the user lost connection, it will be retried once the network returns.

### Installing React Query

```bash
$ npm i @tanstack/react-query
# or
$ pnpm add @tanstack/react-query
# or
$ yarn add @tanstack/react-query
```

It is recommended to also use our ESLint Plugin Query to help you catch bugs and inconsistencies while you code. You can install it via:

```bash
$ npm i -D @tanstack/eslint-plugin-query
# or
$ pnpm add -D @tanstack/eslint-plugin-query
# or
$ yarn add -D @tanstack/eslint-plugin-query
```

### Configuring React Query

To add React Query to your application, there are only two things you need to know:

1. QueryClient
2. QueryClientProvider

#### QueryClient

The `first thing you should do` when setting your application with React Query is to `create a QueryClient instance`. To do so, you need to import it from the @tanstack/react-query package and instantiate it:

```jsx
import { QueryClient } from '@tanstack/react-query'
const queryClient = new QueryClient()
```

There are four options we can pass as arguments when creating our QueryClient :

- `queryCache`: The query cache that this client will use throughout our application.
- `mutationCache`: The mutation cache that this client will use throughout our application.
- `logger`: The logger that this client will use to display errors, warnings, and useful information for debugging. When nothing is specified, then React Query will use the console object.
- `defaultOptions`: The default options that all queries and mutations will use throughout our application

When should you manually set each one of these arguments instead of using the default ones?

##### QueryCache and MutationCache

In the case of QueryCache, this is how it would look:

```ts
import { QueryCache } from '@tanstack/react-query'
const queryCache = new QueryCache({
  onError: (error) => {
    // do something on error
  },
  onSuccess: (data) => {
    // do something on success
  },
})
```

For MutationCache:

```ts
import { MutationCache } from '@tanstack/react-query'
const mutationCache = new MutationCache({
  onError: (error) => {
    // do something on error
  },
  onSuccess: (data) => {
    // do something on success
  },
  onMutate: (newData) => {
    // do something before the mutation
  },
})
```

- When you want to execute some code when there is an error, or when your query or your mutation succeeds, you can use `onSuccess` and `onError` functions..
- Also, in the case of mutations, you can also execute some code before the mutation by using `onMutate`.

- Then, you can pass the `mutationCache` or `queryCache` object to QueryClient when instantiating it

```ts
const queryClient = new QueryClient({
  mutationCache,
  queryCache,
})
```

##### Logger

Are you using logger outside of the console object in your project? Then, you might want to configure it in your QueryClient.

```ts
const logger = {
  log: (...args) => {
    // here you call your custom log function
  },
  warn: (...args) => {
    // here you call your custom warn function
  },
  error: (...args) => {
    // here you call your custom error function
  },
}
```

- We created a logger object.
- This object has three functions that React Query will call whenever it needs to log an error, warn about an error, or display error.

Then, all you need to do is pass this logger object to your QueryClient when instantiating it:

```ts
const queryClient = new QueryClient({
  logger,
})
```

##### defaultOptions

There are a number of options that are used as defaults for all the mutations or queries. `defaultOptions` allows you to override these defaults.
for example you can change the default `staleTime` :

```ts
const defaultOptions = {
  queries: {
    staleTime: Infinity,
  },
}
```

- We specified that `staleTime` for all queries will be Infinity.

Once this setup is done, all you need to do is pass this defaultOptions object to your QueryClient when instantiating it :

```ts
const queryClient = new QueryClient({
  defaultOptions,
})
```

#### QueryClientProvider

React Query allows you to share QueryClient with all the custom hooks it provides automatically, by creating its custom provider called `QueryClientProvider`.

```jsx
import { QueryClient, QueryClientProvider } from '@tanstack/react-query'
// Create a client
const queryClient = new QueryClient()
const App = () => {
  return (
    <QueryClientProvider client={queryClient}>
      <Counter />
    </QueryClientProvider>
  )
}
```

- We import your QueryClientProvider from the @tanstack/react-query package,
- Wrap your main component with it
- and pass the query client as a prop called `client`.

Your application is now ready to start using React Query.

### Adding React Query Devtools to your application

When debugging our applications, we often find ourselves thinking how amazing it would be to have a way to visualize what is happening inside our application. Well, with React Query, you don’t have to worry because it has its own developer tools, or devtools.

React Query Devtools allows you to see and understand the current state of all your queries and mutations. This will save you a lot of time debugging and avoid polluting all your code with unnecessary log functions, even if temporarily.

Depending on the type of project, you can install React Query Devtools in several ways:

```bash
npm i @tanstack/react-query-devtools
yarn add @tanstack/react-query-devtools
pnpm add @tanstack/react-query-devtools
```

There are two ways to use Devtools. They are Floating Mode and Embedded Mode.

- Floating Mode

Floating Mode will render the React Query logo floating in the corner of your screen. By clicking on it, you can toggle Devtools on or off.

To add Devtools in Floating Mode to your application, you need to import it and wrap the `<ReactQueryDevtools` component into the `QueryClientProvider`:

```tsx
<QueryClientProvider client={queryClient}>
  <ReactQueryDevtools initialIsOpen={false} />
  <Counter /> 
</QueryClientProvider>
```

- Embedded Mode will add Devtools embedded as a regular component in your application.

To use Devtools in Embedded Mode in your application, you need to import the `ReactQueryDevtoolsPanel` and wrap it component into the `QueryClientProvider`:

```tsx
<QueryClientProvider client={queryClient}>
     
  <ReactQueryDevtoolsPanel />
  <Counter />
     
</QueryClientProvider>
```

By default, Devtools is not included in production builds. You might want to load them in production to help you debug something.

If you decide to load Devtools in your production environment, you must delay loading it and instead load it dynamically. This is important to help reduce your application bundle size. It is also important to lazy load Devtools because when using our application in production, we might never want to use it, so we want to avoid adding stuff to our build that we will end up not using at all. To lazy load components in React, we can use React.lazy.

Here is how we can import Devtools using React.lazy:

```tsx
const ReactQueryDevtoolsProduction = React.lazy(() =>
  import('@tanstack/react-query-devtools/build/lib/index.prod.js').then(
    (d) => ({
      default: d.ReactQueryDevtools,
    })
  )
)
```

The preceding snippet wraps a dynamic import with React.lazy and assigns the return of the promise to ReactQueryDevtoolsProduction, so that we can lazily load it in our production environment without increasing our bundle size.

What is a dynamic import?

A dynamic import allows you to load a module from any place in your code asynchronously. This import will return a promise that, when fulfilled, returns an object containing the exports from the module.

If you are using a more modern bundler that supports package exports, then instead you can dynamically import your module like this:

```tsx
const ReactQueryDevtoolsProduction = React.lazy(() =>
  import('@tanstack/react-query-devtools/production').then((d) => ({
    default: d.ReactQueryDevtools,
  }))
)
```

- we change the path from where we will import our module to one that will work with more modern bundlers.

When using React.lazy and trying to render the component we just lazy loaded, React requires that the component should be wrapped with a Suspense component. This is important in a scenario where we want to show a fallback while our lazy-loaded component is pending.

What is Suspense?

Suspense allows you to render a loading indication in your UI while the component inside of it is not ready to be rendered yet.

Let see what we need to do to load our ReactQueryDevtoolsProduction component:

```tsx
<React.Suspense fallback={null}>
    
  <ReactQueryDevtoolsProduction />
</React.Suspense>
```

- We wrap our ReactQueryDevtoolsProduction component with Suspense so it can be lazy loaded. You can also see that we didn’t provide any fallback since what we are trying to load are Devtools, and we don’t need to add any pending state while the module is loading.

As you can see in the snippet, we wrap our ReactQueryDevtoolsProduction component with Suspense so it can be lazy loaded. You can also see that we didn’t provide any fallback since what we are trying to load are Devtools, and we don’t need to add any pending state while the module is loading.

Now, we don’t want to automatically load Devtools whenever we render our component. What we want is a way to toggle them in our application.

Since this is a production build, we don’t want to include a button there that might confuse our users. So, a potential way to handle this is by creating a function inside our window object called toggleDevtools.

This is how the React Query documentation suggests we do it:

```tsx
  const [showDevtools, setShowDevtools] = React.useState
    (false)
  React.useEffect(() => {
    window.toggleDevtools = () => setShowDevtools
      ((previousState) => !previousState)
  }, [])
  return (
    …
      {showDevtools && (
        <React.Suspense fallback={null}>
          <ReactQueryDevtoolsProduction />
        </React.Suspense>
      )}
    …
  );
```

- Creating a state variable to hold the current state of the Devtools. This state variable is updated whenever the user toggles the Devtools on or off.
- Running an effect where we assign the toggle function to our window.
- Inside our return, when our showDevtools is toggled on, since we are lazy loading our ReactQueryDevtoolsProduction component, we need to wrap it with Suspense to be able to render it
