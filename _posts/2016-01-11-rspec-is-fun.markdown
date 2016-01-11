---
---

Rspec & TDD

I was searching for some really SIMPLE Rspec tutorial, I mean really simple, with no fancy methods, mocks and all of this crap
(well, not crap but nothing useful for total beginner) Something for real beginners that could grasp the basic idea of testing,
but to my surprise I have found very little resources about the topic. So today some BASIC stuff and as a bonus TDD basics. Enjoy

I assume that ruby is already on board on our machine and we have BASIC knowledge of unix terminal
We `cd` to our directory with programming stuff and:

`mkdir testing && cd testing`
`mkdir lib && mkdir spec`

So what has happened ? - We created directory called testing and in it two directories 'lib' and 'spec'
In 'lib' there will be our awesome ruby program and in 'spec' our even more awesome tests.

next we create 'Gemfile'
(I'm a VIM ultras so...)

`vim Gemfile`

in the opened file:

```
  1 source "https://rubygems.org"
  2
  3 gem "rspec"
```

Ok, pretty self explanatory so moving on...
In main 'testing' folder we hit:

`bundle install`

Rspec will be installed and automagically will detect files ending with `_spec` for keeping order we will store them in `spec` folder

...and voila ! We are set now, the magic begins !

Get ready for the rspec-tdd-hiper-combo !

`cd lib` and `vim testing.rb`

and

```
1 class Testing
2 end
```

now go to the spec folder and `vim testing_spec.rb`

in this file the whole mambo-jumbo will happen:

```
  1 require 'testing'
  2
  3 describe Testing do
  4 end
```

thats it for now, keeping thing as simple as they can be
the `require 'testing'` part tells ruby to look for file `testing.rb`
Now it is time to fire up our tests ! `cd` to main folder of the testing app and hit:

`rspec`

You should see something like:

```
No examples found.


Finished in 0.0002 seconds (files took 0.11704 seconds to load)
0 examples, 0 failures
```

If so, then we are on the right track

time to get really dirty with tdd and testing !
Backing up to `testing_spec.rb`

Lets assume that we want to create method `truth` that always retuns... truth
simple, right ?

```
  1 require 'testing'
  2
  3 describe Testing do
  4   context "truth method" do
  5     it "returns true when invoked" do
  6       expect(Testing.new.truth).to eql(true)
  7     end
  8   end
  9 end

```

We are literally describing what we want:
In `Testing` class we want method `truth` which will return `true` when invoked.
So we are expecting this method to equal ...true

going to main folder and starting tests `rspec` (remember?)

ups, something is f-up... well, not exactly it is absolutly alright.
We are testing not existing method, sooo... its time to create one

time for `lib/testing.rb`

```
  1 class Testing
  2   def truth
  3     return true
  4   end
  5 end
```

Isn't it what we expect?

finally `rspec` and:

```
.

Finished in 0.00077 seconds (files took 0.07281 seconds to load)
1 example, 0 failures
```

another day, another success !

