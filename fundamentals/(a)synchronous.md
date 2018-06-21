### Difference asynchronous and synchronous 
Synchronous = one at a time.
Asynchronous = multiple at once

The difference between synchronous code and asynchronous code is that synchronous code executes from the top of a code block to the bottom in the order it was written. 

Asynchronous — moves with the inertia of the first throw by the quarterback. The code is processed, but along the way you may want to cause it to wait or check for the completion of a task. Such as:
- First connecting to a DB
- Writing to the DB
- Reading from the DB
- Returning the result back to your program

Two main advantages of asyncronous code is that you are less depedent on other functions and that the execution is usually faster.

### Javascript is synchronous that can act in an asynchronous way
JavaScript is single threaded, that means only one statement is executed at a time. Say we have 2 lines of codes Line-1 followed by Line-2. Synchronous means Line-2 can not start running until the Line-1 has finished executing.
 
When you hear folks say that JavaScript is an asynchronous language, what they mean is that you can manipulate JavaScript to behave in an asynchronous way; through callbacks, promises and async/sync functionality. 

### Node.js is asynschronous
Source: https://codeburst.io/how-node-js-single-thread-mechanism-work-understanding-event-loop-in-nodejs-230f7440b0ea

### Callbacks 
Asynchronous callbacks allow you to invoke a callback function which sends a database request (and any other nested callbacks) off to your app, where it waits for the response from the database, freeing up the rest of your code to continue running.

### Promises
In the case of asynchronous actions you could, instead of arranging for a function to be called at some point in the future, return an object that represents this future event.

This is what the standard class Promise is for. A promise is an asynchronous action that may complete at some point and produce a value. It is able to notify anyone who is interested when its value is available.

A promise is an object that may produce a single value some time in the future: either a resolved value, or a reason that it’s not resolved (e.g., a network error occurred). A promise may be in one of 3 possible states: fulfilled, rejected, or pending. Promise users can attach callbacks to handle the fulfilled value or the reason for rejection.

### When to use callbacks and when to use promises
Regular JavaScript computations can fail by throwing an exception. Asynchronous computations often need something like that. A network request may fail, or some code that is as part of the asynchronous computation may throw an exception.

One of the most pressing problems with the callback style of asynchronous programming is that it makes it extremely difficult to make sure failures are properly reported to the callbacks.

A widely used convention is that the first argument to the callback is used to indicate that the action failed, and the second contains the value produced by the action when it was successful. Such callback functions must always check whether they received an exception, and make sure that any problems they cause, including exceptions thrown by functions they call, are caught and given to the right function.

Promises make this easier. They can be either resolved (the action finished successfully) or rejected (it failed). Resolve handlers (as registered with then) are called only when the action is successful, and rejections are automatically propagated to the new promise that is returned by then. And when a handler throws an exception, this automatically causes the promise produced by its then call to be rejected. So if any element in a chain of asynchronous actions fails, the outcome of the whole chain is marked as rejected, and no success handlers are called beyond the point where it failed.

Sources:
https://medium.com/@kvosswinkel/is-javascript-synchronous-or-asynchronous-what-the-hell-is-a-promise-7aa9dd8f3bfb
https://eloquentjavascript.net/11_async.html
https://medium.com/@siddharthac6/javascript-execution-of-synchronous-and-asynchronous-codes-40f3a199e687
