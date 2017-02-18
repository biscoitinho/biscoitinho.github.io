---
layout: post
title: "Is it JS or is it a fantasy?"
date: 2017-02-18 17:00:00
categories: js
---

Javasctipt in recent time went with a big metamorphosis, and I mean really big.
Now it's even more like ruby on the visual aspect of writing code
(and for me that is a great thing!). Let's see on our own how valid JS code with
the blessing of ES2015 and later could look like comparing it with Ruby.

Let's start with simple ruby code:

```
class Car
  def initialize(brand, model, color)
    @brand = brand
    @model = model
    @color = color
  end

  def engineOn
   puts @brand + " " + @model + " started engine"
  end
end

class Trim < Car
  def abs
    puts @brand + " " + @model + " has ABS system"
  end
end

myCar = Car.new("honda", "prelude", "silver")
myCar.engineOn

myCarTrim = Trim.new("honda", "prelude", "silver")
myCarTrim.abs
```

Pretty basic stuff, Two classes with one inheriting from the other
and two simple methods- classical stuff.
Now let's see how this code can look like in new fashion js:

```
class Car {
  constructor(brand, model, color) {
    this.brand = brand;
    this.model = model;
    this.color = color;
  }

  engineOn() {
    console.log(this.brand + " " + this.model + " started engine");
  }
}

class Trim extends Car {
  abs() {
    console.log(this.brand + " " + this.model + " has ABS system");
  }
}

let myCar = new Car("honda", "prelude", "silver");
myCar.engineOn();

let myCarTrim = new Trim("honda", "prelude", "silver");
myCarTrim.abs();
```

Wow! for me it's like a dream come true. Code is pretty much identical except
couple of parenthisis and `@` signs here and there.
Now I like js a little bit more, again ;)


Cheers
