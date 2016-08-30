---
layout: post
title: "OOP Ruby way"
date: 2016-08-30 12:00:00
categories: ruby
---

OOP Ruby way

I admit, I couldn't handle it.
I've rewrite this JS OOP thing to ruby.
In Ruby OOP is by far more natural and more logical to me.
Don't get me wrong I still think OOP in JS was best thing to
happen to it in long time but I had a notion that it is still
like hacking it to be true OOP. Well never mind here you go,
the Ruby way:

```
class AuthorObject
  def initialize(firstName, lastName)
    @firstName = firstName
    @lastName = lastName
  end

  def display
    p "Script author: " + @firstName + " " + @lastName
  end
end

author = AuthorObject.new("Mateusz", "Grotha")
author.display

class LanguageClass
  def initialize(language, style)
    @language = language
    @style = style
  end

  def print
    p "My language is " + @language + " with " + @style
  end
end

class Logo < LanguageClass
  def initialize(language, style, logotype)
    @language = language
    @style = style
    @logotype = logotype
  end

  def matchLogo
    p "Logotype of " + @language + " is " + @logotype
  end
end

firstLanguage = LanguageClass.new("Ruby", "ruby way")
firstLanguage.print
secondLanguage = LanguageClass.new("JS", "OOP")
secondLanguage.print
jsLogo = Logo.new("Javascript", "OOP", "yellow circle with black JS letters")
jsLogo.matchLogo
rubyLogo = Logo.new("Ruby", "ruby way", "ruby")
rubyLogo.print #method from parent class
rubyLogo.matchLogo
```

And after I wrote it I just had to write some tests:

```
require "./rubyOOP"

describe AuthorObject do
  before(:each) do
    @a = AuthorObject.new("one", "two")
  end

  context "initalilize method" do
    it "returns ArgumentError when invoked without arguments" do
      expect{AuthorObject.new}.to raise_error(ArgumentError)
    end

    it "do not raise an ArgumentError" do
      expect{@a}.to_not raise_error
    end
  end

  context "display method" do
    it "displays 'Script author:' string with given arguments" do
      expect(@a.display.to_s).to include("Script author: one two")
    end
  end
end

describe LanguageClass do
  before(:each) do
    @l = LanguageClass.new("one", "two")
  end

  context "initialize method" do
    it "returns ArgumentError when invoked without arguments" do
      expect{LanguageClass.new}.to raise_error(ArgumentError)
    end

    it "do not raise an ArgumentError" do
      expect{@l}.to_not raise_error
    end
  end

  context "print method" do
    it "prints 'My language is' string with given arguments" do
      expect(@l.print.to_s).to include("My language is one with two")
    end
  end
end

describe "Logo" do
  before(:each) do
    @logo = Logo.new("one", "two", "three")
  end

  context "initialize method" do
    it "returns ArgumentError when invoked without arguments" do
      expect{Logo.new("one", "two")}.to raise_error(ArgumentError)
    end

    it "do not raise an ArgumentError" do
      expect{@logo}.to_not raise_error
    end
  end

  context "matchLogo method" do
    it "prints 'Logotype of' string with given arguments" do
      expect(@logo.matchLogo.to_s).to include("Logotype of one is three")
    end
  end

  context "inherited print method from LanguageClass" do
    it "prints 'My language is' string with given arguments" do
      expect(@logo.print.to_s).to include("My language is one with two")
    end
  end
end
```

Maybe not an end-to-end testing but touches the basic funcionality.

Cheers.
