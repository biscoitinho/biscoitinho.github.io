---
layout: post
title: "Charming away C plus plus"
date: 2019-03-13 19:35:00
categories: C
---

Last time we did some demystifying of a C language, now it's time
to do the same to his younger brother C plus plus.

We will start in the same manner - the same example as the last time.
That is calculating the circumference of a circle and checking the
code with yet another awesome minimal testing framework
[Catch2](https://github.com/catchorg/Catch2) this one also consists
of a one single file, but it's a little more complicated (not in usage)
compared to the C minunit.


Ok, no time to waste let's get down and dirty in coding right away.
First thing let's make a header file called `circle.hpp`

```
class Shape {
  public:
    double pi = 3.14;

  double circle(int r) {
    return pi * r * r;
  }
};
```

As u can see code is almost identical to the one in C. We could even
write the same code and it would work, but what is the point in doing
it in C++ and not using it's features. In this case only one actually -
a class. Yeah, I know nothing mindblowing, but keeping it simple, and
just to distinguish the C from C++ code. All in all C++ it's a OPP
variant of C (probably C++ devs will hate me for that).
So, we have here a `Shape` class and a public variable `pi`.


Next `main.cpp` file:

```
#include <iostream>
#include "circle.hpp"
using namespace std;

int main() {
  Shape Shape1;
  cout << Shape1.circle(5) << endl;

  return 0;
}
```

Little different here. First two lines pretty straightforward including
standard library and our file with the circle code.
Next one is a shortcut for not typing every time `std::cout` (or any
other method from iostream lib). With the namespace we can get away
with only `cout`. In the main function we are declaring a new class
instance `Shape1`. After that we are printing the output (cout is for
printing) of our circle function with an argument of 5 and ending
the line. Traditonally returning 0 at the end of the main function.


Next `catch.hpp`. Just go tho the catch2 github and copy the
header file from there.

Time to write a test for our new shiny function. Let's make a
`test_circle.cpp` file:


```
#include <iostream>
#include "circle.hpp"

#define CATCH_CONFIG_MAIN  // This tells Catch to provide a main() - only do this in one cpp file
#include "catch.hpp"

Shape Shape1;

TEST_CASE( " circumference of a circle being computed", "[circle]" ) {
    REQUIRE( Shape1.circle(10) == 314 );
    REQUIRE( Shape1.circle(2) == 12.56 );
    REQUIRE( Shape1.circle(1.66) == 3.14 );
}
```


To be honest, I don't think it needs further explanation, pretty much
plain old english language here. We just have to remember to make
a new instance of a class and call our function from inside it.
Now let's find out if it's passing or not.


To complie the code run `g++ main.cpp -o main` and
`g++ test_circle.cpp -o test_circle`

Execute code with `./main` and test it with `./test_circle`

We are in luck - three passes

Cheers
