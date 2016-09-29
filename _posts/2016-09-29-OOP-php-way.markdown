---
layout: post
title: "OOP PHP way"
date: 2016-09-29 12:00:00
categories: php
---

OOP PHP way

Next chapter of my OOP micro series, this time PHP and a similar
case. Of course I could use some third-party lib and make my life
a little bit easier, but I wanted to make it as raw as possible
to get best comparison to two previous ones in Ruby and JS.

```
<?php

class Author {
    private $firstName;
  private $lastName;

  public function __construct($firstName, $lastName)
  {
      $this->firstName = $firstName;
      $this->lastName = $lastName;
  }

  public function display() {
    echo "Script author is ";
    print_r($this->firstName . " " . $this->lastName);
    echo "\n";
  }
}

$scriptAuthor = new Author("Mateusz", "Grotha");
$scriptAuthor->display();

class Language {
  private $lang;

  public function __construct($lang) {
    $this->lang = $lang;
  }

  public function show() {
    echo "My lanaguage is ";
    print_r($this->lang);
    echo "\n";
  }

  public function sentenceLink() {
    echo "with an ";
  }
}

$myLang = new Language("PHP");
$myLang->show();

class Logo extends Language {
  private $logo;

  public function __construct($logo) {
    $this->logo = $logo;
  }

  public function showLogo() {
    print_r($this->logo);
    echo " logo";
  }
}

$logotype = new Logo("Elephant");
$logotype->sentenceLink();
$logotype->showLogo();

?>
```

Probably not the best php script in the world but touches the concept.
Cheers
