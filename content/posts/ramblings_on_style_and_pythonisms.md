---
title: "Ramblings on style and pythonisms"
author: ["Art"]
date: 2020-01-08T15:24:00+00:00
tags: ["python", "programming", "standards"]
categories: ["programming", "python", "opinion"]
draft: false
---

## Motivation {#motivation}

> If you get yourself into PEP8ing mode, what do you tend to see? You tend to see
> the PEP8 stuff and not what really matters.
>
> -- Raymond Hettinger, 2015

Firstly, I am by no means a python expert, but I have been using `python` since
September 2015 (almost 4 years at the time of writing). In that time I've seen
numerous codebases written by people of varying ability and experience. I have
also worked in a development area which had a hard task of building
infrastructure for a massive organization.

As you use the language and hit your head into problem after problem, messy
codebase after messy codebase you start to wonder if there is some form of best
practice in writing consistent, clean and readable code.

Some of you probably have been in a position of looking back at a codebase you
wrote a while back and recoiling how untidy it is and how now you would \`write
it in a much nicer way\`. And to avoid this situation in the future, you might
have even jumped to your favorite search engine and tried looking for some
community driven standards. If you live in \`python-land\` you'll inevitably
stumble upon **PEP8**.

From here, you may go choose to go down one of two extreme routes:

1.  **The Great Dictator**
    One of those people who are so militant about following this standard, that
    the ideology takes priority and it can impede long-term productivity. These
    people begin to demand more and more time allocated to nitpicking the
    smallest of issues never actually seeing the larger code issues that can
    hide within.

2.  **The Dude/Dudette**
    Loves bowling, being a slacker. Hates having to do commenting, documenting code
    and sure as hell won't bother having nice versions control practices.
    Typically does what they want with a drink in their hand which might or might
    not contain a White Russian and when asked if they are aware of python coding
    standards they will just namedrop **PEP8** as if it was a junior python
    developer job interview and they want to hit the buzzword quota.

In my personal experience I've not met anyone on either of the two extremes.
Most people will end up between the two, even if nobody wants to admit that
they might be 1/5 dictator.

You _**should**_ read **PEP8**, internalize what you think will help you be clean and
consistent within you codebase and then just move on. The coding standard
quagmire is a hard one to navigate and it is why this write up essentially exists.
I'm here to try to encourage you to be more pragmatic and critical about coding
standards and to brain dump my thoughts on clean code in python.

## Thoughts about PEP8 {#thoughts-about-pep8}

Ahh, `Python Enhancement Proposal number 8`, or **PEP8** for short. Created on
05-Jul-2001 to help establish a consistent way of styling your code. However in
my experience, it has been misused to great extent. You see, the issue here is that
people want nice and tidy python code, but by evangelically following this
standard and not compromising with **PEP8** they sometimes introduce other, more
glaring issues. Even **worse**, by reviewing other peoples code and focusing
purely on the compliance with PEP8, people miss what is known as _unpythonic_
code. In other words, code that can be made much tidier and clearer by
leveraging python specific language features.

Raymond Hettinger (python core developer) has a great talk (see [here](https://www.youtube.com/watch?v=wf-BqAjZb8M)) about
**PEP8** and how to write intelligible code. We will come back to some ideas
expressed in this talk throughoutour coverage of **PEP8**. But before moving on
I'd like raise a point that he makes early in the talk to set the scene:

> What you want is that the people who are struggling a little bit (with a coding
> problem) to overcome and deal with a problem and at some point become very
> effective. How can you prevent that outcome. Its easy! You give them some low
> hanging fruit, something to go do that seems productive but isn't. And one of
> those things is a certain PEP, located between 7 and 9.

Also, I will be omitting quite a few guidelines as I cover **PEP8**, for the sake of
simplifying this writeup. By cherry-picking which ones to present, I will give
you fewer things to remember and more rule of thumb things to follow. Consider
this as me doing the hard work to suggest which are the most important parts.
You're welcome.

The one key message I want to get across before we proceed is that I do not
dislike **PEP8**. I think it's extremely useful, but I don't want people being
militant about following every bit of advice it gives without considering it for
their use case. Other than that I think everyone should read the PEP and
internalise what it tries to preach.

### A note on Consistency {#a-note-on-consistency}

The first line of the PEP8 introduction states:

> _This document gives conventions for the Python code comprising the
> standard library in the main Python distribution._

The key word there is \`convention\` which is defined in the Oxford English
dictionary as: _A way in which something is usually done"_. The key word **there**
is _usually_. Consistency is important, but when consistency is not an option you
should use your best judgement. Knowing when to be inconsistent, comes with time and
experience so write code and get feedback.

As Mr. Hettinger says, a lot of people skip this part and jump further into
**PEP8**, but really this is a **_key_** message across **PEP8**, know which battles to
fight and know which sacrifices to make to ensure consistency with PEP8
across your code-base and your project.

See [here](https://www.python.org/dev/peps/pep-0008/#a-foolish-consistency-is-the-hobgoblin-of-little-minds) for yourself for a few cases where sacrificing style guide consistency
might be preferred.

### Code Layout {#code-layout}

<!--list-separator-->

- Indentation

  > _Use 4 spaces for indentation._

  I firmly believe that using 4 spaced indents for your python code has a
  significant impact on the readability of the code. Imagine having a piece of
  code aligned as follows:

  ```python
  def function(parameter):

    result = 0
    for i in parameter:
      if i > 10:
        result+=i
      else:
        continue

    return result
  ```

  I don't know about you, but to me this seems a bit cramped. Refactoring your
  code as follows adds a bit more space and lets the eye clearly align and
  distinguish what level each line is at. And yes, I know you can get indentation
  guides in your fancy editor, but that won't be there for you when u want to
  quickly open this in Notepad or view it on GitHub.

  ```python
  def function(parameter):

      result = 0
      for i in parameter:
          if i > 10:
              result+=i
          else:
              continue

      return result
  ```

  Note that things like the `if statement` and the code for each outcome is quite
  clearly indented and at least to my eye is easier to split out!

  The Indentation chapter of **PEP8** covers some more interesting choices for
  indenting function parameters etc. If you are interested, you can read more
  about it [here](https://www.python.org/dev/peps/pep-0008/#code-lay-out).

<!--list-separator-->

- Tabs or Spaces?

  > _Spaces_

  I think as tedious as it sounds, you should use spaces (ideally 4) for
  indentation. I think to make it simpler, a lot of modern text editors that are
  used in coding as well as full blown [IDE](https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=3&cad=rja&uact=8&ved=2ahUKEwi1keyUr-niAhX%5FQxUIHR41DhUQFjACegQIEhAG&url=https%253A%252F%252Fen.wikipedia.org%252Fwiki%252FIntegrated%5Fdevelopment%5Fenvironment&usg=AOvVaw26G%5FhSQrwphgc0qRbOs%5FUr)'s will convert tabs into spaces for you
  if you ask them to (if not by default).

  The issue is that if you mix tabs and spaces (which might look the same to you,
  but aren't really the same thing to python), you might get a **TabError** and that's
  not something that you really want slowing you down when it is so easily avoidable.

<!--list-separator-->

- Maximum Line Length

  > _PEP8 says 79. I say 90-ish_[^fn:1]

  Hettinger raises a great point (which admittedly is reflected in PEP8 once you
  read it more carefully) that militantly observing 79 character guidelines is
  sometimes isn't very sensible.

  **Indentation in python is important**. So lets say we have a bit of code with a
  function and a few nested structures within the code. Something along the lines
  of:

  ```python
  def useful_function_name(list_of_entries, **kwargs):
      for list_of_subentries in list_of_entries:
          for subentry in list_of_subentries:
              if subentry not in some_global_list:
                  print("{} is not in the global list".format(subentry))
              else:
                  some_global_list.append(another_useful_function(subentry))
      return some_global_list
  ```

  And perhaps you could say that this bit of code is not a realistic example, but
  it serves to illustrate that by having descriptive variable and function names and
  using 4 space indentation makes the 79 character limit approach ever so
  quickly.

  So where does this leave us. Well we could go away and make the names of the
  functions and variables shorter, but we then sacrifice readability for some
  arbitrary number of characters. But guess what? Nobody is going to force you
  (although they could do it through [commit hooks](https://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) and [black](https://pypi.org/project/black/)) to be consistent with
  that number, which gives you the space really to set your own limits for your
  own team.

  Modern monitors and resolutions are wide. We can spare a few more characters
  nowadays! However the key here is to keep the lines not too long and more
  importantly readable. So pick a sensible line length (something like 90
  characters perhaps just to add some space for indentation) and stick to it
  unless you absolutely must deviate.

<!--list-separator-->

- Blank Lines

  I don't really have much to say on this bit of the style guide. I think it lays
  out some sensible rules, but I do want to highlight this line:

  > Use blank lines in functions, sparingly, to indicate logical sections.

  I think this is a really good piece of advice and if you take away something
  from this PEP8 suggestion on blank lines, it should probably be that.

<!--list-separator-->

- Imports

  > - Imports are always put at the top of the file, just after any module comments
  >   and docstrings, and before module globals and constants
  > - Wildcard imports (from <module> import \*) should be avoided

  Those 2 bits of advice I think are key, there are some other things you could
  learn by reading that section, but those are really the key things to follow.

  Imagine yourself opening a piece of code that someone else wrote. Now you scroll
  through it without thinking, take a sip of coffee and start tackling the task of
  understanding what it does. The first thing you want to know is what does it
  depend on? What kind of packages do I need? Well if that person followed
  sensible guidelines, you'd see it all in one place. Neat. On the other hand if
  the imports are scattered everywhere, you need to do the good ol' `CTR+F` to
  actually get what you want and even then it would involve tons of jumping around
  through the file.

  Imports at the top. Always.

  Now the second bit of advice about imports with \* is actually quite important. I
  won't even go into things like namespacing, but suffice it to say that when one
  reads through a codebase and stumbles upon a function that seems to be coming
  from nowhere, usually \* is to blame. And even worse if someone used two or
  more \* imports... well then you're in for a good ol' search online through the
  package documentation (if you're lucky and that exists).

### Pet Peeves and other recommendations {#pet-peeves-and-other-recommendations}

The [pet peeves and other recommendations](https://www.python.org/dev/peps/pep-0008/#pet-peeves) sections in PEP8 are quite sensible. I
would go check them out as they give you a better idea of the 'little things'
that you could do to make your code better.

### Comments {#comments}

> Comments that contradict the code are worse than no comments. Always make a
> priority of keeping the comments up-to-date when the code changes!

Again, the section on comments is actually really good and has a ton of useful
content. However the highlighted bit there is really important. Keep your
comments (and [docstrings](https://www.geeksforgeeks.org/python-docstrings/) and any documentation) up to date!

Avoid inline comments that say what the code does explicitly. If your code looks
like this:

```python
validated_input = validate(some_text)
```

You don't need to comment it. It wastes your time, someone elses time
reading it, computers time, space, time, space-time...

Anyway, if you write your code in a tidy, descriptive way you won't need inline
comments that state what your code does. That said, a very sensible use-case for
an inline comment would be explaining why we are doing something and how it will
impact the outcome down the line. In the previous case there we could say:

```python
validated_input = validate(some_text) # Validate input to avoid code injection
```

### Docstrings {#docstrings}

This deserves its own writeup, but suffice it to say that there is a separate [PEP](https://www.python.org/dev/peps/pep-0257)
that covers how to style those. My advice on this account is: **USE THEM**. Please
go read about them, pick sensible guidelines on how to write them and use them.
Future you and anyone who reads your code will thank you.

I kept writing this post and decided to jump back up to this section and add a
bit more emphasis on docstring. I think once you've written them once or twice
given a chosen standard (I use the [numpy](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example%5Fnumpy.html) standard) it will soon become almost
second nature. And it will **ALWAYS** be easier to write them straight after
you've written the code that they relate to. Because (a) you have all the things
it needs and does fresh in your head and (b) its much harder to not be
complacent with missing documentation when you know that it is code you've
written 3 months ago and it will be a faff figuring out what it does.

### Naming conventions {#naming-conventions}

There is a lot to talk about here. As the [Descriptive: Naming Styles](https://www.python.org/dev/peps/pep-0008/#descriptive-naming-styles) section of
**PEP8** shows, there are a lot of ways to name your functions, variables etc. I
personally use what is called snake case when I write in python meaning that I
link words in my function names with underscores like so:

```language
my_new_value = my_snake_case_function(old_value)
```

In python for the most part, that is the recommended way to name things like
variables and functions. In other languages, like `Rust`, you even get told off
by the compiler for not doing that. That said its not the end of the world to
use different conventions. I use CamelCase in `Haskell` for example as that is
loosely the standard in that language.

But even in python, you really have seperate conventions on what to name how.

Exception names, typing variables and class names make use of upper case and
capitalisation in python. And I think over time I realised that the naming
conventions for the various python objects actually sensible and readable.
The main reason that different types of objects have different styles is to make
it much easier, at a glance, to know something about what you are looking at. As
above, if you meet an object called `Guido` you know that it is a class of some
kind where \`guido\` would instead be a function, a variable or a module.

So not much to add here, go read that section and internalise that.

### Programming Recommendations {#programming-recommendations}

Again, a great section. Well written, gives succinct and useful advice to people.
But more importantly it does a bit more than just tell you how to style your
code. It tells you how to write more _pythonic_ code (which is really sort of
the point of **PEP8** in a way).

It is not a tutorial on how to code in python, but it definitely introduces a
few concepts and how to use them in a sensible way to write more pythonic
looking and functioning code.

## The shifting sands of simple {#the-shifting-sands-of-simple}

This section of the blog is a bit of

What does it mean to be _pythonic_?

Let me give you a few example functions:

```python
def example1():
    index = 0
    for color in ["yellow", "blue", "red"]:
        print(str(index) + " " + color)
        index = index + 1
# ------
def example2():
    for index, color in enumerate(["yellow", "blue", "red"]):
        print(f"{index + 1} {color}")

# ------ for fun :)
def example3():
    list(map(lambda tpl: print(f"{tpl[0]} {tpl[1]}"), enumerate(["yellow","blue","red"])))
```

Let's assume we don't care about efficiency in this function. We don't mind
because it does such a minor thing in the grand scheme of our codebase.
Under that assumption, both these functions will do the same job.

They should **not** fail or have different results unless people use old versions
of python, etc. But just by looking at them I can see which person has used python
more.

Now thats not to say that one is more right than another at this level, but
purely from the way they have written the piece of code its clear that
**example2** is leveraging a lot more of the python in-built tools than
**example1** (if something in there is new to you I hope you go and check it out).

\`So what?\` I can hear you say. Does that change much? Well as I am writing this
these two are equivilent to me, but the second one looks tidier as it leverages
things that are made to solve the required problems (like [enumerate](https://www.python.org/dev/peps/pep-0279/)). This is
what in my head it all boils down to. Over time you learn more about python, its
features and what should feel \`right\` when it comes to code in python. You
become more pythonic which to me means you start leveraging all the tools the
language has to offer in their intended use cases.

But whatever stage you are at, be it python wiz-kid, or just beginning with the
language, try appreciating the perspective of others. Wiz kids, appreciate that
sometime writing things out for others with more clear instructions could help
the target audience and junior devs get their heads around it more clearly
(**example1** is much easier to wrap your head around for someone new to python).

And the newbies, that are just discovering python, accept that some thing written
by people who have been doing it for a while will look much more concise and
sometimes will involve features you don't know about. But don't take this
sitting down, take the time when you can to look at these cryptic functions and
features, understand what **specific use case** they are trying to address and
incorporate them in your codebase.

So to sort of pull this all together, if you know about python and already write
pythonic code, think of who is going to read it and give them time to learn the
features of python. And if you're the **example1** person, take the time when you
see something come up that you haven't seen before to do a quick search online
and see if you can learn more about the language itself.

## Solving things with dirty code {#solving-things-with-dirty-code}

Now this is might be a contraversial section so I will keep it short. If you're
ever opening up a text file and want to get the last few lines or if you're just
opening a csv to save it to Excel, consider not worrying about code style at
all.

Hear me out. Learning how to write python code in a nice and tidy way takes
time. And if you're solving a one-off problem that will never see the light of
day and will sit in the depths of your `D:` drive, do it quick and simple. Dont
worry about variable names, line length, docstrings etc. Just do it and move on.

Over time as you write more and more public facing code you will internalise the
bits of **PEP8** you need and the next time you do the csv to Excel conversion (because
you lost the code in the cold depths of your hard drive), you will likely just
write it in a nice and tidy way without even noticing.

## Conclusion {#conclusion}

Reading back through the blog I realise there is a lot in here. And I really
want to perhaps stress 2 things from all of this:

- read the style guide, be critical and be disciplined
- take opportunities to write more code and learn more about `python` itself
  instead of focusing on line length

[^fn:1]: Well, that's not really true as I stole this guideline from Mr. Hettingers talk.
