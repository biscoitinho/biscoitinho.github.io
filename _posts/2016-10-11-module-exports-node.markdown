---
layout: post
title: "Node module exports"
date: 2016-10-11 17:00:00
categories: php
---

Node module exports


Today our topic is node module exports.
Could be a little tricky at the beginning but I prepared
a simple example to sort things out a little bit. So without
a further ado let code speak for itself:

> file data.js

```
'use strict';

module.exports = {
  logHello: function() {
    console.log("hello");
  },
  logWorld: function() {
    console.log("world");
  },
  differentThing: function() {
    var thing = "something";
    console.log(thing);
  },
  otherThing: "other",
  ble: "blebleble"
};

```

Nothing to complicated just to show basic concept.
We have wraped in 'module.exports' couple of functions and objects.

Now it is time to display it somehow, for that we will use separate file
called `display.js`

```
'use strict';

var data = require('./data');

data.logHello();
data.logWorld();
data.differentThing();
console.log(data.thing);
console.log(data.otherThing);
console.log(data.ble);
console.log(typeof data.ble);
```

Here we are requiring our data.js file to have access to those exported functions and objects.
And one by one we are calling them and therefore displaying them in the terminal.

Output of `node display.js` is as follows:

```
hello
world
something
undefined
other
blebleble
string
```

Pretty logical, but let's go through one by one:
First two lines are returning the output of functions `logHello` and `logWorld` We are calling them through `data` as it represents
our exported content.
With `data.differentThing` is the same story, but when we call `data.thing` we get `undefined` it's because we want to call a variable which
is defined inside a function. Scope in javascript won't let us do it. When we call `otherThing` we are getting our content, because it is directly accessible
(it is not inside a function). When checking what kind of object `data.ble` is we're geting `string` which is exactly what it is.

At the begining it seems more complicated, then in for example Ruby but after couple of tries everyone should get the grasp of it.
Hope it helped someone.

Cheers
