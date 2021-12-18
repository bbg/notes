Promise:
Imagine that you’re a top singer, and fans ask day and night for your upcoming song.

To get some relief, you promise to send it to them when it’s published. You give your fans a list. They can fill in their email addresses, so that when the song becomes available, all subscribed parties instantly receive it. And even if something goes very wrong, say, a fire in the studio, so that you can’t publish the song, they will still be notified.

This is a real-life analogy for things we often have in programming:

- “producing code” that does something and takes time. For instance, some code that loads the data over a network. That’s a “singer”.
- “consuming code” that wants the result of the “producing code” once it’s ready. Many functions may need that result. These are the “fans”.
- promise is a special JavaScript object that links the “producing code” and the “consuming code” together. In terms of our analogy: this is the “subscription list”. The “producing code” takes whatever time it needs to produce the promised result, and the “promise” makes that result available to all of the subscribed code when it’s ready.

Promises are a new feature of ES6. It’s a method to write asynchronous code. They are easy to manage when dealing with multiple asynchronous operations where callbacks can create callback hell leading to unmanageable code.

How It Works.
There are 3 states of the Promise object:

- Pending: Initial State, before the Promise succeeds or fails
- Resolved: Completed Promise
- Rejected: Failed Promise

Creating and Using A Promise Step by Step

Firstly, we use a constructor to create a Promise object:

```js
const myPromise = new Promise();
```

It takes two parameters, one for success (resolve) and one for fail (reject):

```js
const myPromise = new Promise((resolve, reject) => {  
    // condition
});
```

Finally, there will be a condition. If the condition is met, the Promise will be resolved, otherwise it will be rejected:

```js
const myPromise = new Promise((resolve, reject) => {  
    let condition;  

    if(condition is met) {    
        resolve('Promise is resolved successfully.');  
    } else {    
        reject('Promise is rejected');  
    }
});
```

So we have created our first Promise. Now let's use it.

then( ) for resolved Promises:

```js
myPromise.then();
```

The then( ) method is called after the Promise is resolved. Then we can decide what to do with the resolved Promise.

For example, let’s log the message to the console that we got from the Promise:

```js
myPromise.then((message) => {  
    console.log(message);
});
```

catch( ) for rejected Promises:
However, the then( ) method is only for resolved Promises. What if the Promise fails? Then, we need to use the catch( ) method.

Likewise we attach the then( ) method. We can also directly attach the catch( ) method right after then( ):

example,

```js
myPromise.then((message) => { 
    console.log(message);
}).catch((message) => { 
    console.log(message);
});
```

Async functions - making promises friendly
The async functions and the await keyword, added in ECMAScript 2017.
These features basically act as syntactic sugar on top of promises, making asynchronous code easier to write and to read afterwards.

The Async Keyword
First of all we have the async keyword, which you put in front of a function declaration to turn it into an async function. An async function is a function that knows how to expect the possibility of the await keyword being used to invoke asynchronous code.

They allow you to write promise-based code as if it were synchronous, but without blocking the main thread. They make your asynchronous code less "clever" and more readable.

Async functions work like this:

```js
async function myFirstAsyncFunction() {
  try {
    const fulfilledValue = await promise;
  }
  catch (rejectedValue) {
    // …
  }
}
```

If you use the async keyword before a function definition, you can then use await within the function. When you await a promise, the function is paused in a non-blocking way until the promise settles. If the promise fulfills, you get the value back. If the promise rejects, the rejected value is thrown.

Example: Logging a fetch
Say we wanted to fetch a URL and log the response as text. Here's how it looks using promises:

```js
function logFetch(url) {
  return fetch(url)
    .then(response => response.text())
    .then(text => {
      console.log(text);
    }).catch(err => {
      console.error('fetch failed', err);
```

And here's the same thing using async functions:

```js
async function logFetch(url) {
  try {
    const response = await fetch(url);
    console.log(await response.text());
  }
  catch (err) {
    console.log('fetch failed', err);
  }
}
```

It's the same number of lines, but all the callbacks are gone. This makes it way easier to read, especially for those less familiar with promises.

Adding error handling
And if you want to add error handling, you've got a couple of options.

You can use a synchronous try...catch structure with async/await. This example expands on the first version of the code we showed above:

```js
async function myFetch() {
  try {
    let response = await fetch('coffee.jpg');

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }
    let myBlob = await response.blob();
    let objectURL = URL.createObjectURL(myBlob);
    let image = document.createElement('img');
    image.src = objectURL;
    document.body.appendChild(image);

  } catch(e) {
    console.log(e);
  }
}

myFetch();
```

The catch() {} block is passed an error object, which we've called e; we can now log that to the console, and it will give us a detailed error message showing where in the code the error was thrown.

If you wanted to use the second (refactored) version of the code that we showed above, you would be better off just continuing the hybrid approach and chaining a .catch() block onto the end of the .then() call, like this:

```js
async function myFetch() {
  let response = await fetch('coffee.jpg');
  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }
  return await response.blob();

}

myFetch().then((blob) => {
  let objectURL = URL.createObjectURL(blob);
  let image = document.createElement('img');
  image.src = objectURL;
  document.body.appendChild(image);
})
.catch((e) =>
  console.log(e)
);
```

This is because the .catch() block will catch errors occurring in both the async function call and the promise chain. If you used the try/catch block here, you might still get unhandled errors in the myFetch() function when it's called.

Reference :- developer.mozilla.org
