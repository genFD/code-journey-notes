# Integrating the API into the Application

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

- we will be learning how to consume the API via the application

When we say API, we mean the API backend server.

- We will learn how to fetch data from both the client and the server.

For the HTTP client, we will be using Axios, and for handling fetched data, we will be using the React Query library, which allows us to handle API requests and responses in our React application

**We will know how to make our application communicate with the API in a clean and organized way.**

## Notes

### Configuring the API client

For the API client of our application, we will be using `Axios`.
Why?

- It is supported in both the browser and the server
- It has an API for creating instances, intercepting requests and responses, canceling requests, and so on.

Let’s start by `creating an instance of Axios`, which will include some common things we want to be done on every request.

Create the `src/lib/api-client.ts` file and add the following:

```ts
import Axios from 'axios'
import { API_URL } from '@/config/constants'
export const apiClient = Axios.create({
  baseURL: API_URL,
  headers: {
    'Content-Type': 'application/json',
  },
})
apiClient.interceptors.response.use(
  (response) => {
    return response.data
  },
  (error) => {
    const message = error.response?.data?.message || error.message
    console.error(message)
    return Promise.reject(error)
  }
)
```

- Here, we have created an Axios instance where we define a common base URL and the headers we want to include in each request.

- Then, we attached a response interceptor where we want to extract the data property from the response and return that to our client.

- We also defined the error interceptor where we want to log the error to the console.

Having an Axios instance configured is, however, not enough to handle requests in React components elegantly. We would still need to handle calling the API, waiting for the data to arrive, and storing it in a state. `That’s where React Query comes into play`.

### Configuring React Query

React Query is a great library for handling async data and making it available in React components.

### Why React Query?

The main reason that React Query is a `great option for handling the async remote state` is the number of things it handles for us.

Imagine the following component, which loads some data from the API and displays it:

```tsx
const loadData = () => Promise.resolve('data')

const DataComponent = () => {
  const [data, setData] = useState()
  const [error, setError] = useState()
  const [isLoading, setIsLoading] = useState()
  useEffect(() => {
    setIsLoading(true)
    loadData()
      .then((data) => {
        setData(data)
      })
      .catch((error) => {
        setError(error)
      })
      .finally(() => {
        setIsLoading(false)
      })
  }, [])
  if (isLoading) return <div>Loading</div>
  if (error) return <div>{error}</div>
  return <div>{data}</div>
}
```

This is fine if we fetch data from an API only once, but in most cases, we need to fetch it from many different endpoints. We can see that there is a certain amount of boilerplate here:

- The same data, error, and isLoading pieces of state need to be defined
- Different pieces of state must be updated accordingly
- The data is thrown away as soon as we move away from the component

That’s where React Query comes in. We can update our component to the following:

```tsx
import { useQuery } from '@tanstack/react-query'

const loadData = () => Promise.resolve('data')

const DataComponent = () => {
  const { data, error, isLoading } = useQuery({
    queryFn: loadData,
    queryKey: ['data'],
  })
  if (isLoading) return <div>Loading</div>
  if (error) return <div>{error}</div>
  return <div>{data}</div>
}
```

- Notice how the `state handling is abstracted away` (no more setData(data)) from the consumer.We do not need to worry about storing the data, or handling loading and error states; everything is handled by React Query.

- Another benefit of React Query is its`caching mechanism`. For every query, we need to `provide a corresponding query key` that will be used to store the data in the cache. If we called the same query from multiple places, it would make sure the API requests happen only once.

#### Configuring React Query (react-query instance)

We just need to configure it for our application. The configuration needs a query client, which we can create in `src/lib/react-query.ts` and add the following:

```ts
import { QueryClient } from '@tanstack/react-query'
export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      retry: false,
      refetchOnWindowFocus: false,
      useErrorBoundary: true,
    },
  },
})
```

React Query comes with a default configuration that we can override during the query client creation. A full list of options can be found in the documentation.

- Now that we have created our query client, `we must include it in the provider`. Let’s head to `src/providers/app.tsx` and replace the content with the following:

```tsx
import { ChakraProvider, GlobalStyle } from '@chakra-ui/react'
import { QueryClientProvider } from '@tanstack/react-query'
import { ReactQueryDevtools } from '@tanstack/react-query-devtools'
import { ReactNode } from 'react'
import { ErrorBoundary } from 'react-error-boundary'
import { theme } from '@/config/theme'
import { queryClient } from '@/lib/react-query'
type AppProviderProps = {
  children: ReactNode
}
export const AppProvider = ({ children }: AppProviderProps) => {
  return (
    <ChakraProvider theme={theme}>
            
      <ErrorBoundary
        fallback={<div>Something went wrong!</div>}
        onError={console.error}>
                
        <GlobalStyle />
                
        <QueryClientProvider client={queryClient}>
                    
          <ReactQueryDevtools initialIsOpen={false} />
                    {children}
                  
        </QueryClientProvider>
              
      </ErrorBoundary>
          
    </ChakraProvider>
  )
}
```

- we are `importing and adding QueryClientProvider`, which will make the query client and its configuration available for queries and mutations. Notice how `we are passing our query client instance as the client prop`.

- We are also adding ReactQueryDevtools, which is a widget that allows us to inspect all queries. It only works in development, and that is very useful for debugging

Our react-query setup is in place, we can start implementing the API layer for the features.

### Creating the API layer for the features

The API layer will be defined in the api folder of every feature. An API request can be either a query or a mutation.

- A query describes requests that only fetch data (GET).
- A mutation describes an API call that mutates data on the server (like POST, PUT, etc).

#### Jobs

For the jobs feature, we have three API calls:

- GET /jobs
- GET /jobs/:jobId
- POST /jobs

##### Get jobs

Let’s start with the API call that fetches jobs. To define it in our application, let’s create the `src/features/jobs/api/get-jobs.ts` file and add the following:

```ts
import { useQuery } from '@tanstack/react-query'
import { apiClient } from '@/lib/api-client'
import { Job } from '../types'
type GetJobsOptions = {
  params: {
    organizationId: string | undefined
  }
}
export const getJobs = ({ params }: GetJobsOptions): Promise<Job[]> => {
  return apiClient.get('/jobs', {
    params,
  })
}
export const useJobs = ({ params }: GetJobsOptions) => {
  const { data, isFetching, isFetched } = useQuery({
    queryKey: ['jobs', params],
    queryFn: () => getJobs({ params }),
    enabled: !!params.organizationId,
    initialData: [],
  })
  return {
    data,
    isLoading: isFetching && !isFetched,
  }
}
```

As we can see, there are a few things going on:

1. We are defining the type for the request options. There, we can pass organizationId to specify the organization for which we want to get the jobs.

2. We are defining the getJobs function, which is the request definition for getting jobs.

3. We are defining the useJobs hook by using useQuery from react-query. The useQuery hook returns many different properties, but we want to expose only what is needed by the application. Notice how by using the enabled property, we are telling useQuery to run only if organizationId is provided. This means that the query will wait for organizationId to exist before fetching the data.

Since we will be using it outside the feature, let’s make it available at `src/features/jobs/index.ts`:

```ts
export * from './api/get-jobs'
```

##### Get job details

The get job request should be straightforward. Let’s create the `src/features/jobs/api/get-job.ts` file and add the following:

```ts
import { useQuery } from '@tanstack/react-query'
import { apiClient } from '@/lib/api-client'
import { Job } from '../types'
type GetJobOptions = {
  jobId: string
}
export const getJob = ({ jobId }: GetJobOptions): Promise<Job> => {
  return apiClient.get(`/jobs/${jobId}`)
}
export const useJob = ({ jobId }: GetJobOptions) => {
  const { data, isLoading } = useQuery({
    queryKey: ['jobs', jobId],
    queryFn: () => getJob({ jobId }),
  })
  return { data, isLoading }
}
```

- we are defining and exporting the getJob function and the useJob query

We want to consume this API request outside the feature, so we have to make it available by re-exporting it from `src/features/jobs/index.ts`:

```ts
export * from './api/get-job'
```

##### Create job

whenever we change something on the server, it should be considered a mutation. With that said, let’s create the `src/features/jobs/api/create-job.ts` file and add the following:

```ts
import { useMutation } from '@tanstack/react-query'
import { apiClient } from '@/lib/api-client'
import { queryClient } from '@/lib/react-query'
import { Job, CreateJobData } from '../types'

type CreateJobOptions = {
  data: CreateJobData
}
export const createJob = ({ data }: CreateJobOptions): Promise<Job> => {
  return apiClient.post(`/jobs`, data)
}
type UseCreateJobOptions = {
  onSuccess?: (job: Job) => void
}
export const useCreateJob = ({ onSuccess }: UseCreateJobOptions = {}) => {
  const { mutate: submit, isLoading } = useMutation({
    mutationFn: createJob,
    onSuccess: (job) => {
      queryClient.invalidateQueries(['jobs'])
      onSuccess?.(job)
    },
  })
  return { submit, isLoading }
}
```

1. We define the CreateJobOptions type of the API request. It will require a `data object that contains all the fields that are required for creating a new job`.

2. We define the `createJob function`, which makes the request to the server.

3. We define `UseCreateJobOptions`, which accepts an optional callback to call if the request succeeds. This may become useful whenever we want to show a notification, redirect the user, or do anything that is not directly related to the API request.

4. We are defining the `useCreateJob hook`, which uses useMutation from react-query. As defined in the type, it accepts an optional onSuccess callback that gets called if the mutation succeeds.

5. To create the mutation, we provide the createJob function as mutationFn

6. We define onSuccess of useMutation, where we invalidate all job queries once a new job is created. Invalidating queries means that we want to set them as invalid in the cache. If we need them again, we will have to fetch them from the API.

7. We are reducing the API surface of the useCreateJob hook to things that are used by the application, so we are just exposing submit and isLoading. We can always expose more things in the future if we notice we need them.

We don’t have to export this request from the `index.ts` file since it is used only within the jobs feature folder.

#### Organizations

For the organizations feature, we have one API call:

- GET /organizations/:organizationId

##### Get organization details

Let’s create `src/features/organizations/api/get-organization.ts` and add the following:

```ts
import { useQuery } from '@tanstack/react-query'
import { apiClient } from '@/lib/api-client'
import { Organization } from '../types'
type GetOrganizationOptions = {
  organizationId: string
}
export const getOrganization = ({
  organizationId,
}: GetOrganizationOptions): Promise<Organization> => {
  return apiClient.get(`/organizations/${organizationId}`)
}
export const useOrganization = ({ organizationId }: GetOrganizationOptions) => {
  const { data, isLoading } = useQuery({
    queryKey: ['organizations', organizationId],
    queryFn: () => getOrganization({ organizationId }),
  })
  return { data, isLoading }
}
```

- we are defining a query that will fetch the organization based on the organizationId property we pass

this query will also be used outside the organizations feature, let’s also re-export from `src/features/organizations/index.ts`

```ts
export * from './api/get-organization'
```

### Using (Consuming ) the API layer in the application

To be able to build the UI without the API functionality, we used test data on our pages. Now, we want to replace it with the real queries and mutations that we just made for communicating with the API.

#### Public organization

Let’s open `src/pages/organizations/[organizationId]/index.tsx` and remove the following:

```tsx
import { getJobs, getOrganization } from '@/testing/test-data'
```

Now, we must load the data from the API. We can do that by importing getJobs and getOrganization from corresponding features. Let’s add the following:

```tsx
import { JobsList, Job, getJobs } from '@/features/jobs'
import { getOrganization, OrganizationInfo } from '@/features/organizations'
```

The new API functions are a bit different, so we need to replace the following code:

```tsx
const [organization, jobs] = await Promise.all([
  getOrganization(organizationId).catch(() => null),
  getJobs(organizationId).catch(() => [] as Job[]),
])
```

with the following:

```tsx
const [organization, jobs] = await Promise.all([
  getOrganization({ organizationId }).catch(() => null),
  getJobs({
    params: {
      organizationId: organizationId,
    },
  }).catch(() => [] as Job[]),
])
```

#### Public job

The same process should be repeated for the public job page.

remove the following:

```tsx
import { getJob, getOrganization } from '@/testing/test-data'
```

Now, let’s import getJob and getOrganization from the corresponding features:

```tsx
import { getJob, PublicJobInfo } from '@/features/jobs'
import { getOrganization } from '@/features/organizations'
```

Then, inside getServerSideProps, we need to update the following:

```tsx
const [organization, job] = await Promise.all([
  getOrganization({ organizationId }).catch(() => null),
  getJob({ jobId }).catch(() => null),
])
```

#### Dashboard jobs

the only thing we need to do is to update the imports so that we no longer load jobs from test data but from the API.

Let’s import useJobs from the jobs feature instead of the test data by updating the following lines in `src/pages/dashboard/jobs/index.tsx`:

```tsx
import { JobsList, useJobs } from '@/features/jobs'
import { useUser } from '@/testing/test-data'
```

Since the newly created useJobs hook is a bit different than the test-data one, we need to update the way it is being used, as follows:

```tsx
const jobs = useJobs({
  params: {
    organizationId: user.data?.organizationId ?? '',
  },
})
```

#### Dashboard job

The job details page in the dashboard is also very straightforward.

In `src/pages/dashboard/jobs/[jobId].tsx`, let’s remove useJob, which was imported from test-data:

```tsx
import { useJob } from '@/testing/test-data'
```

Now, let’s import it from the jobs feature:

```tsx
import { DashboardJobInfo, useJob } from '@/features/jobs'
```

Here, we need to update how useJob is consumed:

```tsx
const job = useJob({ jobId })
```

#### Create job ()

For the job creation, we will need to update the form, which, when submitted, will create a new job.

Currently, the form is not functional, so we need to add a couple of things.

Let’s open `src/features/jobs/components/create-job-form/create-job-form.tsx` and replace the content with the following:

```tsx
import { Box, Stack } from '@chakra-ui/react'
import { useForm } from 'react-hook-form'
import { Button } from '@/components/button'
import { InputField } from '@/components/form'
import { useCreateJob } from '../../api/create-job'
import { CreateJobData } from '../../types'
export type CreateJobFormProps = {
  onSuccess: () => void
}
export const CreateJobForm = ({ onSuccess }: CreateJobFormProps) => {
  const createJob = useCreateJob({ onSuccess })
  const { register, handleSubmit, formState } = useForm<CreateJobData>()
  const onSubmit = (data: CreateJobData) => {
    createJob.submit({ data })
  }
  return (
    <Box w="full">
            
      <Stack as="form" onSubmit={handleSubmit(onSubmit)} w="full" spacing="8">
                
        <InputField
          label="Position"
          {...register('position', {
            required: 'Required',
          })}
          error={formState.errors['position']}
        />
                
        <InputField
          label="Department"
          {...register('department', {
            required: 'Required',
          })}
          error={formState.errors['department']}
        />
                
        <InputField
          label="Location"
          {...register('location', {
            required: 'Required',
          })}
          error={formState.errors['location']}
        />
                
        <InputField
          type="textarea"
          label="Info"
          {...register('info', {
            required: 'Required',
          })}
          error={formState.errors['info']}
        />
                
        <Button
          isDisabled={createJob.isLoading}
          isLoading={createJob.isLoading}
          type="submit">
                    Create         
        </Button>
              
      </Stack>
          
    </Box>
  )
}
```

1. We are using the useForm hook to handle the form’s state.
2. We are importing and using the useCreateJob API hook we previously defined to submit the request.
3. When the mutation succeeds, the onSuccess callback is called.

The create job form requires the user to be authenticated. Since we didn’t implement the authentication system yet, you can use the MSW dev tools to authenticate with the test user to try the form submission.

### Summary

First, we defined an API client that allows us to unify the API requests. Then, we introduced React Query, a library for handling asynchronous states. Using it reduces boilerplate and simplifies the code base significantly.

Finally, we declared the API requests, and then we integrated them into the application.
