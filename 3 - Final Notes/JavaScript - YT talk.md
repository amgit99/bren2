<span style="color:#e1db3d">What are you JS ?</span> : I am a single threaded, non-blocking, asynchronous, concurrent language. I have call stack, event loop, callback queue and some other APIs. Its also an interpreted language, if that was not clear already. The chrome browser provides this V8 runtime, but just by itself it does not have the tools to deal with the DOM, or HttpRequests etc. Those are provides by separate APIs. 
#### The call stack :
One stack == one thread == one line of execution. 
The functions go onto the stack as stack frames just like C. The stack trace can give us the functions that led up to the point that caused the error. 
Long operations <span style="color:#e1db3d">block the stack</span>, rendering everything unresponsive, since there is just one thread, the instructions that are executing belong to the long task and not to the parts that respond to the user, this is bad for websites which is the most important application of JS. So these long tasks like fetch requests etc should be done asynchronously.
So the browser is not single threaded, ![[Pasted image 20240212121321.png]] The WebAPIs run on a different thread, and we can make calls to that. Now let us see how things are made Async. ![[Pasted image 20240212121727.png]] Functions like `setTimeout()` are part of the webAPIs that run on separate threads, so when one of such functions is called, it along with whatever callback function that is provided leaves the stack immediately and runs on the dedicated threads waiting for the time consuming event(timeout, fetch request) to finish, once that happens the callback is put on the <span style="color:#e1db3d">task queue</span>. The <span style="color:#e1db3d">event loop</span> has the job to check on the task queue, every time the call stack is empty. If it is empty, it places the first thing in front of the task queue and lets it execute.
So a `setTimeout(0)` would just put the callback instantly on the task queue.

### Asynchronous JS :
really like the                                             
Callbacks is a term for the practice where functions accept other functions as arguments and they are called at the end of the first function.
```js
function doTask(successCallback, failureCallback) { 
//function takes in two functions, to call in both cases of success and failure
	// doing task
	bool taskResult;
	if(taskResult == 1)
		successCallback();
	else
		failureCallback();
}

doTask(() => { // as we call doTask we are defining the functions on the fly
	console.log('success !!');
}, () => {
	console.log('failure !!');
})
```
Instead we can use Promises :
```js
function doTaskWithPromises(){
	return new Promise((resolve, reject) => {
		// doing task
		bool taskResult;
		if(taskResult == 1)
			resolve();
		else
			reject();
	})
}

// this returns a promise so we use the then, catch.
doTaskWithPromises().then(() => {
	console.log('success !!');
}).catch(() => {
	console.log('failure !!');
});
```
The best one so far is `async` `await` :
```js
function task1(){
	return new Promise((resolve, reject) => {
		// doing task
		bool taskResult;
		if(taskResult == 1) resolve();
		else reject();
	})
}

function task2(){
	return new Promise((resolve, reject) => {
		// doing task
		bool taskResult;
		if(taskResult == 1) resolve();
		else reject();
	})
}

task1().then((successString) => {
    console.log(successString);
}).catch((failString) => {
	console.log(failString);
});
```
## DOM : 
<span style="color:#e1db3d">Event Listeners</span>: Events are actions that one can perform while interacting with the user interface, there are many like click, hover, mouse button down etc. Event listeners are pieces of code that capture events and allow the user to act on them. 
The DOM has a nested structure, one that can be modeled by a tree. So when an event is triggered on a child, it is triggered for its parent as well, <span style="color:#e1db3d">since the parent encapsulates the child</span>, this happens all the way up to the root, this is <span style="color:#e1db3d">the bubbling phase</span>. But this pass from child to root happens after the first pass that goes from the root to the child. So if we add the `capture` parameter in the event listener <span style="color:#e1db3d">we can capture the event on its first pass from the root to the child</span>, instead of capturing it on the default second pass that bubbles from child to root.

