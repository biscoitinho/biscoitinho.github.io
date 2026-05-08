---
layout: post
title: "Spinel Ruby Compiler"
date: 2026-05-07 09:00:00
categories: ruby
---

Hi,

There is quite a buzz in Ruby community lately due to recent work of the man himself - Matz.
Matz created [Spinel](https://github.com/matz/spinel) which is a Ruby compiler.


Yes, now we can actually compile plain ruby into standalone native executables.
It generates a opimized C code that can be run like a standard compiled C program.
I decided I need to give it a try!


Bare in mind that it's still on an early stage of develoment and has still a lot of restrictions.
For instance gems don't work with it as of now and only basic core ruby funcionalities can be used.
(please see repository README for further details)

Lets bring in the simple example code I've used with speed testing python
[here](https://biscoitinho.github.io/python/2023/01/21/Need-for-Speed-pt2.html)

```
result = 0
1_000_000_000.times { |i| result += i }
puts result
```

Simple stuff, but also quite time consuming
When using `time ruby.speedtest.rb` on my machine (which isn't a monster in any way) took around 50 secs.
quite a while for a 3 lines of code ;)


Now let's see what we can do with it with spindel:
Same code but now first of we will compile it with
`./spindel speedtest.rb -o speedtest` and run it with `./speedtest`
or with in our case `time ./speedtest` let's compare the outcomes:


```
➜  time ruby speedtest.rb
ruby speedtest.rb  55,86s user 0,69s system 98% cpu 57,477 total
➜  time ./speedtest
./speedtest  0,00s user 0,00s system 48% cpu 0,008 total
```

It was THAT fast really...

Conclusion: Huge speed improvment, but lots of limitations, at least a this moment in time.
What I can see is that we can incorporate this as we would with C snippetes used for heavy computation in ruby before,
but this itime we can use plain ruby insted of C, we only need to compile this heavy lifting portion and that's it,
still 100% ruby code, still a happy developer and huge bump up in performance.
Of course still not production ready, but I'm very interested about further developments of this project.
As always Matz is bringing great stuff to the community.

Cheers
