---
layout: post
title: "Python: Need for Speed"
date: 2022-09-06 18:15:00
categories: python
---

Hi,

Everyone complains that python is slow, well they are actually right.
Of course for basic daily work and even majority of web applications it's totally sufficient,
but there are situations that you actually need some data to be iterated on the fly rather then
to wait in seconds or even minutes.
Most complains are pointed to the for loop in python.
Today we will focus how we can improve this and speed things up.


For a code sample to test different approches I've choose this:

```
result = 0

for i in range(1000000):
        result = result + i

print(result)
```

The most basic thing in existance.
Summing up elements with the help of for loop, but with fairly large number
(to actually have any time to measure).


For the measuring part I've choose `time` linux build in app.
It is better then calculating time inside the script or using dedicated python library.
First of all it measures actual execution time in the system outside the python interpreter.
Plus it got some aditional measurments which comes in handy.


Ok, so let's start, how long would it take basic python for loop listed above to make the calculations?
Below the output from `time`:

```
real 0m0,585s
user 0m0,507s
sys 0m0,079s
```

Well, ok, it did it, but for now we don't have anything to compare yet, let's try now something else:
Basically what we do is a simple sum of the elements within the range, we can use python buildin
function which does exactly that `print(sum(range(1, 1000000)))`
The time of execution of it is:

```
real 0m0,241s
user 0m0,190s
sys 0m0,052s
```

That's better, two times faster then the standard loop.
That proofs that to do the job it is better to use dedicated special tools for it.
We can do many stuff with a hammer, but it's not always the best option.
Let's see what else is there.


Time for the bigger guns now. We can run compiled C code from within python like a standard python
function. Here's how:

First fo all let's write the C code.

```
int sum() {
  long int i;
  long int res = 0;

  for (i = 1; i < 1000000; ++i) {
    res = res + i;
  }
  printf("%ld", res);
  return res;
}
```

Slightly more code than python loop, but nothing too intimidating.
Main difference is static typing of the variables and this example is using a `long int` type.
Rest is a fairly standard for loop.

Now the tricky part. We will compile this code as a `.so` file - a dynamically linked library.
To do so we need to type:

`gcc -fPIC -shared -o name_of_the_file.so name_of_the_file.c`


As a return we will get a compiled `.so` file which we can use in our python script as follows:

```
from ctypes import *

so_file = "/path/to/file.so"
c_sum = CDLL(so_file)

c_sum.sum()
```

Very convinient as we can call the function like an any other function written in python.
Was all of this steps worth anything in terms of speed?

```
real 0m0,187s
user 0m0,110s
sys 0m0,079s
```

Yes! maybe not such a spectacular improvement as before but still faster, plus here we can use
any code we want and we are not bound only to buildin functions.


Ok, so we improved a lot when comparing to the basic for loop in terms of speed, so how do we look
now comparing it to the competition (other languages)
I've made a identical for loops for 3 other languages let's see:

`Ruby 3.1.2`

```
real 0m0,394s
user0m0,355s
sys 0m0,037s
```

`Javascript with node 14.18.1` (Yeah, yeah I know ancient version)

```
real 0m0,143s
user 0m0,125s
sys 0m0,020s
```

To pause here for a moment when comparing to ruby we are now way ahead with our optimizations,
but to my surprise JS is smoking fast and double the props doing it on a very old node version.
For a cherry on top I saved the GOAT for last - C (as a standalone program):

```
real 0m0,004s
user 0m0,004s
sys 0m0,000s
```

Well, yeah, I don't think there is a comment needed...


To summarize we can drastically increse the speed of a python code first of all using proper tools
to do it - if speed is the priority we should move away from the for loop as far as possible and
use a dedicated tools if applicable (buildins) Secondly we can write and compile the most vital code
in terms of speed in C and then run it as a function from python. Third option, flip the table and
write everything in C.

### Additional notes

I've also checked cython and the setup is more complicated (more steps nothing to wild) and in terms of
speed it's somwhere in ruby territory so I don't think it's worth it in terms of plain speed, but a
very interesting approch nonetheless.


As for C - we can also run standard compiled C code in python like so:

```
import os

os.system("/path/to/compiled_c_file")

```

To me less elegant and readable and the speed differences is neclectable.
Why this approch is slower then raw C ? - Raw C executes itself and this two methods first use the python
interpreter plus must handle file I/O which will always be slower than bare code.


Cheers
