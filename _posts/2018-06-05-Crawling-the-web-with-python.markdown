---
layout: post
title: "Crawling the web with python"
date: 2018-06-05 12:00:00
categories: python
---

Today we will do some crawling, but instead of barbwire
it will be the web.

I've made a simple table with two columns:



| Document | Last edit |
|---------|---------|
| doc1 | 01.01.2008 |
| doc2 | 14.05.2016 |
| doc4 | 23.08.2017 |
| doc5 | 30.09.2018 |
| doc6 | 23.08.2017 |
| doc7 | 23.05.2018 |
| doc8 | 02.06.2018 ||




So, now when we got ourselves some data to munch through let's
write some code in python:



```
import datetime
import requests
from bs4 import BeautifulSoup


class Crawler:
    @classmethod
    def user_url(cls):
        response = input("Please enter url: ")
        if not response:
            print("Invalid URL!")
            exit()
        return response

    @classmethod
    def timeframe(cls):
        six_months_ago = datetime.date.today() - datetime.timedelta(6*365/12)
        start_date = datetime.date(2015, 1, 1)
        diff = six_months_ago - start_date
        timeframe = []
        for i in range(diff.days + 1):
            timeframe.append(
                datetime.datetime.strptime(
                    str(start_date + datetime.timedelta(i)),
                    '%Y-%m-%d').strftime('%d.%m.%Y')
            )
        return timeframe

    @classmethod
    def checker(cls, url, tabf):
        req = requests.get(url)
        data = req.text
        soup = BeautifulSoup(data, "html.parser")
        date = datetime.date.today()
        print("Today is " + str(date) + " Check which files needs an update:")
        print("\n")
        table = soup.find('table')
        if table:
            table_rows = table.find_all('tr')
            for trow in table_rows:
                tdata = trow.find_all('td')
                for i in tdata:
                    print(i.get_text())
                    if any(x in i.get_text() for x in tabf):
                        print("Document needs an update!")
        else:
            print("No data table!")

c = Crawler()
c.checker(c.user_url(), c.timeframe())
```

Probably not THE best python code you'll ever see - I'm still new to this
language, but it does the job. To be honest it needs still a handful of
tweaks, but for the learning purposes the simpler the better, well at least
for now.

What it does?

It takes a given url by the user and checks if there is a table in the webpage.
When it does find one then iterates through it and check which files were edited
more than six months ago. When it finds such files it prints a message
that the file should be updated.
There is no elaborate mechanism checking which table to iterate, just as simple
as it can get - when it finds one it iterates though. Maybe not the most
bulletproof and clever solution in the world, but for the sake an purpose
 of this task it's sufficient enough.


OK, so from the top:
`user_url` method - pretty simple it takes input form the user and returns it.
When the input is empty it quits to cmd. (of course it can be smarter,
but for now it will do).

Next we got `timeframe` method. There is a little "magic" to it.
`start_date` - We are pointing from where in time the checking should start.
For instance when we know that we haven't got any older files then 01-01-2015
there is no point of iterating further.
The `six_months_ago` variable, here we have to calculate a delta for a six
months from now to determine from which point our files will be in a need
of update.

`diff` - well, subtract.

Now we iterate through the whole time range and add all this dates to an array.
There is a little obstacle in the way - dates in the table are in different
format then the ones in the array by default so we have to `strftime` them.
Afterwords method returns `timeframe` array with converted date format matching
one in the table above.

For the `checker` method I've used `BeautyfulSoup` and `requests` libraries
to parse and fetch the data from the given url/website.
Couple of the first lines in the method are mandatory for the libs to work.
Then there is simple message with a converted todays date to a string.
When the table is found and if any of the dates from timeframe match the dates
in the table the message of a needed update is printed and if there is no table found,
well... the no table message is printed out to the screen.

Now go ahead and try it.
Fire it up in the cmd and paste this link:

`http://biscoitinho.github.io/python/2018/06/05/Crawling-the-web-with-python.html`


CONCLUSION

Python is very simillar to ruby. Writing in it comes with ease and its intuitive.
Loops are simplier and clearier to me then in ruby (less to choose from).
On the downside I haven't thought that I'm so bond to the magic ruby methods.
It was of a little hustle at first not getting to use one of them.
Overall writing this was a time well spent.
Now it's time to tune it up a little bit and maybe add some smarter features to it.


Cheers
