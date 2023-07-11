# Fetching Data with React Query

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

React Query allows you to fetch, cache, and handle your server state by leveraging a custom hooks called `useQuery`.

### What is useQuery and how does it work?

A query is a request you send to an asynchronous source to fetch data.

To use the useQuery custom hook, you have to import it like this:

```tsx
import { useQuery } from '@tanstack/react-query'
```

Here is the useQuery syntax:

```tsx
const values = useQuery({
   queryKey: <insertQueryKey>,
   queryFn: <insertQueryFunction>,
 });
```

The `useQuery hook` accept one argument and object `{}`, that only needs two properties to work:

- A query key: A unique key used to identify your query
- A query function: A function that returns a promise

`Query key` is a unique value used by React Query to identify your queries. It is also how React Query caches your data in `QueryCache`. The query key also allows you to manually interact with the query cache.

The query key needs to be an array that can contain just one string or or other values, such as objects.

The values inside this query key array are serializable.

Here are some valid examples of query keys:

```tsx
useQuery({ queryKey: ['users'] })
useQuery({ queryKey: ['users', 10] })
useQuery({ queryKey: ['users', 10, { isVisible: true }] })
useQuery({ queryKey: ['users', page, filters] })
```

As good practice and to make your query key more distinct and easier to identify while reading through multiple useQuery hooks, you should add all the dependencies of your query as part of the query key.

Think of it as the same model of the dependency array you have on your useEffect hook. This is helpful for reading purposes as well as because the query key also allows React Query to refetch queries automatically when a dependency of the query changes.

The query key is hashed deterministically. This means that the order of the items inside the array matters.

```tsx
useQuery({ queryKey: ['users', 10, { page, filters }] })
useQuery({ queryKey: ['users', 10, { filters, page }] })
useQuery({ queryKey: ['users', 10, { page, random: undefined, filters }] })
```

- All these examples are the same query – the order of the array in the query key is the same. They are still inside an object and that object doesn’t change its position inside the query key array. Also, the random property is undefined, so when hashing the object, it is excluded.

```tsx
useQuery({ queryKey: ['users', 10, undefined, { page, filters }] })
useQuery({ queryKey: ['users', { page, filters }, 10] })
useQuery({ queryKey: ['users', 10, { page, filters }] })
```

- All these examples represent different queries because when the query key is hashed deterministically, these examples end up being completely different queries.

`A query function` is a function that returns a promise. This returned promise will either resolve and return the data or throw an error.

Because the query function just needs to return a promise, it makes React Query even more powerful because the query function can support any client capable of performing asynchronous data fetching. This means that both REST and GraphQL are supported, so you can have both options at the same time if you wish :

```ts
// graphql
import { useQuery } from "@tanstack/react-query";
import { request, gql } from "graphql-request";
const customQuery = gql`
  query {
    posts {
      data {
        id
        title
      }
    }
  }
`;
const fetchGQL = async () => {
  const endpoint = <add_endpoint_here>
  const {
    posts: { data },
  } = await request(endpoint, customQuery);
  return data;
};
 …
useQuery({
queryKey: ["posts"],
queryFn: fetchGQL
});
```

- We start by creating our GraphQL query and assigning it to our customQuery variable.
- Then, we create the fetchGQL function, which will be our query function.
- In our useQuery hook, we pass the respective query key to the hook and our fetchGQL function as the query function.

```ts
// REST
import axios from "axios";
const fetchData = async () => {
  const { data } = await axios.get(`https://test.builtwithdark.com/react-query-api`);
  return data;
};
…
useQuery({
    queryKey: ["api"],
    queryFn: fetchData,
  });
```

In the preceding snippet, we can see an example of using React Query with REST. This is what we are doing:

- We start by creating the fetchData function, which will be our query function.
- In our useQuery hook, we pass the respective query key to the hook and our fetchData function as the query function.

- Inline function

When you don’t have many parameters in your query key that need to be passed to your query function, you can use this pattern.

```ts
const fetchData = async (someVariable) => {
 const { data } = await axios.get(
   `https://test.builtwithdark.com/
     react-query-api/${someVariable}`
 );
 return data;
};
…
useQuery({
   queryKey: ["api", someVariable],
   queryFn: () => fetchData(someVariable),
 });
```

1. we start by creating a fetchData function that will receive a parameter called someVariable.

```ts
const fetchData = async (someVariable) => {}
```

2. This parameter is then used to complement the URL used to fetch the data.

```ts
const { data } = await axios.get(`https://test.builtwithdark.com/
     react-query-api/${someVariable}`)
```

3. When we get to our useQuery declaration, since we need our someVariable variable to be used as a dependency of our query, we include it in the query key.

```ts
useQuery({
   queryKey: ["api", someVariable],
   ...
   }
)
```

4. Finally, in the query function, we create an inline function that will call fetchData with our `someVariable` value.

Now, think about the use case where you have 12 parameters, it will not be good for code readbility.

To avoid this, you can use to the `QueryFunctionContext` object.

- QueryFunctionContext

Every time the query function is called, React Query will take care of automatically passing your query key to the query function (queryFn) as the `QueryFunctionContext` object.

```ts
const fetchData = async ({ queryKey }) => {
  const [_queryKeyIdentifier, someVariable] = queryKey
  const { data } = await axios.get(`https://danieljcafonso.builtwithdark.com/
     react-query-api/${someVariable}`)
  return data
}
useQuery({
  queryKey: ['api', someVariable],
  queryFn: fetchData,
})
```

- we start by creating our fetchData function. This function will be receiving QueryFunctionContext as a parameter, so from this object, we can immediately destructure queryKey.

- The query key is an array, so the order in which we passed the parameters we need in our function matters.

- In this example, we need the someVariable variable, which was passed as the second element of our array, so we destructure our array to get the second element.

- We then use someVariable to complement the URL used to fetch the data. When we get to our useQuery declaration, since we need our someVariable variable to be used as a dependency of our query, we include it in the query key.

- As it is included in the query key, it will automatically be sent to our query function.

The one downside this pattern might have is that with so many parameters, you will have to remember the order you added them to the query key to use them in the query function.

- One way to fix this issue is by sending an object with all the parameters you need in your query function

```ts
useQuery({
  queryKey: [{ queryIdentifier: 'api', someVariable }],
  queryFn: fetchData,
})
```

By passing an object as your query key, the object will be sent as the QueryFunctionContext object to your query function.

Then, in your function, you only need to do this:

```ts
const fetchData = async ({ queryKey }) => {
  const { someVariable } = queryKey[0]
}
```

- we destructure our queryKey from our QueryFunctionContext object
- since our object will be in the first position of the query key, we can destructure the value we need from our object there

#### What does useQuery return?

It returns a couple of values, that you can access by

- assigning the result of calling the hook

```ts
const values = useQuery()
values.data
values.error
...
```

- or destructure the valuues

```ts
const { data, error, status, fetchStatus }= useQuery(...);
```

1. data
2. error
3. status
4. fetchStatus

- data

The data property contains the successfully resolved data returned from your query function.

This is how you can use `data`:

```jsx
const App = () => {
  // We destructure our data variable from our useQuery hook.
  const { data } = useQuery({
    queryKey: ['api'],
    queryFn: fetchData,
  })
  // check if we already have data from our query. When we do, we render it.
  return <div>{data ? data.hello : null}</div>
}
```

If, for some reason, our query function promise rejects, then we can use `error`.

##### error

`error` lets you access the error object returned from your query function after failing.

This is how you can use `error`:

```ts
const App = () => {
  // destructure our error variable from our useQuery hook.
  const { error } = useQuery({
    queryKey: ['api'],
    queryFn: fetchData,
  })
  // check if we have any errors. If we do, we render the error message
  return <div>{error ? error.message : null}</div>
}
```

If, for some reason, the query function rejects and throws an error, then this error will be assigned to our error variable.

In both the data and error examples, we checked if they were defined. To make this easier we can use the status property.

##### status

`status` helps you track the status of the query.

Here are the states that the status variable can have :

- `loading`: No query attempt has finished and there is still no cached data yet.
- `error`: There was an error while performing a query. Whenever this is the status, the error property will receive the error returned from the query function.
- `success`: Your query was successful and it has returned data. Whenever this is the status, the data property will receive the successful data from the query function.

his is how you can use `status`

```ts
const App = () => {
  // We start by destructuring status, error and data from the useQuery hook
  const { status, error, data } = useQuery({
    queryKey: ['api'],
    queryFn: fetchData,
  })
  // We check if status is loading. This means that we still don’t have any data yet.        We render a loading indicator.
  if (status === 'loading') {
    return <div>Loading...</div>
  }
  // If our status is not loading, we check if there was any error during our query. If our status is error, then we need to destructure our error variable and display the error message.
  if (status === 'error') {
    return <div>There was an unexpected error: {error.message}</div>
  }
  // Finally, if our status is also not an error, then we can safely assume that our status equals success; therefore, we should have our data variable with the data our query function returned and we can display it to our user.
  return <div>{data.hello}</div>
}
```

For convenience, React Query also has some Boolean variants

- isLoading: Your status variable is in the loading state
- isError: Your status variable is in the error state
- isSuccess: Your status variable is in the success state

Let’s rewrite our previous snippet leveraging our status Boolean variants :

```jsx
const App = () => {
  const { isLoading, isError, error, data } = useQuery({
    queryKey: ['api'],
    queryFn: fetchData,
  })
  if (isLoading) {
    return <div>Loading...</div>
  }
  if (isError) {
    return <div>There was an unexpected error:{error.message}</div>
  }
  return <div>{data.hello}</div>
}
```

##### fetchStatus

If the user triggered a query but for some reason lost connection during the request, the status variable would stay pending in the loading state until the user got the connection back and the query was automatically retried.

To deal with this type of issue, in React Query v4, they introduced a new property called `networkMode`. This property can have three states, but by default, it will use `online`.

`fetchStatus` variable gives you information about your query function.

- `fetching`: Your query function is currently executing. This means that it’s currently fetching.

- `paused`: Your query wanted to fetch but due to a lost connection, it has now stopped executing. This means that it’s currently paused.

- `idle`: The query is not doing anything at the moment. This means that it’s currently idle.

This is how you can use it :

```jsx
const App = () => {
  // We start by destructuring the fetchStatus variable from the return of our useQuery hook.
  const { fetchStatus, data } = useQuery({
    queryKey: ['api'],
    queryFn: fetchData,
  })
  // We then check if the current state of our fetchStatus is paused. If this is true, then there is no network connection, so we let our user know.
  if (fetchStatus === 'paused') {
    return <div>Waiting for your connection to return…       </div>
  }
  // If the previous If check is false, then we can validate if the current state of our fetchStatus is fetching. If the previous If check is true, then right now, the query function is running, so we let our user know.
  if (fetchStatus === 'fetching') {
    return <div>Fetching…</div>
  }
  // if we are not fetching, then we can assume our query function’s fetchStatus is idle; therefore, it has already finished fetching, so we should have the returned data
  return <div>{data.hello} </div>
}
```

RQ also has some Boolean variants to help identify two of these statuses. They are as follows:

- `isFetching`: Your fetchStatus variable is in the fetching state
- `isPaused`: Your fetchStatus variable is in the paused state

```ts
const App = () => {
  const { isFetching, isPaused, data } = useQuery({
    queryKey: ['api'],
    queryFn: fetchData,
  })
  if (isPaused) {
    return <div>Waiting for your connection to return...       </div>
  }
  if (isFetching) {
    return <div>Fetching...</div>
  }
  return (
    <div>
             {data.hello}
          
    </div>
  )
}
```

##### Commonly used options explained

There are other options available besides the query key and query function that can be used to configure `useQuery()` :

- staleTime
- cacheTime
- retry
- retryDelay
- enabled
- onSuccess
- onError

###### staleTime

staleTime option is the duration in milliseconds until queried data is no longer considered fresh. When the set time elapses, a query is called stale.

While the query is fresh, it will be pulled from the cache without triggering a new request to update the cache. When the query is marked as stale, data will still be pulled from the cache but an automatic refetch of the query can be triggered.

By default, all queries use staleTime set to 0. This means that all cached data will be considered stale by default.

This is how we can configure staleTime:

```ts
useQuery({
  staleTime: 60000,
})
```

- the query data will be fresh for one minute.

###### cacheTime

The cacheTime option is the duration in milliseconds that the inactive data in your cache, remains in memory.

This is how to use the cacheTime option:

```ts
useQuery({
  cacheTime: 60000,
})
```

- after 1 minute, the query data will be garbage collected.

###### retry

The retry option is a value that indicates whether your query will retry or not when it fails. When true, it will retry until it succeeds. When false, it won’t retry.

This property can also be a number. When it is a number, the query will retry that specified number of times.

By default, all queries that are failing will be retried three times.

This is how to use the retry option:

```ts
useQuery({
  retry: false,
})
```

We can also configure the retry option this way:

```ts
useQuery({
  retry: 1,
})
```

###### retryDelay

retryDelay option is the delay to apply before the next retry attempt in milliseconds.

By default, React Query uses an exponential backoff delay algorithm to define the retry timing between retries.

This is how to use the retryDelay option:

```ts
useQuery({
  retryDelay: (attempt) => attempt * 2000,
})
```

###### enabled

enabled is a Boolean value that indicates when your query can run or not.

By default, this value is true, so all queries are enabled.

This is how we can use the enabled option:

```ts
useQuery({
  enabled: arrayVariable.length > 0,
})
```

In this snippet, we assign the return of the expression evaluation to the enabled option. This means that whenever the length of arrayVariable is greater than 0, this query will execute.

###### onSucces

The onSuccess option allows you to pass a function that will be triggered when your query is successful.

This is how we can use the onSuccess option:

```ts
useQuery({
  onSuccess: (data) => console.log('query was successful', data),
})
```

This function will be called with our data.

###### onError

The onError option allows you to pass a function that will be triggered when your query fails.

This is how we can use the onSuccess option:

```ts
useQuery({
  onError: (error) => console.log('query was unsuccessful', error.message),
})
```

When the query fails, this function will be called with the error.

### Refetching data with useQuery

Refetching is an important part of managing our server state. Sometimes, you need your data to be updated because your data has become stale or just because you haven’t interacted with your page in a while.

Manually or automatically, React Query supports and allows you to refetch your data.

#### Automatic refetching

There are a couple of options that allow React Query to perform data refetching automatically :
Whenever one of the dependencies of the query key changes, your query with be automatically refetched.

```ts
//
const [someVariable, setSomeVariable] = useState(0)
//
useQuery({
  queryKey: ['api', someVariable],
  queryFn: fetchData,
})
//
return (
  <button onClick={() => setSomeVariable(someVariable + 1)}> Click me </button>
)
```

- We define a useQuery hook that has someVariable as part of its query key.

- This query will be fetched on the initial render like usual, but when we click on our button, the someVariable value will change

- The query key will also change, which will trigger a query refetch for you to get your new data.

##### Refetching option

Here are the options related to data refetching that useQuery has enabled by default:

- `refetchOnWindowFocus`: Whenever you focus on your current window, this option triggers a refetch. For example, if you change tabs when you return to your application, React Query will trigger a refetch of your data.

- `refetchOnMount`: Whenever your hook mounts, this option triggers a refetch. For example, when a new component that uses your hook mounts, React Query will trigger a refetch of your data.

- `refetchOnReconnect`: Whenever you lose your internet connection, this option will trigger a refetch.

One thing that is important to note is that these options will only refetch your data by default if your data is marked as stale. This refetching of data even if its stale can be configured since all these options, excluding the Boolean value, also support receiving a string with a value of always. When the value of these options is always, it will always retrigger a refetch, even if the data is not stale.

This is how to configure them:

```ts
useQuery({
  refetchOnMount: 'always',
  refetchOnReconnect: true,
  refetchOnWindowFocus: false,
})
```

- For the `refetchOnMount` option, we always want our hook to refetch our data whenever any component using it mounts, even if the cached data is not stale

- For `refetchOnReconnect`, we want our hook to refetch our data whenever we regain connection after being offline, but only if our data is stale

- For `refetchOnWindowFocus`, we never want our hook to refetch our data on window focus

#### Manual refetching

There are two ways to manually trigger a query refetch. You could use

- QueryClient
- refetch function from the hook.

This is how you can trigger a data refetch using `QueryClient`:

```ts
// Using the useQueryClient hook to get access to our QueryClient
const queryClient = useQueryClient()

// refetchQueries allows you to trigger a refetch of all the queries that match the given query key. In this snippet, we are triggering a request for all queries that have the ["api"] query key.
queryClient.refetchQueries({ queryKey: ['api'] })
```

This is how you can trigger a data refetch using the `refetch` function:

```ts
// we are destructuring the refetch function from our useQuery hook
const { refetch } = useQuery({
  queryKey: ['api'],
  queryFn: fetchData,
})
//  we can call that function whenever we want to force that query to refetch.
refetch()
```

### Fetching dependent queries with useQuery

This is how you handle dependent queries:

```ts
const App = () => {
  const { data: firstQueryData } = useQuery({
  // We are creating a query that will have ["api"] as the query key and the fetchData function as the query function.
    queryKey: ["api"],
    queryFn: fetchData,
  });
// we are creating a Boolean variable called canThisDependentQueryFetch that will check if our previous query has the data we need.
  const canThisDependentQueryFetch = firstQueryData?.hello
    !== undefined;
  const { data: dependentData } = useQuery({
  // we are creating our second query with ["dependentAPI", firstQueryData?.hello] as the query key, the fetchDependentData function as the query function, and our canThisDependentQueryFetch as our Boolean variable for the enabled option
    queryKey: ["dependentApi", firstQueryData?.hello],
    queryFn: fetchDependentData,
    enabled: canThisDependentQueryFetch,
  });
…
```

- you only need the enabled option to make a query depend on another one.
