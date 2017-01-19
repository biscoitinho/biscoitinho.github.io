---
layout: post
title: "Ruby design patterns: Observer"
date: 2017-01-20 17:00:00
categories: ruby
---

Ruby design patterns: Observer

> The observer pattern is a software design pattern in which an object,
> called the subject, maintains a list of its dependents, called observers,
> and notifies them automatically of any state changes,
> usually by calling one of their methods.*

*- after wikipedia

Not as controversial as his fellow singelton, also observer pattern
is a key part in MVC architecture.

Let's get started:

```
require 'observer'

class Bodybuilder
  include Observable

  attr_reader :protein, :muscle

  def initialize(protein, muscle)
    @protein = protein
    @muscle = muscle
    add_observer(Log.new)
  end

  def power(exercise)
    @muscle = exercise + protein
    changed
    notify_observers(self, muscle)
  end
end

class Log
  def update(bodybuilder, muscle)
    open('./log.txt', 'a') do |f|
      f << Time.now.strftime("%d/%m/%Y %H:%M") + " " +
      "Bodybuilder with #{bodybuilder.protein} grams of protein, grows pure #{muscle} grams of muscle!. \n"
    end
  end
end

bodybuilder = Bodybuilder.new(100, 50)
bodybuilder.power(30)
bodybuilder.power(400)
```

Ok, so what does our bodybuilder do?
We have two separate classes `Bodybuilder` and `Log`.
Bodybuilder class is the observable one the subject, and Log class is an observer.


In Bodybuilder class we `include` the `Observable` to mark this class as the observable one.
In the initialize method we only add, well `add_observer` - pretty logical.
The `power` method is also very simple there we are only letting know the observer that they
must observe it with `notify_observers`.
Class Log is a basic logger it writes to file `log.txt` date and time of app execution and
additional text, but only when something in `Bodybuilder` is changed.
Let's give it a go and fire it up in terminal. Next thing we need to do is to `cat log.txt`
file and see what is inside:

```
#sample output

20/01/2017 18:26 Bodybuilder with 100 grams of protein, grows pure 130 grams of muscle!.
20/01/2017 18:26 Bodybuilder with 100 grams of protein, grows pure 500 grams of muscle!.
```

Good, it worked. Things changed in our `bodybuilder` and `Log` class responded to them by
logging the data to separate file, that is `log.txt`. Pretty handy stuff!

Cheers
