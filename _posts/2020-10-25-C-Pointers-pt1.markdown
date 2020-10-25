---
layout: post
title: "C Pointers pt1"
date: 2020-10-25 18:15:00
categories: python
---

Hi,

2020 is a tough year, also for my consistency in writing posts, but today
I wanted to start another micro series in mythbusting C pointers.
I mean how to actually use them in real life with simple examples with no
PhD in computer science required.

What are those mythical pointers anyhow ?

After www.learn-c.org:

> A pointer is essentially a simple integer variable which holds a memory
> address that points to a value, instead of holding the actual value itself.
> The computer's memory is a sequential store of data, and a pointer points
> to a specific part of the memory.

Yeah... what it mean ? What I can do about it ?

We have to remember that C is a quite low level language, well, at least in today
standards compering it to Python, Ruby JS etc.
A LOT of things can be done in C but in a slightly different way then we are all
used to with our high level modern languages.
One of such things is `call by reference` which is:
a method of passing arguments to a function which copies the address of an argument
into the parameter. Inside the function, the address is used to access the actual
argument used in the call.


Here's my attempt to picture you this in form of a simple example:


```
#include <stdio.h>
#include <stdlib.h>
#define   MAX  10


int adder(int n) {
  n = n + 1;

  return n;
}

int adderPointer( int *p) {
  *p = *p + 1;

  return *p;
}

int main() {

  int n = 10;
  int p = 5;

  printf("%s %d\n", "Initial value of n: ", n);
  printf("%s %d\n", "Initial value of p: ", p);

  adder(n);
  adderPointer(&p);

  printf("%s\n", "======================================");
  printf("%s %d\n", "Value of n after addrer(): ", n);
  printf("%s %d\n", "Value of p after addrerPointer(): ", p);

}
```


So, now "the same" behaviour in Python 3.8:


```
n = 10
p = 5

def adder(n):
  n = n + 1

  return n

def adderPointer(n):
  n = n + 1

  return n


print("Initial value of n: " + str(n))
print("Initial value of p: " + str(p))

adder(n)
p = adderPointer(p)

print("======================================")
print("Value of n after addrer(): " + str(n))
print("Value of p after addrerPointer(): " + str(p))
```

Outcome is identical but C version is waaay faster.
C as a compiled code will always be faster that's one thing,
but in this example it's additionally working on pointers,
which are well.. pointing to the memory location, and not
directly allocating memory for new variables and working on
them. It's more confusing to read and understand, more ugly
in terms of estetics, harder to debug if something will go wrong
but also incredibly fast, efficient and consumes minimal resources.
Basically definition of C language...

I hope it wasn't that messy, pointers are a difficult thing to
grasp and to master, but it's a very powerful tool.

I'll continue with this little series with another examples.,
maybe arrays will be next ?

Cheers
