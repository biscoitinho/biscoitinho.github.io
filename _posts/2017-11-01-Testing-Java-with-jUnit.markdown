---
layout: post
title: "Testing Java with jUnit"
date: 2017-11-01 12:00:00
categories: java
---

Greetings, as I may say in a day such as this - I'm back from the grave.
Recently my intrests shift to more enterprise solutions but I'm not ditching ruby nowhere soon!
And because enterprise, today we will conquer testing in Java with jUnit - basic example.

Disclaimer: I know Java have big IDEs on windows, mac and even linux, but you know me - all will
be in terminal plus vim - yeah I know is torture when you are faliliar with such tools as eclipse
but what could go wrong...

First of all we need of course java installed on our system, On ubuntu is as simple as `apt-get install`
but it may vary depending on specific system, so please check how to install it on your machine.
Second we need new directory so `mkdir javaTesting` and `cd javaTesting` into it.
Then we need to download latest jUnit `jar` file and hamcrest `jar` file.
I'm using here `jUnit-4.12` and `hamcrest-all-1.3`. Hamcrest is a lib which extends funcionality of
jUnit (additional assertions, test runner etc). These files must be inside our `javaTesting` folder.
OK, now we are all set let's start coding. Create file `Hello.java` and inside:

```
public class Hello {
  public String sayHi(String expression) {
    return "Hello, " + expression;
  }

  public static void main(String[] args) {
    Hello hi = new Hello();
    System.out.println(hi.sayHi("Mateusz"));
  }
}

```

I present you - all time fav `Hello, world`!
Simple java class with two functions in first one we have our logic of hello world - standard thing nothing
to debate on really...
Second one is `main` function specific to java language in this case we are using it to call `sayHi` function
and make our script actually do something when fired up.
Java is not like ruby so we must compile it first to run it, so now we hit in terminal:
`javac Hello.java`
After this operation we will get compiled file `Hello.class` and now we can run our program with:
`java Hello` - java will automatically know which file to run so no need to write whole extention of it.
Now we get:

```
Hello, Mateusz
```

Yey, it works, now for the tests. Make a `HelloTest.java` file and inside it:

```
import static org.junit.Assert.*;
import org.junit.Test;

public class HelloTest {
  @Test
  public void saysHi() {
    String helloName = "Hello, Mateusz";
    Hello hello = new Hello();
    String hi = hello.sayHi("Mateusz");
    assertEquals(hi, helloName);
  }
}
```

Let's break it down.
`import`'s are telling java to look up to those downloaded files and make use of them.
Use jUnit and use jUnit assertions (all of them in this case but we can be more specific)
Beside this is mostly a standard test when you are familiar with any other tests this is mostly
self explanatory. We are making a `String` with a name of `helloName` with a content of`"Hello, Mateusz"`.
After this we invoke our class method and compare it if it is equal to the string provided before.

Good, now how to check if our test is of any sense and is it even working ?
Well it is still java so we need to compile it too. To do so hit this command in terminal:

```
javac -cp .:junit-4.12.jar:hamcrest-all-1.3.jar HelloTest.java
```

It says: Please compile my HelloTest.java file with jUnit 4.12 and with the help of hamcrest-all-1.3.
After this we should get no errors, so it's time for the final step - running the tests.
Still it is java so another magical command to the rescue:

```
java -cp .:junit-4.12.jar:hamcrest-all-1.3.jar org.junit.runner.JUnitCore HelloTest
```

Dear java if you don't mind please run my test with junit 4.12 and hamcrest-all-1.3.
We should get something like this:

```
JUnit version 4.12
.
Time: 0,004

OK (1 test)

```

Success! program is working and test is passing.
Let's try adding something more, open `Hello.java`:

```
public class Hello {
  public String sayHi(String expression) {
    return "Hello, " + expression;
  }

  public int[] addList() {
    int[] array = {};
    return array;
  }

  public static void main(String[] args) {
    Hello hi = new Hello();
    System.out.println(hi.sayHi("Mateusz"));
  }
}
```

We added here a new method `addList` which, well adds a list, an empty one to be exact
and returns it. Save it complie it and now for the test open `HelloTest.java`:

```
import static org.junit.Assert.*;
import org.junit.Test;

public class HelloTest {
  @Test
  public void saysHi() {
    String helloName = "Hello, Mateusz";
    Hello hello = new Hello();
    String hi = hello.sayHi("Mateusz");
    assertEquals(hi, helloName);
  }

  @Test
  public void addsList() {
    Hello hello = new Hello();
    int[] add = hello.addList();
    int[] testAdd = {};
    assertArrayEquals(add, testAdd);
  }
}
```

Pretty simillar to the above one, now we are comparing two arrays, both of them are empty so tests should pass.
Complie it and run it.

We are all green 2 of our test are passing. Now java is a little less scary.

Cheers
