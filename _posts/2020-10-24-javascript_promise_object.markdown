---
layout: post
title:      "Javascript Promise Object"
date:       2020-10-24 22:32:07 +0000
permalink:  javascript_promise_object
---


Javascript is considered a Single Threaded langauge, in that it executes code line by line.  When asynchronous (events happening at different intervals), this can cause a bit of an issue as you can receive errors with undefined variables and functions.  Asychronous Javascript is a constant evolving paradigm on how to effectly handle these events.  The world of Asyncronous Javascript is a little overwhelming and consists of general concepts including blocking, passing in call back functions, and now the Promise object.

The promise object is a representation of the status of an another operation that is happening asychrnously.  It acts as a stand-in value for the eventual computated value (if the operation is sucessful) that can be returned after the Promise is fulfilled.  

The Promise object has an internal state that represents it's current status 
* *Pending*: When a Promise is intialized it is in this state awaiting confirmation
* *Fulfilled*: Operation has settled, and is sucessful;  Promise value is ready to be retrieved
* *Rejected*: Operation has settled, and is not sucessful; Promise is not able to be fulfilled

In our earlier example our Promise object can handle these asynchronous operations that will allow your app to expect future resolution, or a 'promise' of resolution once everything has been settled.  A unique feature of being able to use the Promise object is the `promise.then()`, `promise.catch()`, and `promise.finally()` methods that allow us to create a newly generated Promise object that we can then chain to additional operations.

For example in a sample project:

```
fetchMeals = () => 
    fetch(this.root+'/meals')
    .then(res => res.json())
    .then(this.renderMeals)
```

Our fetch() function returns us our initial Promise object, that we can then chain (thus generating a new promise) to perform additional operations.  In this case, we render our fetch results into JSON, and add an additional .then() to call our rendering function on the newly JSONified data.  

The .catch() method returns a Promise object that handles rejected cases only.  In our above example we can console out any errors.

```
fetchMeals = () => 
    fetch(this.root+'/meals')
    .then(res => res.json())
    .then(this.renderMeals)
		.catch(error => { 
		console.error ('Your Message Here', error)
		})
```

The .finally() method can be used to pass in a callback function after the promise is settled regardless of whether the promise was fulfilled or rejected - this can provide a way to process settled promises regardless of sucess/failure.












