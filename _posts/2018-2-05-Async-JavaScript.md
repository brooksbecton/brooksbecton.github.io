---
layout: post
title: Async Javascript - Return of the Callbacks!
---

JavaScript has had a few iterations of ways to handle asynchronous code. It can be a little funky to understand what is going on if you are new to JavaScript. In this example, we will make a request to the [SWAPI](https://swapi.co/) to get Luke Skywalker's information and use callbacks to handle async. The code should say that is going to print a name, and then print the name.

Here is a naive approach

```js
/**
 * Makes an AJAX request to the SWAPI and
 * returns Luke Skywalker's info
 * @returns {Object} luke - Various Info about Luke Skywalker
 */
function getLuke() {
  fetch("https://swapi.co/api/people/1/")
    .then(response => response.text())
    .then(function(luke) {
      return JSON.parse(luke);
    });
}

/**
 * Prints the name of a Star Wars Character
 * @param {Object} person - A Star Wars character
 * @param {string} person.name - Name of the star wars character
 */
function printName(person) {
  if (person !== undefined) {
    console.log(person.name);
  } else {
    console.log("No Person Supplied");
  }
}

console.log("Printing Name"); // Printing Name
const luke = getLuke(); // undefined
printName(luke); // No Person Supplied
```

This may look like it will work, but remember that JavaScript is always synchronous. This is easy for me to remember if I think about JS always going "straight down." It say's its about to print the name, calls the function, _doesn't wait for the request to finish_, tries to prints Luke's name. After the print the api returns the data, but it is already too late.~

## Callbacks

Callbacks are a way to pass a function into another function as a parameter and then running that "callback" after then async code has finished. With this example, we pass the print function in as a callback, so that when the data is back from the API, the data will get passed into the print function.

```js
/**
 * Makes an AJAX request to the SWAPI and
 * returns Luke Skywalkers info
 * @param {Function} cb - The function to be called
 *                        after the return of the jedi
 */
function getLuke(cb) {
  fetch("https://swapi.co/api/people/1/")
    .then(response => response.text())
    .then(function(data) {
      return cb(JSON.parse(data));
    });
}

// ...

console.log("Printing Name"); // Printing Name
getLuke(printName); // Luke Skywalker
```

We can pass functions as parameters in JS because in functions are [First Class Citizens](https://en.wikipedia.org/wiki/First-class_citizen). Notice that we run `getLuke(printName)` which passes the function in without running it, instead of `getLuke(printName())` which runs printName then hands printName's return value, _undefined_, to getLuke as a parameter

<img src="https://media.giphy.com/media/9K2nFglCAQClO/giphy.gif"  style="display:block;margin: auto; width:75%">

This works for this lunchbox example, but in reality this can get out of hand quickly if you have to make a few sequential requests which leads to [Callback Hell](http://callbackhell.com/). It is much better to use Promises which we will get to next.
