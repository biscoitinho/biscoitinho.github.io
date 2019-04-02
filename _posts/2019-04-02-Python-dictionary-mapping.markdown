---
layout: post
title: "Python dictionary mapping"
date: 2019-04-02 19:35:00
categories: python
---

Once in a while you face a task of writing an If statement.
If the If is simple there is no problem but what if...


The If have to have more then 10, 20 elifs ? The job gets
tedious very fast, fear not, there is a nice remedy for that.


For a start let's write a couple of lines of a classic if:

```
if x == "1":
  return "aaaa"
elif x == "2":
  return 3 + 6
elif x == "3":
  return "ccc"
```

and on, and on, and on...


Not the most exicting thing in the world, and there is a lot of typing.
We can do better. Let's make a dictionary:

```
DICT_MAP = {
  "1": "aaaa",
  "2": 3 + 6,
  "3": "ccc"
}
```

Ok now we need some DRY solution to get this data:

```
def get_values_from_keys(input_var):
  return DICT_MAP[input_var]
```

and to use it:

`print(get_values_from_keys("1"))`


Nice! but the is more, We can also get data not only through keys, but also by values:

```
def get_keys_from_values(input_var):
  for key, value in DICT_MAP.items():
    if value == input_var:
      return key
```

Let's check `print(get_keys_from_values("ccc")`


Dictionary is even more awesome, we can store functions inside!

```
lamb = lambda x: x **2

DECISION_MAP = {
    "1": "aaaa",
    "2": 3 + 6,
    "3": "ccc",
    "4": lamb(7)
}
```

To find out what is the outcome of function `lamb` with argument 7
just call `print(get_values_from_keys("4"))`

Of course there is also a switch statement as an alternative to
a larger If, but this way in my eyes looks more clear and takes
a little less typing.

Cheers
