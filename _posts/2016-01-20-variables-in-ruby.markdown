---
layout: post
title:  "Variables in ruby"
date:   2016-01-20 18:30:00
categories: ruby basics
---

Variables in ruby

For a long time this was very confusing to me. Class variable, instance variable,
variable in method, naming with `@` or `@@`. You could get a headache.
Lets lay it down.
In my opinion the best way to learn is try and to try something in ruby the best, fastest
and most basic way is the `irb`.
I encourage everyone to try it and make use of it, heck I have it even on my smartphone.
For purpose of showing how variables work lets write this little snippet:

```
  1 class Top
  2   def howLong?(var)
  3     var.length
  4   end
  5 end
  6
  7 class Next
  8   attr_reader :vari
  9
 10   def initialize
 11     @vari = "name"
 12   end
 13
 14   def symbolize(v)
 15     v.to_sym
 16   end
 17 end
 18
 19 class Example
 20   attr_reader :iv
 21   def initialize
 22     @iv = 123
 23   end
 24 end
 25
 26 class Bottom
 27   @count = 123
 28   class << self
 29     attr_reader :count
 30   end
 31 end
 32
 33 # Bottom.count     # 123
 34
 35 # n = Next.new
 36 # va = n.vari #=> "name"
 37 # n.symbolize(va) #=> :name
```

We are covering here some typical usage of classes methods and variables.
The commented lines are calls and outputs from ruby.

Let's try it out!

Write and save this code to a file, let it be `variables.rb`
As being in the same folder as this file fire up `irb` and type `load "variables.rb"`
This will load our script to irb and let it use our defined classes, methods and variables.

Test it out ! Write your own scripts and test them, have fun and learn

