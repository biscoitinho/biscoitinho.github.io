---
layout: post
title: "Introduction to Ruby custom errors"
date: 2016-10-28 17:00:00
categories: ruby
---

Introduction to Ruby custom Errors

Generic Ruby errors could be helpful and informative but why not make them
even more informative and helpful by tweaking them a little bit.
Ever wanted to make your own custom Ruby error ? Now you got a chance.
We would need two files:

`app.rb` and `errors.rb` for simplicity let's store them in one folder.
Let's start with `errors.rb` file:

```
module Exceptions
  class FooError < ScriptError
  end
end

class BarError < NameError
end
```

What is going here ?
We made a class which inherits from bulid in Ruby class `ScriptError`
and we wrap it in module `Exceptions`. Second one simillar but without
module around it.

Now it's time for `app.rb`

```
require_relative 'errors.rb'

class App
  def self.raiser(input)
    if input == "foo"
      raise Exceptions::FooError
    elsif input == "bar"
      raise BarError, "This is bar!"
    else
      p "Nothing to raise"
    end
  end
end
```

In line one we are requiring `errors.rb` file to have access to its classes.
Next we define a simple class `App` with simple if statement which
output is based on provided later on input data.
Thing is what is happening when certain inputs are provided.
If `"foo"` then we raise an error called `FooError` which is inside
`Exceptions` module.
When it is `"bar"` then we raise, a `BarError` with additional message
`"This is bar!"` to give us even more info and maybe hint how to deal
with this kind of exception later on.
Now how to make it work?
At the bottom of `app.rb` file let's call an `App` class with `raiser` method.
For example like this:

`call = App.raiser("foo")`

After saving our work and runing our program with `ruby app.rb` we should get:


> app.rb:6:in 'raiser': Exceptions::FooError (Exceptions::FooError)
> from app.rb:15


But after changing from `"foo"` to `"bar"` in `call` variable output of `app.rb`
will be:


> app.rb:8:in 'raiser': This is bar! (BarError)
> from app.rb:16


We are getting a little more information with it than plain generic `standarError`
and in the future this little info could make our work with debugging a lot easier
and faster. Of course this is dead simple example you could make your errors more
sophisticated. For more reference hit:
[ruby-doc.org/Exception](https://ruby-doc.org/core-2.2.0/Exception.html)

Cheers
