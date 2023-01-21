---
layout: post
title: "Python: Need for Speed part 2"
date: 2023-01-21 09:00:00
categories: python
---

Hi,

When I did this mini research last time two more pretendents has gotten into the stage.
Python version 3.11 which is alegedly faster than older versions and something I haven't used before
[Pypy](http://www.pypy.org) Pypy is a implementation of a compiled python written in RPython (restricted python).
Interesting, let's give it both a go.


For a code sample everything is almost identical, I've added couple more zereos to spice things up (nine in total to be exact).

```
result = 0

for i in range(1000000000):
        result = result + i

print(result)
```

Actually there is now quite of numbers to cover and it should take substatntial amount of time.
For good measure to start slowly I've started with version 3.9 of mainstream Python to have a good base.
The results are:

```
real 6m49,172s
user 6m48,179s
sys 0m0,157s
```

Yes, MINUTES...


With that in place let's check now the latest (at the moment of writing this) version 3.11.1.
The results:

```
real 6m54,571s
user 6m53,166s
sys 0m0,308s
```

Well, wouldn't call it an improvment... but I'm not saying the newest version haven't been optimised and
some implementations are indeed faster, it's not just my test case...


Now let's try this Pypy thing (version 3.9, also the latest as of writing this).
Everything stayed the same, just switched the python version in pyenv.
The results:

```
real 0m6,330s
user 0m6,176s
sys 0m0,093s
```

WOW, that is my comment, It went from 6 minutes to 6 SECONDS.
It's still python. Same code.


For the sake of science let's compare it to our champion - the mighty C:
The results:

```
real 0m6,820s
user 0m6,812s
sys 0m0,004s
```

The C is... slower...
seeing the speed of pypy I thought it would be a close call, but didn't expect this.


SUM UP:

Well I DID NOT expect this kind of results, mighty impressed, althrough pypy also has it limitations and drawbacks
(please see the official documentation)
It's 100% usable and LIGHTNING fast. Of course I'm also aware that my example is extremally simple
and defenetly more complex code could open or close new possibilites.
At the end there is only one thing to ask now: What would python haters do now?


(Still for OS development or crutial speed application usage I wouldn't use python...yet)


Cheers
