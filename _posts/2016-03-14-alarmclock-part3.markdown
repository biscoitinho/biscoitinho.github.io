---
layout: post
title:  "AlarmClock part 3"
date:   2016-03-14 21:00:00
categories: js basics
---

Welcome in part 3 of building Alarm Clock in JS, which will make our nightmares
go away, and this time for sure !
Thanks to my friend Magda today with her help we will be adding some style and
slick looks to our little app.
Lets begin by adding a new directory in our main folder go ahead and
`mkdir fonts` in this catalogue we will store font which looks just like a display
of digital clock. We are using [this](http://www.dafont.com/digital-7.font) font
but you can use anyone you like. Just copy these font files to `/fonts` directory.
Now let's make some changes in our html file:

```
<head>
<script src="alarmclock.js"></script>
<link rel="stylesheet" type="text/css" href="style.css">
</head>
<body>
<div class="wrapper">
  <div class="clock-number">
     <p id="clock">Clock</p>
  </div>
<div class="instruction">
  <p>Set your alarm here:</p>
</div>
  <div class="clock-inputs">
    <label for="hours">Hour</label>
      <input placeholder="00" type="number" name="hour" id="hours" maxlength="2"/>
      <label for="minutes">Minutes</label>
    <input placeholder="00" type="number" name="minute" id="minutes" maxlength="2"/>
  </div>
  <div class="clock-alarm">
      <p id="alarm"></p>
    </div>
</div>
</body>
</html>
```
We added path to CSS file where our styles will be doing their magic, and also
 structured html code little bit. Nothing to fancy ;)
Time for style!:


```
@font-face {
  font-family: 'digital-7';
  src: url('fonts/digital-7.ttf')  format('truetype')
}
body {
  padding: 0;
  margin: 0;
  width: 100%;
  background-color: #000;
}
p {
  margin: 0;
  padding: 0;
  padding-top: 15px;
}
.wrapper {
  display: block;
  margin: 0 auto;
  width: 280px;
  padding-top: 200px;
  text-align: center;
}
.clock-number {
  font-family: "digital-7";
  font-size: 100px;
  color: #2CF9FD;
  text-shadow: 1px 1px 7px #2CF9FD;
}
.instruction {
  font-family: "Tahoma", Arial, sans-serif;
  color: #ccc;
  margin-bottom: 15px;
  margin-top: 5px;
  text-align: center;
}
.clock-inputs input{
  width: 65px;
  font-family: "Tahoma", Arial, sans-serif;
  padding: 4px 10px;
  border-radius: 3px;
  border: none;
  background-color: #555;
}
.clock-inputs input:hover{
  background-color: #666;
}
.clock-inputs {
  font-family: "Tahoma", Arial, sans-serif;
}
.clock-inputs label {
  color: #ccc;
}
.clock-alarm {
font-family: "Tahoma", Arial, sans-serif;
color: #2CF9FD;
font-weight: bold;
margin: 20px;
}
```

First we are adding font path and font family, rest are css styles used in certain
html tags. I won't be going 1 by 1 css selector and property. Instead look at
[this](https://developer.mozilla.org/en-US/docs/Web/CSS/Reference) link for more info.

Lastly let's make some final touches to our js code:

```
"use strict"

function alarmClock() {
  var time = new Date();
  var hour = time.getHours();
  var minute = time.getMinutes();
  var second = time.getSeconds();
  var t = setTimeout(alarmClock, 1000);
  var hourValue = +document.getElementById('hours').value;
  var minuteValue = +document.getElementById('minutes').value;

  document.getElementById("clock").innerHTML =
  hour + ":" + minute + ":" + second;

  if (hour === hourValue && minute === minuteValue) {
    document.getElementById("alarm").innerHTML = "Time to get up!";
    clearTimeout(t);
    var sound = new Audio("filename.mp3");
    sound.play();
  }
}
window.onload = alarmClock;
```

We got rid of variables `m` and `h` and now using sorter js syntax.
The `+` sign is making the same job as `parseInt` method. With the help of
this little guy our code is clearer and shorter - classical win-win ;)

That wraps up AlarmClock series special thanks to Magda for awesome job with
css styles - Thanks Magda!

Next time we will dig in to some basics of rails magic.
