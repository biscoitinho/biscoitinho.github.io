---
layout: post
title: "Finding game deals with python"
date: 2018-08-28 18:35:00
categories: python
---

Yesterday I was doing usual stuff - finding deals and promotions
on PSN (playstation store) as a classic geek I love games, but
also I'm cheap as a Scotish highlander, so it's only promotions for me.
(Ok, Uncharted 4 Ulimate Collector Edition was one exception)
Most of the time I'm looking for a specific game so this procedure
is very repetetive - go to the website, click on the search icon etc.
Why not geek-it-out-to-the-limit and make a CLI app for doing it
for me (or at least at this stage some parts).


Remember the crawler from two posts back? It's very similar thing.
Let's look at the code:

```
"""This module finds deals on PS games"""
import re
import requests
from bs4 import BeautifulSoup


class Deal:
    """Main container of the script"""
    @classmethod
    def choose_game(cls):
        """This method converts user inputed game to a valid url """
        game_name = input("Please enter game name: ")
        if not game_name:
            print("Invalid URL!")
            exit()
        raw_url = "https://store.playstation.com/pl-pl/search/" + game_name
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
            print("No game found :(")
            exit()

        return formated_div

    @classmethod
    def pricetag(cls, game):
        """This method sets and compares a price range of a game"""
        price_input = input("Choose maximum price value (blank no price): ")
        if price_input != "":
            price_max = int(price_input)
            res_nums = re.findall(r'\d+', game)
            if int(res_nums[-2]) <= price_max:
                print("Go get it tiger!")
                exit()
            else:
                print("No go, man!")
                exit()
        else:
            pass

DEAL = Deal()
GAME_DEAL = DEAL.finder(DEAL.choose_game())
print(DEAL.pricetag(GAME_DEAL))
print(GAME_DEAL)
```


The libs I'm using here are the same as for crawler script excluding datetime lib.
Only added `re` a regexp library.


First method `choose_game` grabs an input from the user with the name of the game and converts
it to an url used in the query. It adds `%20` in place of a whitespaces if any present and adds
url to polish version of the PS store. Of course you can change url to your country specific.
The currency should then be also changed automatically.


Second method uses the end value from the first method and converts it to a query with a help from
our old friends `BeautifulSoap` and `Requests` libraries. When finds one it returns the value. Please note
here it returns only the first value from the page. I did it on purpose - most of the time only the first value
is the wanted one, next ones are in most cases addons or skins and other related content to the listed game.
This method works with the specific version of the page which is using this specific div class.

Finally the last method. It's comparing the amount of money which I am willing to spend with the actual current
price of the game. Using regexp here to compare the numbers. When my money is sufficient it's "a go" in other case
a bummer.


Of course as everything this too could be tunned. Maybe a cron job every day and piping the result to a file ?
That's easy, but where do I find money for more games?


Cheers
