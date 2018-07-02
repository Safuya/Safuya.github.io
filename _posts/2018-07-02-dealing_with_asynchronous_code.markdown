---
layout: post
title:      "Dealing with Asynchronous Code"
date:       2018-07-02 20:14:27 +0000
permalink:  dealing_with_asynchronous_code
---


Synchronous code is straightforward for me to understand. When you write everything, you write it in order. I know that execution always happens in the same order. When it comes to Javascript, however, code starts becoming asynchronous, meaning that it will not usually run in the same order.

Requests to an API can be put into the background while waiting for a response and be dealt with later. If you put something in the oven to cook, you don't usually stand in front of it waiting for whatever your food to be ready. You will go and carry out some other tasks and come back once it is ready. Javascript makes calls asynchronous by default for a lot of different actions. If you're writing to a file or making a call to a website, you will come across this.

The async and await functions make it easier to create asynchronous code that wouldn't usually be asynchronous, while also having functions run in a set order if they need to be. Take the below code as an example:

```
function resolveAfter2Seconds() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('You have arrived.');
    }, 2000);
  });
}

async function asyncCall() {
  console.log('calling');
  const result = await resolveAfter2Seconds();
  console.log(result);
  console.log('Welcome to your destination');
}

asyncCall();
console.log('Are we there yet?');
```

The output of this if run becomes:
> "calling"
> "Are we there yet?"
> "You have arrived."
> "Welcome to your destination"

So everything within the async function will run in order, but everything outside of the async call will run as soon as there's available time on the stack to execute.

jQuery allows a similar syntax for its get function, providing method chaining for success, failure and a default that always runs.

```
const jqxhr = $.get( "example.php", function() {
  alert( "success" );
})
  .done(function() {
    alert( "second success" );
  })
  .fail(function() {
    alert( "error" );
  })
  .always(function() {
    alert( "finished" );
  });
```
    
For more extended and more nested results, I think this provides a much cleaner syntax than doing everything in the initial callback.
