---
layout: post
title: "Defeating nightmares with JS"
date: 2016-02-17 18:00:00
categories: js basics
---

Today my collegue at work told me:

> Yesterday I had a dream that I was back in school standing next to the
> blackboard and I had to write in front of whole class a clock in javascript
> I didn't know what to do, it was a nightmare !
(true story)(Cheers Magda)

So it is time to defeat nightmares with JS!

Code below is more like a proof-of-concept then actual script,
it is very simple and barebone (for now!) we will gradually extend it.
Our nightmeres will go away, at least for a moment ;)

Let's create a file called alarmclock.js
(I assume we already have nodeJS installed)

and in that file:

```
"use strict"

function alarmClock() {
  var time = new Date();
  var hour = time.getHours();
  var minute = time.getMinutes();
  var second = time.getSeconds();
  var t = setTimeout(alarmClock, 1000);

  console.log(hour + ":" + minute + ":" + second);
  if (hour === 21 && minute === 45) {
    console.log("time to get up!");
  }
}

alarmClock();
```

let's save it and fire it up with `node alarmclock.js`

And we have an alarm clock set at 21:45... well sort of ;)
Code is extremelly simple but for the sake of it let's
break it down line by line:

`use strict` best way to explain it is to read
[this](http://www.atatus.com/blog/solve-javascript-error-before-it-happens-strict-mode/)
 link.

In next line we are creating a function that is our clock.
The lines with `var` keyword are for declaring variables which
we need to display time with (in real time).
The `time` variable creates a new `Date()` object, and from that
object we obtain hours, minutes and seconds.
The `t` variable is a little bit tricky it calls our function
by a specified amount of time - in our case 1000 miliseconds.
`console.log` logs our variables to the console.
The next lines are where we set our 'alarm'.
If it is 21:45 log to the console that it is time to get up.

On last line we call(invoke) our function.

Ok, it is working, but lets face it its far from perfect.
It constantly repeats in terminal and we have to use `Ctrl+c`
to quit it. It is not very convinient.

We can make our clock to display in web page without this annoying
console logs. Let's make another file in the same directory as
our script. Let's call it `clock.html`

```
<html>
<head>
<script src="alarmclock.js"></script>
</head>
<body>
  <p id="clock">Clock</p>
</body>
</html>
```

It is simplefied version of html file which is using javascript script
named alarmclock.js. Within body of this page we have paragraph with
id "clock" and word "Clock" in it.
So let's try it out by opening this file in out browser.
When the page loads we see a word Clock displayed.
But it is not all into it !
Time to hit right mouse button anywhere on the page and click on
`inspect`. We will have a developers console/tools. Lets navigate to
`console` and `Logs` tab.
Our script is running in the background like it was in the terminal.
Time to tweak it a little more.

```
"use strict"

function alarmClock() {
  var time = new Date();
  var hour = time.getHours();
  var minute = time.getMinutes();
  var second = time.getSeconds();
  var t = setTimeout(alarmClock, 1000);

  document.getElementById("clock").innerHTML =
  hour + ":" + minute + ":" + second;

  if (hour === 21 && minute === 45) {
    alert("time to get up!");
  }
}
window.onload = alarmClock;
```

Ok, so what happened here ?
`document.getElementById(clock")` finds our paragraph in `clock.html`
with id of `clock`.
`innerHTML` property changes paragraph to a value after `=` sign and
that is in our case our old `console.log`.
Next changed thing is in if statement, we changed `console.log` to
`alert` - Now we are using browser and what `alert` does it tells browser
to render popup window, it is a more verbose sign to wake up then modest
`console.log` hidden in dev console.
And finally last line now we want to load and execute our function immediately
after page is loaded.
This is it our simple basic clock with basic alarm funcionality.

In upcoming posts I will try to tweak it to be more useful and nicer to
the eyes!

stay tuned.
