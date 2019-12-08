---
title: "Python climate change"
date: "2019-11-05"
author: "Art"
cover: "img/green_python.jpg"
tags: ["pipenv", "programming", "python"]
keywords: ["pipenv", "learning", "programming", "python"]
description: "Exploring python environment management"
showFullContent: false
---

# The Setup

First of climate change is an insanely scary issue for everyone and should _not_
be neglected! _However_, I want to talk about a different environment, a
`python` environment.

I recently got asked to talk about my environment management when I work in
python. However if that person had read my first post on this page they would
have noticed:

> "Pipenv: The Future of Python Dependency Management" - A splendid talk about
> how not to have a breakdown trying to run your python code on other peoples
> computers (aka sharing projects and proper dependency management)

I still recommend going there and watching the whole talk about it, however I
will expand on what _I use at the moment_.

Now the reason I say at the moment is because the landscape for this stuff will
change and has been changing, however I think we've reached a point where
environment management for python couldn't be easier if you spend 10 minutes
setting things up **right**.

<small>(if you're hardcore you can skip this and use nix)</small>

At the moment I will use a Unix like system as an example so Mac users and Linux
users shouldn't have any issues. However you are likely to find the same tools
on Windows that work in the same exact way.

I will also assume you know a bit of python, meaning I can throw around terms
like `pip` and not scare you off.

And finally I think the goal of this post is just to consolidate the use of
existing tools into a workflow that I use. So if you want to learn more about
the bells and whistles of the tools themselves or find alternatives, you _will_
have to do it yourselves.

# The Performance

## pyenv

Ok, so you got your new computer, first things first you need python. Likely
Unix systems come with their own version of python. Now this python is there to
make sure that the system itself can run things it needs to run in python.
Meaning that if it has python scripts it needs to execute, this is what it would
use to run.

You don't want to mess with that.

The easiest way to get python and be able to switch versions on the fly and
update it super simply, is to use [pyenv](https://github.com/pyenv/pyenv). Make
sure to look up the very neat
[pyenv-installer](https://github.com/pyenv/pyenv-installer).

What this will allow you to do is to:

- install any version of python you want (including things like Iron-Python
  and Anaconda) by simply typing `pyenv install 3.9-dev` or `pyenv install list`
  to see all the versions
- set a global version of python whenever you choose by running `pyenv global 3.7.6`
- set local versions of python per folder! which is great for example if
  you've gotten some old code from a colleague and you need to run it as a one
  off and don't want to replace all those pesky print statements.

This. Is. Magical. Trust me. The ability to jump around is useful to no end and
recently I used it when presenting the python gotchas to my colleagues as some
of the cool mind-bending tricks no longer worked in 3.7.

## pipenv

Now. Once you've sorted your python and you can run stuff, the next step is to
actually figure out how to install packages. Whichever python version you
install through `pyenv` will come with `pip` - the python package manager.

Now you might get tempted to start installing tools and packages that you want
to use straight away through `pip`. The issue with that is that you're
installing everything to a global bucket.

**Warning: Incoming horrible analogy!**

Imagine [Waterworld](https://en.wikipedia.org/wiki/Waterworld) the 1995, Kevin
Costner movie about the whole world engulfed in water.

Lets say you want to drink some water in that world, and instead of pouring
fresh water into a cup and drinking from there, you pour it into the ocean then
drink from the ocean. Now that's not pleasant as it gets mixed with all sorts of
stuff. Don't get me wrong, its still drinkable in theory, but in practice it
will likely make you ill.

When you install a package without containing it, in whats called a virtual
environment, you're essentially pouring yourself some fresh water to drink by
dumping it into the ocean instead of a cup. `pip install PACKAGE` will by
default dump any packages into one massive, system-wide location. Which isn't
great when someone else asks you to pass a cup of water as you can't unpick what
you installed from what was already there from previous installations. In other
words, telling someone what exactly they need from your massive set of packages
to run your scripts is hard.

Virtual environment tools allow you to containerise your installations in a way
that lets you then share your work in a reproducible manner. I recommend at this
point checking out `venv` in python and coming back to this post afterwards as most
of the other tools are just nice little wrappers and quality of life
improvements around that basic tool.

As I said, most tools just let you use `venv` in a more concise and easily
accessible manner. Recently I have had my eye on `poetry` which has some nice
little features, but as you saw from the title to this section we'll be focusing
on `pipenv`.

Now `pipenv` is the recommended python dependency management tool as per the
python documentation (at the time of writing). And I think for good reason. It
has a few great features that I personally think make it great for usage. But if
you really boil it down it is just a nice little wrapper around virtual
environments and `pip` in one package.

Let me show you a basic workflow with this fine tool.

### The workflow

Lets say you are starting a new project in a folder called `ML_dev`. Now you
want to be able to share this project in the future with other people so they
can run it.

The first thing to do is to create the folder and `cd` into it. In the folder
you then can type in `pipenv install SPACE_SEPARATED_LIST_OF_PACKAGES` this will
create a new virtual environment **linked to the folder name and location**.

You now have a virtual environment somewhere (you don't need to know where the
files for it live as pipenv will handle it for you). This virtual environment,
once activated, will contain all your newly installed packages, assuming you
install them within the virtual environment.

So in order for you to abstract yourself from your global set of packages and
live in your cosy new world, you need to run `pipenv shell` this will shove you
into a world of pure imagination. Where you no longer have access to the global
packages you had before, but you will have access to the ones you installed in
the previous command and will have the ability to `pipenv install` new ones
without tainting your ocean. Note, please don't use `pip install` in your virtual
environments and the more tech savvy devs check out the `--dev` option for pipenv
;)

For the sake of completeness I will also briefly cover of how pipenv actually
keeps track of all this stuff. When you create a virtual environment with
`pipenv`, you will create a nice little file called `Pipfile` which will keep
track of all the key bits and pieces about the environment. This is the file
(it and its sister file `Pipfile.lock`) are the files that you need to ship to
someone else for them to recreate their environment. The `Pipfile.lock` file is
a file that gets created and update whenever `pipenv` is **sure** that all the
dependencies that are installed in the environment can be resolved and will work
nicely. So if you ever see a `locking failed` when you install a new package,
this means that the package was installed (it will appear in the `Pipfile`), but
it might have issues working with other packages or might need newer versions of
a given package that other packages might not work with. Anyway, here is an
example `Pipfile`:

```yaml
[[source]]
name = "pypi"
url = "https://pypi.org/simple"
verify_ssl = true

[dev-packages]

[packages]
numpy = "*"
scipy = "*"
pandas = "*"
pytest = "*"
hypothesis = "*"

[requires]
python_version = "3.7"
```

I suppose I should highlight the things I like about this tool and the workflow
it offers.

1. No need to write a bunch of shell commands to jump into a virtual
   environment. Just go to the folder and type `pipenv shell`
2. The reason why I insisted on you using `pipenv install` rather than the `pip`
   equivalent is because `pipenv` resolves dependencies. This means that it will
   try to `lock` the packages after each install and if that success this means
   that all packages can work in harmony together. Similar to what `conda`
   promises.
3. Vulnerability detection. You can run `pipenv check` and it will check for security
   vulnerabilities as part of that command against known ones published online.
4. Perhaps one of the things I like the most is that `pipenv` works with `pyenv`
   and allows you to set **PER PROJECT** python versions by using `--python=3.8`
   like commands when you initialise an environment
5. The last feature is something I will dedicate the next section to, so read on.

### Dotenv

So let us suppose that you are developing some form of online website in python.
Now, you will/might stumble upon an issue fairly quickly of how to make sure
that what you have developed on your local machine can be easily deployed on a
virtual machine somewhere online to sit there until the **HEAT DEATH OF THE
UNIVERSE**.

"What?" I hear you say. Well imagine a situation where you are developing a
website and it needs a database URL. Or lets say you have some API key
that you don't want to include in your code as someone with access to your code
on github can see the key and start using it as their own.

In that case what you might want to consider is environment variables. Your
shell and computer in general has variables that can be set for the whole system
and you can access those variables through python or any other languages. So if
you set a variable (how you do this will depend on the Operating System)
**API_KEY=001123abc0987asd** and another one **DATABASE_URL=.../.../...** you
can then access these using the `os` package in python.

Hence you haven't hard-coded these in your code anywhere and no malicious people
can start using any of this for their own purposes.

Now, setting up these things is annoying in some cases. So there are tools like
`dotenv` which allow you to load in these variables from a file, usually called
a `.env` file. So in practice you could make a `.env`
file, dump the aforementioned variables into it and load it in every time your
code runs programmatically through the `dotenv`library for your language and you're
set to go. But `pipenv` (and other similar tools for other languages) started
including this `dotenv` stuff from the get go. Meaning that if you have a file
called `.env` in your project directory and your jump into a `pipenv` shell or
use `pipenv run`, it automatically picks up these things into your environment.
This allows you to not hard-code in the existence of such a file and the usage
of the `dotenv` package in your code and you can just use `os` to pick up these
files assuming that they will be there. And once you deploy this to some other
system, you will just have to make sure these variables exist in their environment.

# The Prestige

Anywho, that's about the end of the post. To summarise, use `pyenv` to not mess up your
python installations and `pipenv` to manage your packages in a nice and tidy
way.

Read up more on it online, check out the link in the beginning of this blog
or if you're unlucky enough to know me in person, come ask me more about it.
