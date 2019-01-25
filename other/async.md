# Learning Goals?
- What is the difference between asynchronous and synchronous code?
- Is JS synchronous or asynchronous?
- Is Node synchronous or asynchronous?
- Wat are ways to deal with asynchronous code?

# Difference asynchronous and synchronous
Synchronous = one at a time.
Asynchronous = multiple at once

The difference between synchronous code and asynchronous code is that synchronous code executes from the top of a code block to the bottom in the order it was written.

Asynchronous — moves with the inertia of the first throw by the quarterback. The code is processed, but along the way you may want to cause it to wait or check for the completion of a task. Such as:
- First connecting to a DB
- Writing to the DB

Two main advantages of asyncronous code is that you are less dependent on other functions and that the execution is usually faster.

# Is JS synchronous or asynchronous?
JavaScript is single threaded, that means only one statement is executed at a time. Say we have 2 lines of codes Line-1 followed by Line-2. Synchronous means Line-2 can not start running until the Line-1 has finished executing.

When you hear folks say that **JavaScript is an asynchronous language, what they mean is that you can manipulate JavaScript to behave in an asynchronous way**; through callbacks, promises and async/sync functionality.

#  Is JS synchronous or asynchronous?
Source: https://codeburst.io/how-node-js-single-thread-mechanism-work-understanding-event-loop-in-nodejs-230f7440b0ea

# How to handle asynschronous code?
## Callbacks
Asynchronous callbacks allow you to invoke a callback function which sends a database request (and any other nested callbacks) off to your app, where it waits for the response from the database, freeing up the rest of your code to continue running.

## Promises
In the case of asynchronous actions you could, instead of arranging for a function to be called at some point in the future, return an object that represents this future event.This is what the standard class Promise is for. A promise is an asynchronous action that may complete at some point and produce a value. It is able to notify anyone who is interested when its value is available.

#### Sometimes there is a ready-made promise
You can create a promise by calling the ES6 Promise constructor function with new (see Listing 1 below), then call resolve() when results are ready or reject() on detecting an error. Sometimes you can get a ready-made promise by calling an appropriate API or library function, like the fetch() Web API function in Listing 1

#### Receive a 'promised' value with .then
You can receive the 'promised' value by calling the .then() method of the promise, passing it a function that will receive that value as its argument as soon as it is available.

#### Three states
Internally, a promise can be in one of three states:
- pending: the asynchronous result is still awaiting delivery
- fulfilled: the asynchronous result has been delivered and is available (resolve() was called)
- rejected: an error was encountered: the promise could not be fulfilled (reject() was called)

#### Chaining
Because .then() always returns a new promise, it’s possible to chain promises with precise control over how and where errors are handled. Promises allow you to mimic normal synchronous code’s try/catch behavior.

To chain promises, you specifically have to state return. If you make use of the arrow syntax shorthand, specifying return is not required.

```javascript
let wordnikAPI = 'link'
giphyApi = 'link'

function setup () {
  fetch(wordnikAPI)
    .then(response => response.json()) // arrows syntax shorthand, return is not required
    .then(json => {
      createP(json.word);
      return fetch(giphyAPI + json.word); //including return is required
    })
    .then(response => {
      return response.json();
    })
    .then(json => {
      createImg(json.data.data[0].images['fixed_height_small'].url)
    })
    .catch(err => console.log(err));
}
```

## Async Await

