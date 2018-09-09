---
layout: post
title: "Refactoring the deals"
date: 2018-09-09 18:35:00
categories: python
---

Today we will refactor my recent code.
To be less.. to be better.

Here's how the `deals` have changed:

```
import re
import requests
from bs4 import BeautifulSoup


class Deal:
    """Main container of the script"""
    def __init__(self, game, price):
        self.game = game
        self.price = price

    def choose_game(self):
        """This method converts user inputed game to a valid url """
        if not self.game:
            exit("No game typed!")
        raw_url = "https://store.playstation.com/pl-pl/search/" + self.game
        url = raw_url.replace(" ", "%20")

        return url

    @classmethod
    def finder(cls, url):
        """This method finds a game in psstore"""
        req = requests.get(url)
        data = req.text
        soup = BeautifulSoup(data, "html.parser")
        div = soup.find("div", {"class" : "grid-cell grid-cell--game"})
        if div != None:
            formated_div = " ".join(div.get_text().split())
        else:
            exit("No game found :(")

        print(formated_div)
        return formated_div

    def pricetag(self, res):
        """This method sets and compares a price range of a game"""
        if self.price != "":
            try:
                price_max = int(self.price)
            except ValueError:
                exit("Price must be a number")
            res_nums = re.findall(r'\d+', res)
            if int(res_nums[-2]) <= price_max:
                exit("Go get it tiger!")
            else:
                exit("No go, man!")
        else:
            exit()

if __name__ == "__main__":
    PRICE_VALUE = input("Choose maximum price value (blank no price): ")
    GAME_NAME = input("Please enter game name: ")
    DEAL = Deal(GAME_NAME, PRICE_VALUE)
    print(DEAL.pricetag(DEAL.finder(DEAL.choose_game())))
```

First of all there is finally a proper `init` method to initailize the class with two
arguments. I had to alter all of those places where the old form of the variables was present.
`choose_game` method is almost the same besides the variable change.


In the `finder` method and in the rest of the code I've merged `print` and `exit`
to be one. Less code, clearer does the same thing.


`Pricetag` method got one additional upgrade. I've added a fail-safe solution when someone
will try enter a value not convertible to integer.


Now after adding the `init` method to the class the invocation of the class and methods have also changed.
Inputs are outside of the class stored in separate variables. Invoking now looks slightly better and less
complicated.
Another new thing is the line `if __name__ == "__main__":` before the call.
It's job is to make sure that the code will be executed when we want to run the module as a progam
and not when someone is just importing it and calling the functions/methods (for testing purpose for example)
Without the main check the code would be executed everytime.


Still probably not the best python code in the world but looks and behave better then before.
Got to write some tests to it now...


Cheers
