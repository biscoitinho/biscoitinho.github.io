---
layout: post
title: "Ruby design patterns: Singleton"
date: 2016-12-07 17:00:00
categories: ruby
---

Ruby design patterns: Singleton

> In software engineering, the singleton pattern is a software design pattern
> that restricts the instantiation of a class to one object.
> This is useful when exactly one object is needed to coordinate actions
> across the system. The concept is sometimes generalized to systems that operate
> more efficiently when only one object exists, or that restrict the instantiation
> to a certain number of objects. The term comes from the mathematical concept
> of a singleton.*

NOTE:
There are some who are critical of the singleton pattern and consider it
 to be an anti-pattern in that it is frequently used in scenarios where
 it is not beneficial, introduces unnecessary restrictions in situations
 where a sole instance of a class is not actually required,
 and introduces global state into an application.*

*- after wikipedia

So, implementation of singleton pattern is controversial, but imho as they
say: It is good to know your enemy ;)

Let's get started:

```
require 'singleton'

class Creature
  include Singleton
  attr_accessor :type

  def animal
    'snake'
  end
end

p first = Creature.instance
p second =  Creature.instance

p first.type = {dangerous: true}
p second.type
p Creature.instance.animal
p second.animal
```

First of all we need to require singleton lib (ruby build-in) and include it
in our class in which we want to implement singleton pattern.
`attr_accessor` or this simple method is rather self explanatory, so let's
move further. We instantiate two variables with `Creature.instance`
For the variable `first` we declare type, but in case of `second` we aren't
declaring any type instead we only call it.
In the next line We call a method `animal` from `Creature.instance` and on
the last line we call a method `animal` on a `second` variable.

This are the outputs:

```
#=> <Creature:0x000000010ca700>
#=> <Creature:0x000000010ca700>
#{:dangerous=>true}
#{:dangerous=>true}
#"snake"
#"snake"
```

Variables and instances are identical.
As mentioned above the instantiation of a class is restricted
to one object.

Cheers
