---
layout: post
title: "AlarmClock part 2"
date: 2016-02-21 21:00:00
categories: js basics
---

Welcome in part 2 of building Alarm Clock in JS, which will make our nightmares
go away.
We made very simple alarm clock and to be honest no the most useful one.
Today we will change that!
We need to make setting our alarm more user friendly and alarm itself
to trigger some...alarm.
Let's go to our `clock.html' file and modify it:


```
<html>
<head>
<script src="alarmclock.js"></script>
</head>
<body>
  <p id="clock">Clock</p>
  <input name="hour" id="hours" maxlength="2"/>
  <input name="minute" id="minutes" maxlength="2"/>
  <p id="alarm"></p>
</body>
</html>
```

We added two input fields for gathering data typed by the user of our
alarm clock. One for the hours of the alarm and second for minutes.
Additional we added max length of the input to two signs.
Last paragraph is for a text based alarm signal.
Now when we have html all set let's go to our script file
`alarmClock.js` where all the fun is

```
"use strict"

function alarmClock() {
  var time = new Date();
  var hour = time.getHours();
  var minute = time.getMinutes();
  var second = time.getSeconds();
  var t = setTimeout(alarmClock, 1000);
  var hourValue = document.getElementById('hours').value;
  var minuteValue = document.getElementById('minutes').value;
  var h = parseInt(hourValue);
  var m = parseInt(minuteValue);

  document.getElementById("clock").innerHTML =
  hour + ":" + minute + ":" + second;

  if (hour === h && minute === m) {
    document.getElementById("alarm").innerHTML = "Time to get up!";
    clearTimeout(t);
    var sound = new Audio("filename.mp3");
    sound.play();
  }
}
window.onload = alarmClock;
```

Ok, so what is going on here:
Variables `hourValue` and `minuteValue` are retriving values from inputs.
Those values are strings not numbers so we have to change them to such, these job
is taking care of by `h` and `m` variables.
In if statement we are adding to paragraph with id of `alarm` text.
Next part is little tricky because we are disabling timeouts.
`sound` variable is an audio object with (mp3 file of your choosing, have to be
in the same folder as rest of the files) and with sound we got `play()` for playing it.
If we hadn't cleared timeouts sound whould start every 1000 milisecond (we didn't
want that ;))

Now our clock is much more useful we can easily set alarm and when time comes alarm
triggers alarm sound. Clock is still kind of ugly, we will take care of this in
next post.

