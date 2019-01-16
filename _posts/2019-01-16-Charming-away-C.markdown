---
layout: post
title: "Charming away C"
date: 2019-01-16 19:35:00
categories: C
---

> C is so hard and so low level that you must have a PhD to understand it.

The above is a common conception, is it true ? Let's find out.
To check if it's true we will make a function which does a mathematical
calculation - all in all it is C so it must be scientific and all...
We will do math, but primary school level example.
To start let's make a file named `circle.h`.

```
int circle(int r) {
  int result = 3.14 * r * r;
  printf("%d", result);
  return result;
}
```

Basic stuff - circumference of a circle, looks simple but let's do it line by line.
'int' tells us that function will return an integer value. `circle` is the name of
the function. `int` inside the parentheses tells us that the argument `r` of the function
must also be an integer. On the second line we declare a variable `result` which is
also of a type integer and inside it we calculate the circumference. Third line
prints the result variable to the screen - `%d` means that we are passing a digit
value to the `printf` function. Fourth line returns the variable and ends the function.


So far so good, ok so now how to run this ? C lang need a `main` function, easiest way
of thinking of it is as a runner container, anything you'll put there will be executed.
So, let's make one now. We need another file `main.c`

```
#include "stdio.h"
#include "circle.h"

int main(void) {
  circle(12);

  return 0;
}

```

What happened here? First two lines fetches necessary files and libraries. First one is
a standard C library for input output operations and second is our file with the function.
Then the misterious `main` function declaration with a void argument and an integer output
of zero. Void here means that function takes no arguments. Empty parentheses on the other
hand stands for any number of arguments. Functions in C by default return an `int` so it
is a good practice to make sure function is returning the correct value and not any other
that's why there is a zero at the end. Inside it all is a call to our function with the
argument of 12.


Ok, so now we got everything in place to run our awesome code. We must type in the
terminal `gcc -o main main.c` Code will compile (hopefully with no errors) and now the moment
we all been waiting for - runing the code, to do so type in `./main` and there we have it.


Ok, so what can we do next ? Let's write a unit test for our code. This is C so it should
be almost impossible to do and very hard, right?


There is an awesome C test framework named [minunit](http://www.jera.com/techinfo/jtns/jtn002.html)
which blowed my mind, it's like quintessence of C and what it stands for - whole framework
is three lines of code. THREE. Of course it is an exaggeration to call it a full framework,
but it does the job, really.

We need to make a file called `test_circle.c`.

```
#include <stdio.h>
#include "minunit.h"
#include "circle.h"

int tests_run = 0;

static char * test_with_argument_equal_10() {
   mu_assert("circle value should be 314",
   circle(10) == 314);
   return 0;
}

static char * all_tests() {
   mu_run_test(test_with_argument_equal_10);
   return 0;
}

int main(int argc, char **argv) {
   char *result = all_tests();
   if (result != 0) {
       printf("%s\n", result);
   }
   else {
       printf("ALL TESTS PASSED\n");
   }
   printf("Tests run: %d\n", tests_run);

   return result != 0;
}

```

First three lines are the mentioned above includes - standard lib,
file with the code we wanted to test and a header file with the
framework. Next is the counter and after it our test.
We want to test our function with different argument then 12 this
time, that is 10. Basic math and the result should be 314. What we
need is to call the function with the argument 10 and assert the
value 314. Rest of the code is the framework outputs and shedule.
(to be honest it's more than 3 lines more like 17 but still pretty
damn impressive). Next thing is to make a `minunit.h` file with
it contents:

```
#define mu_assert(message, test) do { if (!(test)) return message; } while (0)
#define mu_run_test(test) do { char *message = test(); tests_run++; \
                                if (message) return message; } while (0)
 extern int tests_run;
```

Compile it `gcc -o test_circle test_circle.c` and run it `./test_circle`.


Was it so hard? I don't think so. C could be used also for such easy tasks
and quick scripts. Of course it is quicker to make the same script in
python, ruby or whatever, but C can also be a "high level" language.
When you want to code a CPU or a Martian rover it will get low level
pretty quick, but you don't do it every day and now you don't have an
excuse that it is hard and complex, it is as hard as you wanted it to
be.

Next time it will be C++ on the table...


Cheers
