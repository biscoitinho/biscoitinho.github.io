---
layout: post
title: "Testing Python with pytest"
date: 2018-03-11 12:00:00
categories: python
---

Uff, it's been a while, but kickin' it back, and kickin' it hard
with Python! Recently looking more and more to the python side
I've finally decided to give it a go and of course to test it at
the same time (that is - python and testing in python)

I've coupled a simple stuff, just to get hang of new syntax and
feel of the new language. In addition I've chosen a `pytest`
testing framework (It felt most `Rspec` like so to me it was
most natural).

```
class Person:
  def __init__(self, name, age):
    self.name = name
    self.age = age

  def showPersonName(self):
    return "Person name:" + " " + self.name

  def showPersonAge(self):
    return "Person age:" + " " + str(self.age)

  @staticmethod
  def counter():
    count = 0
    while True:
      count += 1
      if count >= 5:
        break
    return count

  @staticmethod
  def primes():
    prime_list = [2, 3, 5, 7]
    new_prime_list = []
    for prime in prime_list:
      new_prime_list.append("_" + str(prime) + "_")
    return ", ".join(map(str, new_prime_list))

  @staticmethod
  def interpolation():
    data = ("John", "Doe", 53.44)
    format_string = "Hello %s %s. Your current balance is $%s."
    return(format_string % data)
```

Most of it we have covered numerous times in the past with ruby
or even js. Hoever there are a couple of new/odd things to talk
about.

Let's start with the `@staticmethod`. When this method is called,
we don't pass an instance of the class to it as we  would normally
do with methods. To say it simpler it could be outside of it's
class because it isn't directly connected with it. Placement of
it inside this class is mostly for semantic/context reasons.

Next different thing may be last line in `primes()` method.
Let's go through this whole method what it does.
For a given array of prime numbers it adds an underscore sign
before and after every number in the array. Now the tricky part,
it returns the altered numbers as a one string divided with a comma.
Simple, but a little different code.

Another odd thing might be a python way of interpolating strings.
The `%s` part says: "put a first string here", when after the first
`%s` sign comes another it whould take next string from the `data`
variable. Last line of the `interpolation` method `returns`
`format_string` string with combined `data`to it.

Now when we got all this sorted let's move to the testing part.

```

import pytest
from app import Person

@pytest.fixture
def john():
  return Person("John", 22)

def test_class_Person_initialization(john):
  assert john != None

def test_showPersonName(john):
  assert john.showPersonName() == "Person name: John"

def test_showPersonAge(john):
  assert john.showPersonAge() == "Person age: 22"

def test_counter():
  assert Person.counter() == 5

def test_primes():
  assert Person.primes() == "_2_, _3_, _5_, _7_"

def test_interpolation():
  assert Person.interpolation() == \
  "Hello John Doe. Your current balance is $53.44."

```

Pretty simple, first we import pytest library, and code which
we will be testing.
Next step is to make a basic fixture for our class so
tests could be initialized from the dummy class object.
After that it's easy peasy - assertion of our methods.
For static methods we do not need fixtures they can be run
without instance of a class.

I've saved this in two files `app.py` and `app_test.py` to run
the tests type in simply `pytest`.

Couple of words for a wrap up.
I found it really easy, everyone told me that python and ruby
are very alike but this similarity blow my mind. The same goes for
`pytest` to be honest I even like it more then `rspec` now `;)`
There are differences no doubt about it. Python indeed seems more
simple or straightforward then ruby - there is less magic methods
and loops are simplified to the very essence.


It was a definitely a good idea to give it a go!


Cheers
