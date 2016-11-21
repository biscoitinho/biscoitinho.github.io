---
layout: post
title: "Introduction to Rails Service Objects"
date: 2016-10-28 17:00:00
categories: ruby
---

Introduction to Rails Service Objects

I won't tell you what a service object is, there is a plethora
of articles out there in the wild (web). Here I will show you
how to make one yourself!

As we will be working today in rails there is a little bit of
setup first ahead of us. If you already got some working rails project
you can skip it and move to the `service object` part.

> The setup

First of all we need a rails app so go ahead and fire up:
`rails new service_rails` and hit enter `cd` into it and
a quick scaffold to get started:

`rails generate controller Articles`

`rails generate model Article`

`rake db:migrate`

Now navigate to the `/config/routes.rb` and add a line:
`root 'articles#index'`
Open `app/controllers/articles_controller.rb` and add index method:

```
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
  end
end

```

Next thing - the view. Navigate to the `app/views/articles` and
`touch index.html.erb` In index file add some content, for example:

`<h1>Articles</h1>`

That's it we are set up to make our first service object. If something above is not clear
don't worry [here](http://guides.rubyonrails.org/getting_started.html) it is all covered.
Now after hitting `rails s` you should see "Articles" heading from our view.

> The service object.

Now the fun part.
In the `app` folder let's make another directory: `mkdir prints` and `cd` into it.
Now let's make our object: `vim article_print.rb` and hit enter.
We want to make a method which prints greetings to the screen so:


```
class ArticlePrint
  def print_txt
    p "Greetings from the Service Object!"
  end
end

```

Can't get simplier then this.

What's next? We have to make it work somehere. Navigate to the article controller and add one line
to `index` method:


```
class ArticlesController < ApplicationController
  def index
    @articles = Article.all
    @print = ArticlePrint.new.print_txt
  end
end
```

We are declaring a variable with `print_txt` method from `ArticlePrint` class (our service object)

Next step is display it somewhere, so let's go to the `index.html.erb` in `views/articles`
and add one line below our heading:
`<%= @print %>`

It calls our variable from `articles_controller` file

That's it - our first service object in rails. Go check it out by hitting `rails s`
Of course it doesn't do much but you could use service objects for much more sofisticated purposes with much,
much more code inside them (and not in your models or controllers)

Cheers

