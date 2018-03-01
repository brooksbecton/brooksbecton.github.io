---
title: SWAPI Graph
date: 2018-03-01 19:16:00 Z
---

# SWAPI Graph

[https://brooksbecton.github.io/SWAPI-Graph/](https://brooksbecton.github.io/SWAPI-Graph/)

I had started making a few posts about how asynchronous code works in JavaScript. At the the end of these I wanted to make a challenge so that someone could get some practice with the new ideas they had just learned about. Also, I would use the [SWAPI](https://swapi.co/) as a data source.

Why SWAPI?

1. It's completely open, no keys required

2. It's Star Wars.

## Origin

The ending challenge for one was to follow a chain of resources from the SWAPI to help users see what callback hell can look/feel like. Then I thought about how I could maybe show these relationships from the SWAPI. I immediately thought of a graph with nodes and edges. I could make the graph just for the information I asked them to give, but I wanted it to be more dynamic.

## Goals

My goals were to make a small intuitive app with a short life span. I stuck with React for this one and looked at a couple of different graphing libraries in JS. Most were fairly straight forward, but [vis](http://visjs.org/) was the easiest to get up and running with. I also wanted to UI to look slick. I have used [Material UI](http://www.material-ui.com/) and [React Bootstrap](https://react-bootstrap.github.io/) before so I wanted to try something new. After a little bit of searching, I came across [Ant Design](https://ant.design/) and it seemed pretty popular (20k\+ stars at the time of writing).\
\
I had a good time making this little app. I used the [create-react-app](https://github.com/facebook/create-react-app)  cli for the boiler and I didn't know that it registered a [service worker](https://developers.google.com/web/fundamentals/primers/service-workers/) in production. Which helps out a lot. I cache the response from the SWAPI in the app to avoid a lot of back and forth and with the service worker registered that even further decreases the amount of information going over the wire. Also, almost anytime I can get a service worker easily integrated I like to try to max out the [Lighthouse](https://developers.google.com/web/tools/lighthouse/) scores in Chrome.

Here is what I ended up with at the time of writing.

* **PWA** - 100

* **Performance** - 76

* **Accessibility** - 94

* **Best Practices** - 94