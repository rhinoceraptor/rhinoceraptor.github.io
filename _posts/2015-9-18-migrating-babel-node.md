---
layout: post
title: Migrating From Babel to Node.js 4.1.0
---

Node.js 4 marks the convergence of io.js with the Node.js project, and with that comes many [ES6 features](https://nodejs.org/en/docs/es6/) into mainline Node.js.

One thing I ran into migrating a small project from [Babel.js](https://babeljs.io/) to Node.js 4 is that Babel will automatically convert function expressions into named function expressions:

```
// my_func.name === ''
var my_func = function() {
  return 'I am a function expression';
}
```

is transformed into

```
// func.name === 'my_func'
var func = function my_func() {
  return 'I am also a function expression, except I have name!';
}
```

One of the nice things about this feature in Babel is it allowed you to use arrow function syntax, but the function would still have a name. With native arrow functions, this is no longer the case.

I found [this post](https://kangax.github.io/nfe/) very helpful for learning about the different ways of creating functions in JavaScript.

