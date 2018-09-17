---
layout: post
title: "Mocking, testing deals"
date: 2018-09-17 19:35:00
categories: python
---

Now, as I said earlier time to write some tests of our now refactored code.
Today I will be using `unittest` testing framework.
One reason for this is plain old simple need to learn new things and expand
my toolset in Python. Second one is that `pytest` docs are well... not that
good. Besides there is a `mock` sublib in `unittest` already.
I have to admit `pytest` seemed to look defenetly nicer (from a syntax stand point)
but `unittest` gets the things done so why not to give it a try.


```
from unittest import TestCase
from unittest.mock import patch
from deal import Deal

class Test_Deal(TestCase):
    deal = Deal("yakuza", "23")
    resp = "ZAOSZCZĘDŹ 50% Yakuza Kiwami Pełna wersja gry PS4 79,00 zl 39,00 zl"
    url = "https://store.playstation.com/pl-pl/search/yakuza"

    def test_class_Deal_initialization(self):
        self.assertIsNotNone(self.deal, msg=None)

    def test_choose_game(self):
        self.assertEqual(self.deal.choose_game(), self.url)

    @patch('deal2.Deal.finder', return_value= resp)
    def test_finder_mocked(self, finder):
        self.assertEqual(finder(self.url), self.resp)

#Test without patch calling to the website directly
    def test_finder(self):
        self.assertEqual(self.deal.finder(self.url), self.resp)

    def test_pricetag(self):
        with self.assertRaises(SystemExit) as error:
            self.deal.pricetag(self.resp)
            self.assertEqual(error.exception.message, 'No go, man!')
```

Ok, let's go through it one by one.
First three lines are standard imports, for unittest library, our code in the `deal.py` file
and a `mock` library - to be specific a `patch` module.

Afterwards we are stating that we will test a class with methods.
In the class itself we declare three variables. `deal` variable is an instance of a our `deal`
class with already provided inputs ("yakuza" and "23"). As for the other two we will get to them later on.


Time for the first test. `test_class_Deal_initialization` checks if the class is initalized properly.
In that case it tests if the value returned after initialization is not none. Simple, basic test, gets things
going. Next test `test_choose_game` is making sure that `choose_game` method returns the right value when called.
That is our third variable from the begining - `url`.


`test_finder_mocked` is when the fun begins. Here we have to go back to our second variable `resp`.
Value of `resp` (don't mind those funny polish letters) is a response from the site at a given timeframe. When I
was writing those tests the promo was still valid, but next day discount have ended and the response was different.
So, every time there will be a new promo or it will end our tests will break because they are expecting different
values. There are two solutions to this problem. One - everyday check for the promo and alter tests if nessesary
(a silly one). Two - patch/mock the call to the website with a made response and then it no longer needs to call to the
website and wait for the response but instead get it right away from the value from the `resp` variable.
This solution got two benefits. Tests are significantly faster and are more bulletproof in case of connectivity to
the server to which the methods are calling.


All of this paragraph above is compressed to one line starting with `@patch`. Below the patched/mocked test I wrote
a standard test directly calling the website for comparison.


Finally last `test_pricetag` is expecting a `SystemExit` error because with such values ("yakuza and "23") method
shoud return `exit()` function with "No go, man!" message.


For sure not a 100% coverage of tests, but a fairly simple introduction to `unittest` library and (very) basic mocking.


To be honest I'm still missing `Rspec` greatness, but as I said in the begining `unittest` gets things done and in the
end this is the most important thing.


Cheers
