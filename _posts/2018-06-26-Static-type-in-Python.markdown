---
layout: post
title: "Static type in Python"
date: 2018-06-26 12:00:00
categories: python
---

Now in python 3.6 and newer we can write programs with
static type checking. It's a feature and something
completely not obligatory. You can write program with
them and basic interpreter will omit them, but if you
do an additional check with `mypy` then the static type
will check your code for errors.


So, to use this new funcionality only thing we need
is to install `mypy` and the rest is already in the
language.

```
pip3 install mypy
```

...or any other way, and we are good to go.


Here are some basic examples:

```
var1: str
var2: int

var1 = "hello"
var2 = 21

print(var1)
print(var2)
```

I think the most basic one. First we declare that `var1` and `var2`
should be for the first one a string type and for the second an integer.
Next thing we are assigning values to the variables. For the first one
a string "hello" and for the second int of 21 and then print them to
the screen.
Next step is to fire up `mypy name_of_the_file.py`.
Nothing is showing, that means we are good, at least for now,
but what happens when we mix the values ?
Let's try to assign not a `21` but a string "21".
Now the outcome of mypy check will be:

```
error: Incompatible types in assignment (expression has type "str", variable has type "int")
```

Great, so now we know something is not right and we need to fix it.
There is one thing to remember though - when you run the script normally
`python3 name_of_the_file.py` it will run fine without any error.
This type checking works more like a linter then an interpreter.
So think of it as a additional linter and not a build-in strict funcionality.
Having this sorted let's move on with another example this time
a little more complicated:

```
class Person:
    def __init__(self, name: str, age: int) -> None:
        self.name = name
        self.age = age

    @classmethod
    def calc(cls, num1: int, num2: int):
        return num1 + num2

    def other(self) -> str:
        return self.age

p = Person("Bob", "11")
p.calc(11, "31")
p.other()
```

Let's start with the method declaration. Besides the classic `self` or `cls`
now next to the argument we declare a type of it eg. `name: str` and this
arrow at the end means what the method should return. It is not mandatory
as you see in the second method, but it's a nice touch.

Second method is an iteresting one. Here we declare the arguments to be
`int` but python itself won't allow us to add something different then
numbers (it's not javascript after all...). So basiclly we could get away
without static types here becouse Python dynamic types will do that for us.

Third one is a simple return of a string.

Now, for the calls.
When running mypy the invoking of the class will raise an error.
There should be string and an integer and there are both strings.
`p.calc(11, "31")` won't work also, it should be int in both cases.
`p.other` will fail too. We stated that it should return a string,
but in the method it's `self.age` which is an integer.


All in all I think static typs a great addition to the toolset.
Great thing that it's blended in nicely to the language and not opressive
at any point, as I said it's more of a linter then anything else.
I treat it as a one more step (checking the file with `mypy`)
and an another test to the script/app I wrote. Defenetly not a something
you couldn't live without, but a nice additional protection.


Cheers
