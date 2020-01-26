---
title: "Arts adventures in functionland"
date: "2019-10-18"
author: "Art"
cover: "img/functional.jpg"
tags: ["functional", "programming", "python"]
keywords: ["functional", "programming", "opinion", "python"]
description: "Exploring python environment management"
showFullContent: false
---

{{< mathjax >}}

_Oh boy, where do we even start._

My colleague, an ardent mathematician, kept telling me about functional
programming for what basically is my whole programming life. I come from an
economist background so for me the terms `Object-Oriented` and `functional` did
not mean much. Also, I think early on in our journeys on the path to learn how
to write code we don't concern ourselves with paradigms (which is what those
terms meant).

However, as you get better and better at the programming and stop worrying about
indentation errors and learning syntax, your brain craves for what [Raymond
Hettinger](https://github.com/rhettinger) calls _**"there must be a better way"**_
moments. Those moments where you step back, look at whats infront of you and
realise that you might need to abstract stuff or maybe figure out a better way
to organise your code.

So after coding in python for 3 years, I listened to my mathsy friend and
learned a bit of `Haskell` which is a great functional programming language.
Basically the poster boy of functional programming. I will try to convey the
lessons I learned from that to you in this blog thing and showcase some things
in python that would allow you to do functional programming.

Also I won't go into Object Oriented stuff. I think I'm definitely not qualified
to talk about all that. But my aim will be to talk why I at the very least
started thinking more functionally about my code and hopefully make a case on
why you should at least try it.

## Functional Programming

**Disclaimer:** This is how I understand the subject matter. I'm not a math
scientist person. If you want more details heres the [wikipedia
link](https://en.wikipedia.org/wiki/Functional_programming) for convenience.

Maths is something I love, but don't do well enough. So you could call me an
enthusiastic cheerleader for maths. So I'll keep this light on the maths but
sprinkle in notation as and when.

So what is a `function`? A function is something that associates every element
of a set of inputs to a unique element of an output set. Or at least something
along those lines anyway. When it comes to the world outside of maths, a
function in say python looks something like `func(input1, input2)`. Well in that
case the inputs are mapped to an output (if nothing is returned it got mapped to
the `None` value).

Now python doesn't check what comes into the function versus what is _supposed_ to
come into the function. Same goes for the output side which means as soon as it
hits a snag, because you haven't passed the right things into it, will kick up a
fuss (document your code). But I digress.

Functional programming sort of forces you to think about your code as a set of
functions. A program is then defined almost like this:

$$f(g(z(h(x))))$$

Let us break it down a bit. You have some input `x` and then you pass it to
function `h` which then does something to it (maps it to some output). After
this is done it then passes whatever comes out of it to the next function (`z`
in this case). Let me rewrite this with more relatable names

$$publish(report(clean(load(filepath))))$$

One great thing when your code conceptually looks like this (if you're say a
data scientist or someone building pipelines) is that you get a nice flow of
data or any other input through your system in pipeline. Say I forgot a step
(and I did) of creating statistics before I generate my report. Well the simple
way of fixing this issue in our little functional world is to simply insert
another function. You know what has to come in and what has to come out in each
stage after all so if you respect that flow you're all set to go!

After adding that extra step we get:

$$publish(report(stats(clean(load(filepath)))))$$

Before we rush off and start looking at python I think its important to bring up
the concept of `composition`. As you might have seen writing all of those
brackets constantly is annoying and for larger programs it would get more and
more annoying over time. This is where composition comes in handy.

Composition essentially allows you to join functions together into a much larger
function (basically what we've been doing to write our 'pipeline'). In maths you
would show composition like this:

$$f \circ g$$

which is identical to

$$f(g())$$

so our old pipeline can be expressed as

$$pipe = publish \circ report \circ stats \circ clean \circ load$$

and then we can do

$$pipe(filepath)$$

Remember, respect the order of what has to come in and what has to come out!

## Map

Since not only our pipeline, but the whole program in itself has to be function
composed out of smaller functions, functional programmers have tools at their
disposal that help them solve very specific issues.

The one I wanted bring up is `map`. Not as in Mercator and projections and
such (looking at you Dan), but more going from one thing to another.

Mapping means (pedantic maths people see disclaimer above) that you have some
sort of structure (often times a list) and a function and then proceed to apply
the given function onto the elements of the structure.

So think of an array (list in python for example), then we would take a function
like say `f` and applied it to each element of the list. So it might look
something like this if we tried to `square` all the values from 0 to 3.

$$
\begin{align*}
&input: [0, 1, 2, 3] \\\\
&function: square \\\\
&produces:[square(0), square(1), square(2), square(3)]
\end{align*}
$$

## Partial Functions

Another common (and useful!) concept to be aware off is `partial` functions. As
you may or may not know you can pass in default arguments when you define
functions in python. Meaning that if you don't pass values for a given
parameter, it will fill it with defaults. Similarly if you are using a function
over and over and need to pass in the same arguments over and over, might as
well create a prefilled version of it!

We'll cover how this is done in python later and it will be much clearer!

## Functional stuff in python

By now you might be quite annoyed by all the abstract stuff. So for the
sake of actually having some python in here lets translate our pipeline to
python.

```python
publish(report(stats(clean(load("data.csv")))))
```

The more eagle-eyed among you might have realised I am not using composition.
That is because that doesn't exist by default in python unfortunately. But you
could write your own composition function like this:

```python
def compose(outter, inner):
#     0      1      2       3        4       5
    return lambda *args, **kwargs: outter(inner(x))
```

There are nicer ways to do this but this is the simplest and fastest way I could
think of at the moment. Lets break it down!

0. return whatever follows
1. we will be returning an anonymous function ([more info](https://en.wikipedia.org/wiki/Anonymous_function#Python))
1. the `*args` bit captures any arguments we pass in
1. the `**kwargs` will contain any `key=value` style arguments (keyword
   arguments)
1. thats the outter function
1. is the inner function that we're wrapping up

Neat. There are nicer and more user friendly composition functions we can create
but for the sake of our little exploration that one is just a nice and simple
example. Better ones could be functions that take in more than 2 functions for
example `compose(publish, report, stats, clean, load)` could literally produce
our whole pipeline.

### Generators

One thing I want to mention is generators. Some of you might be familiar with
the list comprehension syntax in python which lets you generate lists like the
squared values from 1 to 3 (see Map section above) easily like so

```python
[square(val) for val in range(4)]
```

Now the issue is that when you get to ranges or data that is really large, you
are in essence creating a maaaaasive array in memory. What if I wanted to create
an array and the filter out the values that I don't need. Well generators come
in handy!

A generator is basically what it says in the name, it is a procedure or object
that will generate certain values. Say we had a generator that would generate
square values and as we looped through it would give us the square of the next
number. Now it doesn't have all these values in memory. It just knows how to get
the next one and the one after. If we had such as thing, then we could filter
our values as we go along rather than create a massive array and then filter.

Funny enough, `map` as a function already exist in python and is a generator!

```python
squareGen = map(square, range(4))
```

This would give us a generator, not a list and if we wanted it to be a list we
would have to pass the generator to the `list` function. Now if we wanted to
filter some values we could use `filter` which takes in a function (which has to
be able to go from input to a True/False value) and an iterable. It will only
keep values in the iterable if after it has passed the each value to the filter
function, the function returns True. So for a filter that wants values that are
even, it will pass the number 0 in, then check with if it is something it wants
to keep using the function supplied and in this case it would!

To put it in code

```python
filter(lambda x: x % 2 == 0, range(42))
```

We can then use the map and the filter together!

```python
filter(lambda x: x % 2 == 0, map(square, range(42)))
```

This will run instantly, but unless you pass the resulting generator into a
`list` function, or loop through it in some other way, you'll be stuck with a
`filter` object.

### Functools, Itertools and other nice things

So where do all these functional things sit in python? Well things like `map`,
`filter` and `range` sit already very nicely in python without the need for
imports. However python has 2 libraries `functools` and `itertools` that will
cover you for quite a lot of the functional programming needs.

Why `itertools`? Well, as you learn more functional programming and use more of
it you'll eventually see that you need to use `for` loops and such, however all
the clever tools in itertools will help you create useful iterators that you can
apply functions to instead of looping in the traditional sense of `for` and
`while` loops.

The other little package I would like to highlight is
[toolz](https://github.com/pytoolz/toolz). This package is like itertools and
functools in one and on steroids. It has a nice little drop in replacement
called `cytoolz`, but I'll leave the googling to you. `toolz` is great as it has
things that make doing functional programming much easier, like a nice `compose`
function that we already covered.

`functools` and `toolz` have in them tons of stuff but one thing I promised to
talk to you about is `partial` so I will briefly cover it here. Say we usually
find ourselves using rounding and constantly want to round to the same accuracy
(lets say 3 decimal places). So instead of typing `round(my_value, 3)` we can do
something like this

```python
from functools import partial
round3 = partial(round, ndigits=3)
```

This allows us to use `round3` and it will always be rounded to 3 decimal places.

## Side Effects

Ok last thing I want to cover is side effects. So you know how in math if I gave
you the function

$$
y = x^2
$$

You know what the graph would look like regardless of when you plotted it. Day,
night, morning, afternoon, if it rains, if it snows etc. Basically, the function
describes the process of going from one thing to another right? So this is true
always. In a world where this is true in programming, there would be much less
coding errors.

The basic idea is that a maths function would not depend if some file exists on
disk. In programming, you need to read files sometimes and they could be
missing, hence you'd get an error! Oh no! You want to limit those or at least
know where they can happen! Reading files and writing files, connecting to
databases basically relying on and changing state outside of your pipeline can
cause issues and make your functions not behave like maths functions!

This becomes an issue if you run things out of order for example. So keeping
your side effect heavy functions segregated and clearly labelled as such etc will
save you a headache or two in future.

I think I haven't explicitly stated this, but you might infer this (and
definitely realise this when you work in FP), another property of the maths
functions is that one thing comes in one comes out. Conceptually. If you stick
with this and avoid coding in side effects where you don't need to you'll be
better off.

## Summary

This whole post is a massive simplication. Massive simpliciation! Of a subject
matter that is quite expansive. I think if you are keen to learn some of the
things I've described here, _go learn haskell_. And if you are tempted to give
it a bash read [the Haskell Book](http://haskellbook.com/)

Anyway, fascinating subject, useful for data scientists (easy pipelining) and
super usefull to think about in order to maintain the potential points of
failure clearly labelled.
