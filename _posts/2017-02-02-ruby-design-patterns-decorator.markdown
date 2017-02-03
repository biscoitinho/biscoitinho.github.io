---
layout: post
title: "Ruby design patterns: Decorator"
date: 2017-02-02 17:00:00
categories: ruby
---

Ruby design patterns: Decorator

> In object-oriented programming, the decorator pattern is a design
> pattern that allows behavior to be added to an individual object,
> either statically or dynamically, without affecting the behavior
> of other objects from the same class.*

*- after wikipedia

It is very useful when we want our code to be as much DRY as possible.
We can obtain this kind of behaviour in couple of ways, I will show
two of them.

```
class Car
  def initialize(maker:, model:, year:, horsepower:)
    @maker = maker
    @model = model
    @year = year
    @horsepower = horsepower
  end

  attr_accessor :maker, :model, :year, :horsepower
end

class CarDecorator < SimpleDelegator
  def kilowatts
    (horsepower / 1.35).round
  end
end

car = Car.new(
  maker: "honda",
  model: "prelude",
  year: 1995,
  horsepower: 185
)

car_decorator = CarDecorator.new(car)

p car_decorator.class # => UserDecorator
p car_decorator.maker # => "honda"
p car_decorator.model # => "prelude"
p car_decorator.horsepower # => "185"
p car_decorator.kilowatts # => "137"
p car_decorator.year # => "1995"
p car #=> #<Car:0x000000022ae818 @maker="honda", @model="prelude", @year=1995, @horsepower=185>
```

Ok, so first we have pretty basic and standard initialize in class `Car`.
Nothing fancy, moving on.
Next class is where the money is. We are using `SimpleDelegator`, it's ruby
build-in delegator implementation. Here we are creating a separate class method
for a `Car` class. Which takes `horsepower` from `Car` class and converts it to kW.
Next step is to make some `car` object, and call a `car_decorator` decorator on a `car`
object.
And now `car_decorator` got all of the data from `car` object and also his unique
method for kilowatts.


As for the second implementation here it goes:

```
class Money
  def total_amount
    20
  end
end

module Rent
  def total_amount
    super - 7
  end
end

module Bills
  def total_amount
    super - 3
  end
end

money = Money.new
money.extend(Rent)
money.extend(Bills)
p money.total_amount   # => 10
```

In this approach we are also making a basic class of `Money` but instead of
`SimpleDecorator` we are using modules and a `super` keyword which calls
a parent method of the same name, with the same arguments. Then for our
`money` object we use `extend` method to extend it by our  modules `Rent`
and `Bills`.

Very useful stuff. Using this approach we won't build gigantic classes with
hundreds of methods but instead a small useful chunks of modules and methods
which can extend funcionality of our basic smaller and nicer objects.

Cheers
