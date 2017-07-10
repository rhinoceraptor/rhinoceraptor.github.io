---
layout: post
title: Using Tail Call Optimization in Node.js
---

Everyone who has used recursion has probably seen something like the following:

```
RangeError: Maximum call stack size exceeded
    at recursive (/Users/jack/recursive.js:1:1)
    at recursive (/Users/jack/recursive.js:1:1)
    at recursive (/Users/jack/recursive.js:1:1)
    ...
```

There is a language feature that can prevent this, tail call optimization. The compiler can transform self-recursive functions into loops so that only a single stack frame is needed, regardless of the number of times a function calls itself.

Node.js (as of July 2017) offers a work-in-progress implementation of tail call optimization behind a feature flag, ```--harmony_tailcalls```.

Running the following program without ```--harmony_tailcalls```, we can see each recursive call will slowly fill the function call stack:

```javascript
'use strict'; // --harmony_tailcalls will not work without strict mode

function countdown (x) {
  console.trace(x); // Print a stack trace for each countdown() call
  return x !== 0 ? countdown(x - 1) : null;
}

countdown(10);
```

```
$ node countdown.js
Trace: 10
    at countdown (/Users/jack/countdown.js:4:11)
    at Object.<anonymous> (/Users/jack/countdown.js:8:1)
    at Module._compile (module.js:571:32)
    at Object.Module._extensions..js (module.js:580:10)
    at Module.load (module.js:488:32)
    at tryModuleLoad (module.js:447:12)
    at Function.Module._load (module.js:439:3)
    at Module.runMain (module.js:605:10)
    at run (bootstrap_node.js:425:7)
    at startup (bootstrap_node.js:146:9)
Trace: 9
    at countdown (/Users/jack/countdown.js:4:11)
    at countdown (/Users/jack/countdown.js:5:20)
    at Object.<anonymous> (/Users/jack/countdown.js:8:1)
    at Module._compile (module.js:571:32)
    at Object.Module._extensions..js (module.js:580:10)
    at Module.load (module.js:488:32)
    at tryModuleLoad (module.js:447:12)
    at Function.Module._load (module.js:439:3)
    at Module.runMain (module.js:605:10)
    at run (bootstrap_node.js:425:7)

...

Trace: 0
    at countdown (/Users/jack/countdown.js:4:11)
    at countdown (/Users/jack/countdown.js:5:20)
    at countdown (/Users/jack/countdown.js:5:20)
    at countdown (/Users/jack/countdown.js:5:20)
    at countdown (/Users/jack/countdown.js:5:20)
    at countdown (/Users/jack/countdown.js:5:20)
    at countdown (/Users/jack/countdown.js:5:20)
    at countdown (/Users/jack/countdown.js:5:20)
    at countdown (/Users/jack/countdown.js:5:20)
    at countdown (/Users/jack/countdown.js:5:20)
```


When running with the tail call feature flag, we see that only a single stack frame is generated:

```
$ node --harmony_tailcalls countdown.js
Trace: 10
    at countdown (/Users/jack/countdown.js:4:11)
    at Object.<anonymous> (/Users/jack/countdown.js:8:1)
    at Module._compile (module.js:571:32)
    at Object.Module._extensions..js (module.js:580:10)
    at Module.load (module.js:488:32)
    at tryModuleLoad (module.js:447:12)
    at Function.Module._load (module.js:439:3)
    at Module.runMain (module.js:605:10)
    at run (bootstrap_node.js:425:7)
    at startup (bootstrap_node.js:146:9)
Trace: 9
    at countdown (/Users/jack/countdown.js:4:11)
    at Object.<anonymous> (/Users/jack/countdown.js:8:1)
    at Module._compile (module.js:571:32)
    at Object.Module._extensions..js (module.js:580:10)
    at Module.load (module.js:488:32)
    at tryModuleLoad (module.js:447:12)
    at Function.Module._load (module.js:439:3)
    at Module.runMain (module.js:605:10)
    at run (bootstrap_node.js:425:7)
    at startup (bootstrap_node.js:146:9)

...

Trace: 0
    at countdown (/Users/jack/countdown.js:4:11)
    at Object.<anonymous> (/Users/jack/countdown.js:8:1)
    at Module._compile (module.js:571:32)
    at Object.Module._extensions..js (module.js:580:10)
    at Module.load (module.js:488:32)
    at tryModuleLoad (module.js:447:12)
    at Function.Module._load (module.js:439:3)
    at Module.runMain (module.js:605:10)
    at run (bootstrap_node.js:425:7)
    at startup (bootstrap_node.js:146:9)
```
