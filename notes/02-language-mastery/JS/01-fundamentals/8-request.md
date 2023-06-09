# ASYNC PROGRAMMING

## Table of contents

- [📖 Resources](#resources)
- [📚 Other Useful resources](#other-useful-resources)
- [🎯 Learning Objectives](#learning-objectives)
- [📝 Notes](#notes)

  - [HTTP Request](#http-requests)
  - [Request with fetch API](#request-with-fetch-api)
  - [Project](#film-finder)

## Resources

> ☞ TODO: Add resources links

## Other Useful resources

> ☞ TODO: Add resources links

## Learning Objectives

- Understand how web browsers communicate via HTTP.
- Create calls to various APIs using requests.
- Make asynchronous requests using async/await syntax.

## Notes

### HTTP Requests

- Understand how web browsers communicate via HTTP.

#### Background

#### `What is HTTP?`

HTTP stands for `Hypertext Transfer Protocol` and is used to structure requests and responses over the internet.
The transfer of resources happens using `TCP` (Transmission Control Protocol).

#### HTTP & TCP: How it Works

- When you type an address such as `www.codecademy.com` into your browser, you are commanding your browser to open a `TCP channel` to the server that responds to that URL

- A URL is like your home address or phone number because it describes how to reach you.

- In this situation, your computer, which is making the request, is called the client. The URL you are requesting is the address that belongs to the server.

- Once the `TCP connection` is established, `the client` sends a `HTTP GET request` to the server to retrieve the webpage it should display.

- After the server has sent the response, it closes the TCP connection.

- GET requests are one kind of `HTTP method` a client can call. there are other common ones (POST, PUT and DELETE)

- If the server is able to locate the path requested, the server might respond with the header:

```md
HTTP/1.1 200 OK
Content-Type: text/html
```

The first line of the header, `HTTP/1.1 200 OK`, is confirmation that the server understands the protocol that the client wants to communicate with `(HTTP/1.1)`, and an HTTP status code signifying that the resource was found on the server. The second line, `Content-Type: text/html`, shows the type of content that it will be sending to the client.

If the server is not able to locate the path requested by the client, it will respond with the header:

```md
HTTP/1.1 404 NOT FOUND
```

In this case, the server identifies that it understands the `HTTP protocol`, but the `404 NOT FOUND` status code signifies that the specific piece of content requested was not found. This might happen if the content was moved or if you typed in the URL path incorrectly or if the page was removed.

#### An Analogy

Imagine the internet is a town. You are a client and your address determines where you can be reached. Businesses in town, such as Codecademy.com, serve requests that are sent to them. The other houses are filled with other clients like you that are making requests and expecting responses from these businesses in town. This town also has a crazy fast mail service, an army of mail delivery staff that can travel on trains that move at the speed of light.

Suppose you want to read the morning newspaper. In order to retrieve it, you write down what you need in a language called HTTP and ask your local mail delivery staff agent to retrieve it from a specific business. The mail delivery person agrees and builds a railroad track (connection) between your house and the business nearly instantly, and rides the train car labeled “TCP” to the address of the business you provided.

Upon arriving at the business, she asks the first of several free employees ready to fulfill the request. The employee searches for the page of the newspaper that you requested but cannot find it and communicates that back to the mail delivery person.

The mail delivery person returns on the light speed train, ripping up the tracks on the way back, and tells you that there was a problem “404 Not Found.” After you check the spelling of what you had written, you realize that you misspelled the newspaper title. You correct it and provide the corrected title to the mail delivery person.

This time the mail delivery person is able to retrieve it from the business. You can now read your newspaper in peace until you decide you want to read the next page, at which point, you would make another request and give it to the mail delivery person.

#### What is HTTPS?

Since your HTTP request can be read by anyone at certain network junctures, it might not be a good idea to deliver information such as your credit card or password using this protocol. Fortunately, many servers support HTTPS, short for HTTP Secure, which allows you to encrypt data that you send and receive.

HTTPS is important to use when passing sensitive or personal information to and from websites. However, it is up to the businesses maintaining the servers to set it up. In order to support HTTPS, the business must apply for a certificate from a Certificate Authority.

### Request with fetch API

#### Introduction to Requests with ES6

The four most commonly used types of HTTP requests are `GET`, `POST`, `PUT`, and `DELETE`. In this lesson, we’ll cover `GET` and `POST` requests.

With a GET request, we’re retrieving, or getting, information from some source (usually a website). For a POST request, we’re posting information to a source that will process the information and send it back.

#### Introduction to GET request with ES6

The first type of requests we’re going to tackle is `GET` requests using `fetch()`.

The `fetch()` function:

- is an alternative to the XMLHttpRequest.
- Creates a request object that contains relevant information that an API needs.
- Sends that request object to the API endpoint provided.
- Returns a promise that ultimately resolves to a response object, which contains the status of the promise with information the API sent back.

![fetch](/notes/02-language-mastery/assets/fetch-GET.svg)

_Technical communication_:

- First, call the fetch() function and pass it a URL as a string for the first argument, determining the endpoint of the request

```js
fetch('https://api-to-call.com/endpoint')
```

- The `.then()` method is chained at the end of the `fetch()`. The `.then()` method will fire only after the promise status of fetch() has been resolved.

- Inside the callback function, the `ok` property of the response object returns a Boolean value. If there are no errors, `response.ok` will be true and the code will return `response.json()`

- If response.ok is a falsy value, our code will throw an error

```js
throw new Error('Request failed!')
```

- A second argument passed to .then() will be another arrow function that will be triggered when the promise is rejected. It takes a single parameter, networkError. This object logs the networkError if we could not reach the endpoint at all (e.g., the server is down).

- A second .then() method will run after the previous .then() method has finished running without error. It takes jsonResponse, which contains the returned response.json() object from the previous .then() method, as its parameter and can now be handled, however we may choose.

#### Introduction to GET request using fetch

#### Making a GET request

```js
const url = 'https://api.datamuse.com/words?sl='

// Selects page elements
const inputField = document.querySelector('#input')
const submit = document.querySelector('#submit')
const responseField = document.querySelector('#responseField')

// Asynchronous function
const getSuggestions = () => {
  const wordQuery = inputField.value
  const endpoint = url + wordQuery
  fetch(endpoint, { cache: 'no-cache' })
    .then(
      (response) => {
        if (response.ok) {
          return response.json()
        }
        throw new Error('Request failed!')
      },
      (networkError) => {
        console.log(`${networkError.message}`)
      }
    )
    .then((jsonResponse) => {
      // renderRawResponse(jsonResponse)
      renderResponse(jsonResponse)
    })
}
```

we make a GET request to the Datamuse API endpoint. Then, we chained a `.then()` method and passed two callback functions as arguments — one to handle the promise if it resolves, and one to handle network errors if the promise is rejected. we will chain another .then() method, which will allow us to take the information that was returned with the promise and manipulate the webpage! Note that if there is an error returned in the first .then() method, the second .then() method will not execute.

#### Handling a GET request

#### Introduction to POST request using fetch

#### Making a POST request

![post](/notes/02-language-mastery/assets/fetch-POST.svg)

the `fetch()` call takes two arguments: an endpoint and an object that contains information needed for the `POST` request.

The object passed to the fetch() function as its second argument contains two properties: method, with a value of 'POST', and body, with a value of JSON.stringify({id: '200'});. This second argument determines that this request is a POST request and what information will be sent to the API.

A successful POST request will return a response body, which will vary depending on how the API is set up.

The rest of the request is identical to the GET request.

```js
const shortenUrl = () => {
  const urlToShorten = inputField.value
  const data = JSON.stringify({ destination: urlToShorten })

  fetch(url, {
    method: 'POST',
    headers: {
      'Content-type': 'application/json',
      apikey: apiKey,
    },
    body: data,
  })
    .then(
      (response) => {
        if (response.ok) {
          return response.json()
        }
        throw new Error('Request failed!')
      },
      (networkError) => console.log(`${networkError.message}`)
    )
    .then((jsonResponse) => {
      renderResponse(jsonResponse)
    })
}
```

#### Introduction to async GET request

![get with async](/notes/02-language-mastery/assets/async-get-request.svg)

The structure for this request will be slightly different. We will use the new keywords `async` and `await`, as well as the `try and catch statements`.

- The async keyword is used to declare an async function that returns a promise.

- The await keyword can only be used within an async function. await suspends the program while waiting for a promise to resolve.

- In a try...catch statement, code in the try block will be run and in the event of an exception, the code in the catch statement will run.

#### Making an async GET request

```js
// Selecting page elements
const inputField = document.querySelector('#input')
const submit = document.querySelector('#submit')
const responseField = document.querySelector('#responseField')

// Asynchronous function
// Code goes here

const getSuggestions = async () => {
  const wordQuery = inputField.value
  const endpoint = url + queryParams + wordQuery
  try {
    const response = await fetch(endpoint, { cache: 'no-cache' })
    if (response.ok) {
      const jsonResponse = await response.json()
      renderResponse(jsonResponse)
    }
  } catch (error) {
    console.log(error)
  }
}
```

#### Introduction to async POST request

We still have the same structure of using try and catch as the async GET request we just learned about. But, in the fetch() call, we now have to include an additional argument that contains more information like method and body.

The method property value is set to 'POST' to specify the type of request we are making. Then we have to include a body property with the value of JSON.stringify({id: 200}).

```js
const shortenUrl = async () => {
  const urlToShorten = inputField.value
  const data = JSON.stringify({ destination: urlToShorten })
  try {
    const response = await fetch(url, {
      method: 'POST',
      body: data,
      headers: {
        'Content-type': 'application/json',
        apikey: apiKey,
      },
    })
    if (response.ok) {
      const jsonResponse = await response.json()
      renderResponse(jsonResponse)
    }
  } catch (error) {
    console.log(error)
  }
}
```

#### Recap

- GET and POST requests can be created in a variety of ways.
- We can use fetch() and async/await to asynchronous request data from APIs.
- Promises are a type of JavaScript object that represents data that will eventually be returned from a request.
- The fetch() function can be used to create requests and will return promises.
- We can chain .then() methods to handle promises returned by the fetch() function.
- The async keyword is used to create asynchronous functions that will return promises.
- The await keyword can only be used with functions declared with the async keyword.
- The await keyword suspends the program while waiting for a promise to resolve.

### Film Finder
