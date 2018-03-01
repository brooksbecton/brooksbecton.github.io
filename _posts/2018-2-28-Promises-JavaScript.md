---
layout: post
title: Promises in JavaScript - .then strikes back!
---

In a [previous post]({{ site.baseurl }}{% post_url 2018-2-05-Async-JavaScript %})
I had talked about what callbacks, but now we are going to talk about how to use promises.

A promises in JavaScript is a constructor that takes in a function. The function can take in two functions as parameters. The first being a resolve function. The resolver is what you want to run when every thing went well. The second parameter is the function you want to run when things don't go right.

Lets say we want to make a fetch request to our favorite Star Wars API to get a character by their id, if everything goes well we will resolve with the data, otherwise we'll reject with an error.

```js
function fetchSwapiPerson(personId) {
  return Promise(function(resolve, reject) {
    fetch("https://swapi.co/api/people/" + personId + "/")
      .then(resp => resp.json())
      .then(swapiData => {
        if (swapiData.name) {
          resolve(swapiData);
        } else {
          reject(new Error("Person Not Found"));
        }
      });
  });
}
```

To use this function would look something like

```js
fetchSwapiPerson(1)
.then(person => {
    console.log(person.name)
})
.catch(error => /*handle error*/);
```

Whatever data is sent as a parameter in the resolve function, will come out as a parameter in the function passed to the `.then`. So in our case, the variable `swapiData`, right after the `if(swapiData.name)` is the same thing as the `person` in the function passed to the `.then`

The same holds to for the `reject` function and the `.catch`. If the person we get back doesn't have a name, then we send back an error. So the `new Error("Person Not Found")` is what is coming out at `.catch(error)`

You will mostly see promises used for HTTP requests or when writing/reading a file. If you see a `.then` then you know that the function it is attached to is returning a promise.

So what does that say about `fetch` ? It has a `.then`.

![Luke Skywalker saying no](https://media.giphy.com/media/EsfkYoDAqa2mk/giphy.gif)

You've already been using promises and you may not have even known it. So that's it go ahead and stop reading. You're an expert in Promises.

.

..

...

....

.....

......

Oh. Still here?

Alright, lets see what the example from the callbacks post would look like with promises

```js
/**
 * Makes an AJAX request to the SWAPI and
 * returns Luke Skywalker's info
 * @returns {Promise}
 */
function getLuke() {
  return new Promise((resolve, reject) => {
    fetch("https://swapi.co/api/people/1/")
      .then(response => response.text())
      .then(data => {
        resolve(JSON.parse(data));
      });
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

const luke = getLuke().then(luke => {
  printName(luke);
});
```

Boy that's dandy, but aren't you just going to end up with a lot of `.then` and fall into the darkest pit of JavaScript _look out there are a couple_ of Promise Hell?

Well yeah.
