I have been developing web apps over a decade and have witnessed the rise and fall of many JavaScript libraries and frameworks. Change is inevitable on the web.

> Today’s most famous successor bridges the gap generated by its ancestor.

Haha.. jQuery (and associated libraries) was the de facto way to go about web development however, it has its limitations like plugins and its interferences. Then the rise in module driven development, etc. Then we saw the rise of web component specification that enabled capabilities to extend existing HTML elements. This inspired the era of component-based JS frameworks, including AngularJS and React with the lifecycle management model, etc. and it was the battle between the watchers and the Virtual-DOM. Angular2.x soon emerged trying to fix the mess in the first version. VueJS came in the picture in 2014 and it was better in a few aspects where Angular or React lacks.

> Developers lookout for newer options when there is an alternative option that solves the actual problem.

as 2020, Svelte / Stencil / Angular elements / Polymers / Web components are examples of this emerging trend.

## What is the difference between Arrow and Regular Functions?

- **This** value (context): in a regular function, the value of *this* has nothing to do with the class on which it was defined; instead, it depends on the object that it was ***called upon\***; inside an arrow function *this* value always equals the *this* value from the outer function
- **Constructors**: regular functions can easily construct objects, while an arrow function cannot be used as a constructor
- **Arguments** object: is a special array-like object containing the list of arguments with which the function has been invoked, inside an arrow function the *arguments* object is resolved lexically: it accesses arguments from the outer function
- **Implicit return**: regular functions use the return expression statement — otherwise it will just return undefined, while with arrow functions, if they contain one expression and the function’s curly braces are missing, then the expression is implicitly returned
- [Read more](https://dmitripavlutin.com/differences-between-arrow-and-regular-functions/)

## How does This work?

- ***this\*** keyword refers to an object, that object which is executing the current bit of JavaScript code
- Every JavaScript function while executing has a reference to its current execution context, called ***this\*** — execution context means here is how the function is called
- To understand ***this\*** keyword, only we need to know how, when and from where the function is called, does not matter how and where function is declared or defined
- [Read more & see examples](https://codeburst.io/all-about-this-and-new-keywords-in-javascript-38039f71780c)

## What are callbacks and closures?

- **Callback**: a function which is accessible by another function and invoked after the first function — if that first function completed
- [Read more](https://www.freecodecamp.org/news/javascript-callback-functions-what-are-callbacks-in-js-and-how-to-use-them/) about callbacks
- **Closure**: are created whenever a variable that is defined outside the current scope is accessed from within some inner scope — it gives you access to an outer function’s scope from an inner function
- To use a closure, simply define a function inside another function and expose it
- [Read more](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures#:~:text=A closure is the combination,scope from an inner function.) about closures

## What is an Anonymous Function?

- A function that was declared without any function named identifier to refer to it — it is usually not accessible after its initial creation. These functions are created at run time
- [Read more](https://www.javascripttutorial.net/javascript-anonymous-functions/)

## What are Higher-Order Functions?

- Functions that r**eceive a function as an argument** or **return a function as output**
- For example, Array.prototype.map, Array.prototype.filter and Array.prototype.reduce are some of the Higher-Order functions built into the language.
- [More here](https://blog.bitsrc.io/understanding-higher-order-functions-in-javascript-75461803bad)

## What is “strict” mode in JS?

- Strict mode enables more rigorous error-checking in your code and makes debugging easier
- You enable strict mode by adding “use strict”; at the beginning of the file
- [Read more](https://www.educative.io/edpresso/what-is-use-strict-in-javascript?utm_source=Google AdWords&aid=5082902844932096&utm_medium=cpc&utm_campaign=kb-dynamic-edpresso&gclid=CjwKCAjwoc_8BRAcEiwAzJevtZyt8ueI8zRKbsnF5_b0OYQXvsk4kA5GMACgxLhBfJQH-XNfPt_WmRoCC0wQAvD_BwE)

## Promises vs Async/Await?

- **Scope**: in a promise, only the promise chain is asynchronous — doesn’t block the execution; with async/await the entire wrapper function is asynchronous
- **Logic**: in a promise, synchronous work can be handled in the same callback, and multiple promises can be handled using Promise.all; with async/await the synchronous work needs to be placed outside the callback, and multiple promises can be handled with more simple variables
- **Error handling**: Promises: *then, catch, finally*; Async/await: *try, catch, finally*
- [See examples here](https://levelup.gitconnected.com/async-await-vs-promises-4fe98d11038f)

## Mutable vs Immutable in JS

- The difference between immutable and mutable: if an item is mutable, when changing the value of the reference variable it will also affect the value of the original referenced variable
- **Primitive data types** such as numbers, strings, and Booleans are **immutable** — it is impossible to change values of those types by changing the reference — you can combine them and derive new values from them, but when you assign a specific value, that value will always remain the same; you can make a variable name point to a new value, but the previous value is still held in memory
- **Mutable** is a type of variable that can be changed by its reference — in JS only **objects and arrays** are mutable: you can change their properties; for example: causing a single object value to have different content at different times
- [Read more and see examples](https://gomakethings.com/mutable-vs.-immutable-in-javascript/)

## What is a typed language?

- The values are associated with values and not with variables — it can be *static or dynamic*
- **Static**: the variable can hold only one type, like in Java: a variable declared of string can take only a set of characters and nothing else
- **Dynamic**: the variable can hold multiple types — like in JS: a variable can take number, chars, etc.

## What is hoisting?

- JavaScript’s default behavior of moving all declarations to the top of the current scope (to the top of the current script or the current function)
- JavaScript only hoists *declarations*, not *initializations*

## What is event delegation in JS?

- A simple technique by which you add a single event handler to a parent element in order to avoid having to add event handlers to multiple child elements
- Using event delegation it’s possible to add an event handler to an element, wait for an event to bubble up from a child element and easily determine from which element the event originated
- [Examples](https://www.sitepoint.com/javascript-event-delegation-is-easier-than-you-think/#:~:text=JavaScript event delegation is a,handlers to multiple child elements.)

## What is the difference between call, apply & bind?

- **bind** can be particularly helpful when you want to use events to access properties of one class within another class
- **call** and **apply** are very similar — they invoke a function with a specified *this* context, and optional arguments
- The only difference between call and apply is that **call** requires the arguments to be passed in one-by-one, and **apply** takes the arguments as an array
- [Read more and see examples](https://www.taniarascia.com/this-bind-call-apply-javascript/)

## What is the difference between host and native objects?

- Objects can be divided into these two main categories, depending on the environment and language
- Host objects: environment specific — ex. Browser supplies certain objects such as window, node supplies NodeList, etc.
- Native / Built-in objects: standard objects provided by JS — sometimes referred to as global objects; JS is mainly constructed by these categorized native objects (String, Number, etc)

## What is a curry function?

- Currying is a process in functional programming in which we can transform a function with multiple arguments into a sequence of nesting functions — it returns a new function that expects the next argument inline
- Transformation of functions that translates a function from callable as f(a, b, c) into callable as f(a)(b)(c )
- It allows us to easily get partials, avoid passing the same variable multiple times
- It creates nesting functions according to the number of the arguments of the function so each function receives an argument: if there is no argument, there is no currying.
- [Read more](https://javascript.info/currying-partials)
- [Read more 2](https://dev.to/suprabhasupi/currying-in-javascript-1k3l)

## What is the Event loop?

- In most browsers there is **an event loop for every browser tab**, to make every process isolated and avoid a web page with infinite loops or heavy processing to block your entire browser
- The environment manages multiple concurrent event loops, to handle API calls
- [Web Workers](https://flaviocopes.com/web-workers/) run in their own event loop as well
- You mainly need to be concerned that your code will run on a **single event loop**, and write code with this thing in mind to avoid blocking it
- Any JavaScript code that takes too long to return back control to the event loop will **block the execution** of any JavaScript code in the page, even block the UI thread, and the user cannot click around, scroll the page, and so on
- [Read more](https://flaviocopes.com/javascript-event-loop/#:~:text=The event loop continuously checks,executes each one in order.)

## What is Event propagation?

- Event propagation is a mechanism that defines how events propagate or travel through the DOM tree to arrive at its target and what happens to it afterward
- In modern browser event propagation proceeds in two phases: **capturing**, and **bubbling** phase
- ***Capturing\***: events propagate from the window down through the DOM tree to the target node — only works with event handlers registered with the **addEventListener()** method when the third argument is set to **true**
- ***Bubbling\***: In this phase event propagates or bubbles back up the DOM tree, from the target element up to the window, visiting all of the ancestors of the target element one by one — supported in all browsers, and it works for all handlers, regardless of how they are registered e.g. using **onclick** or **addEventListener()**
- [Read more](https://www.tutorialrepublic.com/javascript-tutorial/javascript-event-propagation.php#:~:text=Event propagation is a mechanism,what happens to it afterward.)

## What is the difference between Cookies, Local Storage and Session Storage?

- Browser **storage**: data is never transferred to the server & can only be read on client-side; storage limit is about 5MB
- **Session vs local storage**: session is only temporary — data is stored until tab/browser is closed; local doesn’t have an expiration date
- Cons for storage: not secure, limited to strings, danger for XSS
- **Cookie**: stores data with expiration date, storage limit about 4KB, is sent to the server for each request, read/write on both client & server side (if HttpOnly then it is inaccessible to client-side scripts)

- https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)