# Testing

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

We have finally finished developing our application. Before we release it to production, we want to ensure that everything works as expected.

- we will learn how to test our application by using different testing approaches. This will give us the confidence to refactor the application, build new features, and modify the existing ones without worrying about breaking the current application behavior.

**We will know how to test our application with different methods and tools.**

## Notes

### Unit testing

Unit testing is a testing method where application units are tested in isolation without depending on other parts.

For unit testing, we will use Jest, which is the most popular framework for testing JavaScript applications.

In our application, we will unit test the notifications store.

Let’s open the `src/stores/notifications/__tests__/notifications`.test.ts file and add the following:

```ts
import { notificationsStore, Notification } from '../notifications'
const notification = {
  id: '123',
  title: 'Hello World',
  type: 'info',
  message: 'This is a notification',
} as Notification
describe('notifications store', () => {
  it('should show and dismiss notifications', () => {
    // 1
    expect(notificationsStore.getState().notifications.length).toBe(0) // 2
    notificationsStore.getState().showNotification(notification)
    expect(notificationsStore.getState().notifications).toContainEqual(
      notification
    ) // 3
    notificationsStore.getState().dismissNotification(notification.id)
    expect(notificationsStore.getState().notifications).not.toContainEqual(
      notification
    )
  })
})
```

The notifications test works as follows:

1. We assert that the notifications array is initially empty.
2. Then, we fire the showNotification action and test that the newly created notification exists in the notifications array.
3. Finally, we call the dismissNotification function to dismiss the notification and make sure the notification is removed from the notifications array.

To run unit tests, we can execute the following command:

```sh
npm run test
```

Another use case for unit testing would be various utility functions and reusable components, including logic that could be tested in isolation. However, in our case, we will test our components mostly with integration tests.

### Integration testing

Integration testing is a testing method where multiple parts of the application are tested together. Integration tests are generally more helpful than unit tests, and most application tests should be integration tests.

Integration tests are more valuable because they can give more confidence in our application since we are testing the functionality of different parts, the relationship between them, and how they communicate.

For integration testing, we will use Jest and the React Testing Library. This is a great approach to testing features of the application in the same way the user would use it.

In `src/testing/test-utils.ts`, we can define some utilities we can use in our tests. We should also re-export all utilities provided by the React Testing Library from here so that we can easily reach out to them whenever they are needed in our tests. Currently, in addition to all the functions provided by the React Testing Library, we are also exporting the following utilities:

- `appRender` is a function that calls the render function from the React Testing Library and adds AppProvider as a wrapper. We need this because, in our integration tests, our components rely on multiple dependencies defined in AppProvider, such as the React Query context, notifications, and more. Providing AppProvider as a wrapper will make it available when we render the component during testing.

- `checkTableValues` is a function that goes through all the cells in the table and compares each value with the corresponding value from the provided data, ensuring that all the information is displayed in the table.

- `waitForLoadingToFinish` is a function that waits for all loading spinners to disappear before we can proceed further with our tests. This is useful when we must wait for some data to be fetched before we can assert the values.

Another file worth mentioning is `src/testing/setup-tests.ts`, where we can configure different initialization and cleanup actions. In our case, it helps us initialize and reset the mocked API between tests.

We can split our integration tests by pages and test all the parts on each page. The idea is to perform integration tests on the following parts of our application:

- Dashboard jobs page
- Dashboard job page
- Create job page
- Login page
- Public job page
- Public organization page

#### Dashboard jobs page

he functionality of the dashboard jobs page is based on the currently logged-in user. Here, we are fetching all the jobs of the user’s organization and displaying them in the jobs table.

Let’s start by opening the `src/__tests__/dashboard-jobs-page.test.tsx` file and adding the following:

```tsx
import DashboardJobsPage from '@/pages/dashboard/jobs'
import { getUser } from '@/testing/mocks/utils'
import { testData } from '@/testing/test-data'
import {
  appRender,
  checkTableValues,
  screen,
  waitForLoadingToFinish,
} from '@/testing/test-utils'
// 1
jest.mock('@/features/auth', () => ({
  useUser: () => ({ data: getUser() }),
}))
describe('Dashboard Jobs Page', () => {
  it('should render the jobs list', async () => {
    // 2
    await appRender(<DashboardJobsPage />) // 3
    expect(screen.getByText(/jobs/i)).toBeInTheDocument() // 4
    await waitForLoadingToFinish() // 5
    checkTableValues({
      container: screen.getByTestId('jobs-list'),
      data: testData.jobs,
      columns: ['position', 'department', 'location'],
    })
  })
})
```

The test is working as follows:

1. Since loading the jobs depends on the currently logged-in user, we need to mock the useUser hook to return the proper user object.
2. Then, we render the page.
3. Then, we make sure the jobs page’s title is displayed on the page.
4. To get the loaded jobs, we need to wait for them to finish loading.
5. Finally, we assert the jobs values in the table.

#### Dashboard job page

The functionality of the dashboard job page is that we want to load the job data and display it on the page.

Let’s start by opening the `src/__tests__/dashboard-job-page.test.tsx` file and adding the following:

```tsx
import DashboardJobPage from '@/pages/dashboard/jobs/[jobId]'
import { testData } from '@/testing/test-data'
import { appRender, screen, waitForLoadingToFinish } from '@/testing/test-utils'
const job = testData.jobs[0]
const router = {
  query: {
    jobId: job.id,
  },
}
// 1
jest.mock('next/router', () => ({
  useRouter: () => router,
}))
describe('Dashboard Job Page', () => {
  it('should render all the job details', async () => {
    // 2
    await appRender(<DashboardJobPage />)
    await waitForLoadingToFinish()
    const jobPosition = screen.getByRole('heading', {
      name: job.position,
    })
    const info = screen.getByText(job.info) // 3
    expect(jobPosition).toBeInTheDocument()
    expect(info).toBeInTheDocument()
  })
})
```

The test works as follows:

1. Since we are loading job data based on the jobId URL parameter, we need to mock the useRouter hook to return the proper job ID.
2. Then, we render the page and wait for the data to load by waiting for all loaders to disappear from the page.
3. Finally, we check that the job data is displayed on the page.

#### Job creation page

The job creation page contains a form which, when submitted, calls the API endpoint that creates a new job on the backend. When the request succeeds, we redirect the user to the dashboard jobs page and show the notification about successful job creation.

Let’s start by opening the `src/__tests__/dashboard-create-job-page.test.tsx` file and adding the following:

```tsx
import DashboardCreateJobPage from '@/pages/dashboard/jobs/create'
import { appRender, screen, userEvent, waitFor } from '@/testing/test-utils'
const router = {
  push: jest.fn(),
}
// 1
jest.mock('next/router', () => ({
  useRouter: () => router,
}))
const jobData = {
  position: 'Software Engineer',
  location: 'London',
  department: 'Engineering',
  info: 'Lorem Ipsum',
}
describe('Dashboard Create Job Page', () => {
  it('should create a new job', async () => {
    // 2
    appRender(<DashboardCreateJobPage />)
    const positionInput = screen.getByRole('textbox', {
      name: /position/i,
    })
    const locationInput = screen.getByRole('textbox', {
      name: /location/i,
    })
    const departmentInput = screen.getByRole('textbox', {
      name: /department/i,
    })
    const infoInput = screen.getByRole('textbox', {
      name: /info/i,
    })
    const submitButton = screen.getByRole('button', {
      name: /create/i,
    }) // 3
    userEvent.type(positionInput, jobData.position)
    userEvent.type(locationInput, jobData.location)
    userEvent.type(departmentInput, jobData.department)
    userEvent.type(infoInput, jobData.info) // 4
    userEvent.click(submitButton) // 5
    await waitFor(() =>
      expect(screen.getByText(/job created!/i)).toBeInTheDocument()
    )
  })
})
```

The test works as follows:

1. First, we need to mock the useRouter hook to contain the push method because it is used for navigating to the jobs page after the submission.
2. Then, we render the page component.
3. After that, we get all the inputs and insert values into them.
4. Then, we submit the form by simulating the click event on the Submit button.
5. After the submission, we need to wait for the Job Created notification to appear in the document.

#### Public organization page

For the organization page, since we are rendering it on the server, we need to fetch the data on the server and display it on the page.

Let’s start by opening the `src/__tests__/public-organization-page.test.tsx` file and defining the skeleton of the test suite, as follows:

```tsx
import PublicOrganizationPage, {
  getServerSideProps,
} from '@/pages/organizations/[organizationId]';
import { testData } from '@/testing/test-data';
import {
  appRender,
  checkTableValues,
  screen,
} from '@/testing/test-utils';
const organization = testData.organizations[0];
const jobs = testData.jobs;
describe('Public Organization Page', () => {
  it('should use getServerSideProps that fetches and
    returns the proper data', async () => {
  });
  it('should render the organization details', async () => {
  });
  it('should render the not found message if the
    organization is not found', async () => {
  });
});
```

Now, we will focus on each test in the test suite.

First, we want to test that the`getServerSideProps` function fetches the right data and returns it as props, which will be provided on the page:

```tsx
it('should use getServerSideProps that fetches and returns the proper data', async () => {
  const { props } = await getServerSideProps({
    params: {
      organizationId: organization.id,
    },
  } as any)
  expect(props.organization).toEqual(organization)
  expect(props.jobs).toEqual(jobs)
})
```

we are `calling the getServerSideProps function` and `asserting that the returned value`contains the corresponding data.

In the second test, we want to `verify that the data provided as props to the PublicOrganizationPage component is rendered` properly:

```tsx
it('should render the organization details', async () => {
  appRender(<PublicOrganizationPage organization={organization} jobs={jobs} />)
  expect(
    screen.getByRole('heading', {
      name: organization.name,
    })
  ).toBeInTheDocument()
  expect(
    screen.getByRole('heading', {
      name: organization.email,
    })
  ).toBeInTheDocument()
  expect(
    screen.getByRole('heading', {
      name: organization.phone,
    })
  ).toBeInTheDocument()
  checkTableValues({
    container: screen.getByTestId('jobs-list'),
    data: jobs,
    columns: ['position', 'department', 'location'],
  })
})
```

In this test, we are rendering the page component and verifying that all the values are displayed on the page.

In the third test of the test suite, we want to assert that if the organization does not exist, we want to display the not found message:

```tsx
it('should render the not found message if the organization is not found', async () => {
  appRender(<PublicOrganizationPage organization={null} jobs={[]} />)
  const notFoundMessage = screen.getByRole('heading', {
    name: /not found/i,
  })
  expect(notFoundMessage).toBeInTheDocument()
})
```

we are rendering the PublicOrganizationPage component with an organization value of null, and then verifying that the not found message should be in the document.

#### Public job page

For the public job page, since we are rendering it on the server, we need to fetch the data on the server and display it on the page.

Let’s start by opening the `src/__tests__/public-job-page.test.tsx` file and defining the skeleton for the tests:

```tsx
import PublicJobPage, {
  getServerSideProps,
} from '@/pages/organizations/[organizationId]/jobs/[jobId]'
import { testData } from '@/testing/test-data'
import { appRender, screen } from '@/testing/test-utils'
const job = testData.jobs[0]
const organization = testData.organizations[0]
describe('Public Job Page', () => {
  it('should use getServerSideProps that fetches and returns the proper data', async () => {})
  it('should render the job details', async () => {})
  it('should render the not found message if the data doesnot exist', async () => {})
})
```

Now, we can focus on each test in the test suite.

First, we need to test the `getServerSideProps` function, which will fetch the data and return it via props to the page:

```tsx
it('should use getServerSideProps that fetches and returns the proper data', async () => {
  const { props } = await getServerSideProps({
    params: {
      jobId: job.id,
      organizationId: organization.id,
    },
  } as any)
  expect(props.job).toEqual(job)
  expect(props.organization).toEqual(organization)
})
```

we are calling getServerSideProps and asserting whether the return value matches the expected data.

Now, we can test PublicJobPage, where we want to ensure the provided data is displayed on the page:

```tsx
it('should render the job details', async () => {
  appRender(<PublicJobPage organization={organization} job={job} />)
  const jobPosition = screen.getByRole('heading', {
    name: job.position,
  })
  const info = screen.getByText(job.info)
  expect(jobPosition).toBeInTheDocument()
  expect(info).toBeInTheDocument()
})
```

we are rendering the page component and verifying that the given job’s data is displayed on the page.

Finally, we want to assert the case where the data provided by getServerSideProps does not exist :

```tsx
it('should render the not found message if the data does not exist', async () => {
  const { rerender } = appRender(
    <PublicJobPage organization={null} job={null} />
  )
  const notFoundMessage = screen.getByRole('heading', {
    name: /not found/i,
  })
  expect(notFoundMessage).toBeInTheDocument()
  rerender(<PublicJobPage organization={organization} job={null} />)
  expect(notFoundMessage).toBeInTheDocument()
  rerender(<PublicJobPage organization={null} job={job} />)
  expect(notFoundMessage).toBeInTheDocument()
  rerender(
    <PublicJobPage
      organization={organization}
      job={{ ...job, organizationId: '123' }}
    />
  )
  expect(notFoundMessage).toBeInTheDocument()
})
```

Since there are several cases where the data can be considered invalid, `we are using the rerender function`, which can re-render the component with a different set of props. We assert that if the data is not found, the not found message is displayed on the page.

#### Login page

The login page renders the login form, which, when submitted successfully, navigates the user to the dashboard.

Let’s start by opening the `src/__tests__/login-page.test.tsx` file and adding the following:

```tsx
import LoginPage from '@/pages/auth/login'
import { appRender, screen, userEvent, waitFor } from '@/testing/test-utils'
// 1
const router = {
  replace: jest.fn(),
  query: {},
}
jest.mock('next/router', () => ({
  useRouter: () => router,
}))
describe('Login Page', () => {
  it('should login the user into the dashboard', async () => {
    // 2
    await appRender(<LoginPage />)
    const emailInput = screen.getByRole('textbox', {
      name: /email/i,
    })
    const passwordInput = screen.getByLabelText(/password/i)
    const submitButton = screen.getByRole('button', {
      name: /log in/i,
    })
    const credentials = {
      email: 'user1@test.com',
      password: 'password',
    } // 3
    userEvent.type(emailInput, credentials.email)
    userEvent.type(passwordInput, credentials.password)
    userEvent.click(submitButton) // 4
    await waitFor(() =>
      expect(router.replace).toHaveBeenCalledWith('/dashboard/jobs')
    )
  })
})
```

The test works as follows:

1. We need to mock the useRouter hook because it is being used to navigate the user to the dashboard on successful submission.
2. Next, we render the page.
3. Then, we enter the credentials into the form and submit it.
4. Finally, we expect the replace method on the router to be called with the `/dashboard/jobs`value, which should navigate the user to the dashboard if the login submission succeeds.

To run the integration tests, we can execute the following command:

```sh
npm run test
```

If we want to watch the changes in the test, we can execute the following command:

```sh
npm run test:watch
```

### End-to-end testing

End-to-end testing is a testing method where an application is tested as a complete entity. Usually, these tests consist of running the entire application with the frontend and the backend in an automated way and verifying that the entire system works.

In end-to-end tests, we usually want to test the happy path to confirm that everything works as expected.

To test our application end to end, we will be using Cypress, a very popular testing framework that works by executing the tests in a headless browser. This means that the tests will be running in a real browser environment. In addition to Cypress, since we have become familiar with the React Testing Library, we will use the Testing Library plugin for Cypress to interact with the page.

For our application, we want to test two flows of the application:

- Dashboard flow
- Public flow

#### Dashboard flow

The dashboard flow is the flow for organization admins where we want to test authenticating the user and accessing and interacting with different parts of the dashboard.

Let’s start by opening the `cypress/e2e/dashboard.cy.ts` file and adding the skeleton for our test :

```ts
import { testData } from '../../src/testing/test-data'
const user = testData.users[0]
const job = testData.jobs[0]
describe('dashboard', () => {
  it('should authenticate into the dashboard', () => {})
  it('should navigate to and visit the job details page', () => {})
  it('should create a new job', () => {})
  it('should log out from the dashboard', () => {})
})
```

Now, let’s implement the tests.

First, we want to authenticate into the dashboard:

```ts
it('should authenticate into the dashboard', () => {
  cy.clearCookies()
  cy.clearLocalStorage()
  cy.visit('http://localhost:3000/dashboard/jobs')
  cy.wait(500)
  cy.url().should(
    'equal',
    'http://localhost:3000/auth/login?redirect=/dashboard/jobs'
  )
  cy.findByRole('textbox', {
    name: /email/i,
  }).type(user.email)
  cy.findByLabelText(/password/i).type(user.password.toLowerCase())
  cy.findByRole('button', {
    name: /log in/i,
  }).click()
  cy.findByRole('heading', {
    name: /jobs/i,
  }).should('exist')
})
```

Here,

- we want to clear cookies and localStorage.
- Then, we must attempt to navigate to the dashboard; however, the application will redirect us to the login page.
- We must enter the credentials in the login form and submit it.
- After that, we will be redirected to the dashboard jobs page, where we can see the Jobs title.

Now that we are on the dashboard jobs page, we can proceed further by visiting the job details page:

```ts
it('should navigate to and visit the job details page', () => {
  cy.findByRole('row', {
    name: new RegExp(
      `${job.position} ${job.department} ${job.location}
        View`,
      'i'
    ),
  }).within(() => {
    cy.findByRole('link', {
      name: /view/i,
    }).click()
  })
  cy.findByRole('heading', {
    name: job.position,
  }).should('exist')
  cy.findByText(new RegExp(job.info, 'i')).should('exist')
})
```

Here, we are clicking the View link of one of the jobs and navigating to the job details page, where we verify that the selected job data is being displayed on the page.

Now, let’s test the job creation process:

```ts
it('should create a new job', () => {
  cy.go('back')
  cy.findByRole('link', {
    name: /create job/i,
  }).click()
  const jobData = {
    position: 'Software Engineer',
    location: 'London',
    department: 'Engineering',
    info: 'Lorem Ipsum',
  }
  cy.findByRole('textbox', {
    name: /position/i,
  }).type(jobData.position)
  cy.findByRole('textbox', {
    name: /department/i,
  }).type(jobData.department)
  cy.findByRole('textbox', {
    name: /location/i,
  }).type(jobData.location)
  cy.findByRole('textbox', {
    name: /info/i,
  }).type(jobData.info)
  cy.findByRole('button', {
    name: /create/i,
  }).click()
  cy.findByText(/job created!/i).should('exist')
})
```

Since we are on the job details page, we need to navigate back to the dashboard jobs page, where we can click on the Create Job link. This will take us to the create job page. Here, we fill in the form and submit it. When the submission succeeds, the Job Created notification should appear.

Now that we have tested at all the features of the dashboard, we can log out from the dashboard:

```ts
it('should log out from the dashboard', () => {
  cy.findByRole('button', {
    name: /log out/i,
  }).click()
  cy.wait(500)
  cy.url().should('equal', 'http://localhost:3000/auth/login')
})
```

Clicking the Log Out button logs the user out and redirects them to the login page.

#### Public flow

The public flow of the application is available for everyone who visits it.

Let’s start by opening the`cypress/e2e/public.cy.ts` file and adding the skeleton of the test:

```ts
import { testData } from '../../src/testing/test-data'
const organization = testData.organizations[0]
const job = testData.jobs[0]
describe('public application flow', () => {
  it('should display the organization public page', () => {})
  it('should navigate to and display the public job details page', () => {})
})
```

Now, let’s start implementing the tests.

First, we want to visit the organization page:

```ts
it('should display the organization public page', () => {
  cy.visit(`http://localhost:3000/organizations/${organization.id}`)
  cy.findByRole('heading', {
    name: organization.name,
  }).should('exist')
  cy.findByRole('heading', {
    name: organization.email,
  }).should('exist')
  cy.findByRole('heading', {
    name: organization.phone,
  }).should('exist')
  cy.findByText(new RegExp(organization.info, 'i')).should('exist')
})
```

Here, we are visiting an organization details page and checking whether the data displayed there matches the organization.

Now that we are on the organization details page, we can view a job of the organization:

```ts
it('should navigate to and display the public job details page', () => {
  cy.findByTestId('jobs-list').should('exist')
  cy.findByRole('row', {
    name: new RegExp(
      `${job.position} ${job.department} ${job.location}
        View`,
      'i'
    ),
  }).within(() => {
    cy.findByRole('link', {
      name: /view/i,
    }).click()
  })
  cy.url().should(
    'equal',
    `http://localhost:3000/organizations/$
      {organization.id}/jobs/${job.id}`
  )
  cy.findByRole('heading', {
    name: job.position,
  }).should('exist')
  cy.findByText(new RegExp(job.info, 'i')).should('exist')
})
```

Here, we click the View link of a job, and then we navigate to the job details page, where we are asserting the job data.

To run end-to-end tests, we need to build the application first by running the following command:

```sh
npm run build

# Then, we can start the tests by opening the browser:

npm run e2e

# Alternatively, we can run the tests in headless mode since it is less resource-demanding, which is great for CI:

npm run e2e:headless
```

### Summary

In this chapter, we learned how to test our application, thus making it ready for production.

We started by learning about unit testing by implementing unit tests for our notifications store.

Since integration tests are much more valuable because they give more confidence that something is working properly, we used these tests to test the pages.

Finally, we created end-to-end tests for public and dashboard flows, where we tested the entire functionality of each flow.

In the next chapter, we will learn how to prepare and release our application to production. We will use these tests and integrate them within our CI/CD pipeline, where we will not allow the application to be released to production if any of the tests fail. This will keep our users more satisfied as there is less chance of bugs ending up in production.
