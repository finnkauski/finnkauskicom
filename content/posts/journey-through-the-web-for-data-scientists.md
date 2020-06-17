---
title: "Journey through the web for Data Scientists"
author: ["Art"]
date: 2020-02-12T20:00:00+00:00
draft: true
---

## Intro {#intro}

The tempo of these posts is going to escalate as I expect people to keep reading
more about the subject if they want to. This is mainly to keep me motivated to
write the content and for us to finish going through the good bits in good time.

This post will kick us off into this adventure by introducing the concepts of
HTML and CSS in as much depth as you need to know to follow along. I am not a
web developer so things like SCSS, React, Angular and so on are definitely out
of scope.

Anyway let's kick it off.

## HTML {#html}

I won't cover the history of HTML, if you are interested in how it became what
it is now and where it all started read this [great article by Wired](https://www.wired.com/1997/04/a-brief-history-of-html/).

What we are concerned about today is what HTML is, how we can start using it and
where to find more information about. So let us follow that order in our
explanations.

### What is it? {#what-is-it}

Basically, imagine you are writing on a piece of paper something along the lines
of a CV. Now you might want certain bits to stand out so you make the text
larger. In other cases you want to add bullet points so you start adding little
dots as you write down the list and so on.

Now as you write this down, instead of actually drawing (rendering) the text
larger or adding these bullet points, you could just tag each bit of text with a
flag saying: _"Oh, this is a bullet point"_ or _"Enlarge this heading"_.
Essentially instead of putting the work in drawing it out yourself, you are just
tagging bits of text with something that will tell however has to draw it out
(your browser) how to draw (render) the CV.

That fundamentally is whats called a _markup language_. There are many of them,
some less cluttered than others. But HTML is king and here are a few snippets of
it following along with out CV example:

```html
<h1>Jack Torrance</h1>

<h2>Skills and Expertise</h2>
<ul>
  <li>Caretaking</li>
  <li>Axes</li>
  <li>Writting</li>
</ul>
```

`<h1></h1>` and `<h2></h2>` are headings. `h1` is going to be rendered in larger
font and will be useful to make the name stand out. Similarly `h2` will also be
rendered with some emphasis, but it will be smaller than `h1`. You can go right
up to `h6`! Now, why are there two of them for every bit of text? Well as you
can see we need to tell the browser where one tag ends and where another tag
begins. Hence we call the tags that have `/` the closing tags.

Anyway, `ul` stands for unordered list and represents an unordered list, hence
the **ul** tag. `li` represents a list item, which once again is easy to remember
because of the name. Anyway, if you can imagine tables, horizontal parting lines
and so on these and many more are tags that you can put in your HTML. For a full
list of tags check out [this page here](https://developer.mozilla.org/en-US/docs/Web/HTML/Element).

One thing we haven't covered yet is that HTML in principle (as well as other
markup languages) are text. What I mean by that, is that when you look at HTML
you can read it and if you know all the tags you can likely imagine what it will
look like. So lets see what the html code above will look like if I incorporate
it in the blog:

---

<h1>Jack Torrance</h1>

<h2>Skills and Expertise</h2>
<ul>
  <li>Caretaking</li>
  <li>Axes</li>
  <li>Writting</li>
</ul>

---

(note that I've added horizontal lines for separation)

So that is fundamentally what you need to know about HTML (at least
conceptually). If you want to be much more effective in HTML, you will need to
learn more about what tags are available ([here](https://www.imdb.com/title/tt0081505/)) and how to manipulate them.

#### The `<div>` {#the-div}

The one thing I still need to cover is the idea of nesting and the best and most
useful thing to show you in that respect is the `div` element (elements is what
we call parts of the page). The `div` crops up quite a lot and is basically a
content divider. Lets say I wanted to _group_ all the things we wrote about
skills. The best way to start is to _put it in a div_ like so:

```html
<h1>Jack Torrance</h1>

<div>
  <h2>Skills and Expertise</h2>
  <ul>
    <li>Caretaking</li>
    <li>Axes</li>
    <li>Writting</li>
  </ul>
</div>
```

Why is this important? Well when we get to CSS and styling and layout, the
`<div>` will be one of the best tools in our arsenal to style and positions
things in chunks. Conceptually it will not be rendered to anything special, if I
showed you the page that comes out of the above HTML it would look the same as
before, but what changed is the grouping, which allows us to treat the skills
and experience list as one element. One can keep nesting with no issues which
will once again come into play soon.

#### Class and ID {#class-and-id}

So lets say you have two lists on our CV. One could refer to skills and
experience and the other perhaps to the educational background and past studies.
How do we conceptually differentiate these 2 lists?

To name individual elements of the HTML document we can give them a _unique_
name like so:

```html
<ul id="skills">
  ...
</ul>
<ul id="education">
  ...
</ul>
```

You can do this with the `<div>` element as well:

```html
<div id="skills">
  <ul>
    ...
  </ul>
</div>

<div id="education">
  <ul>
    ...
  </ul>
</div>
```

Now we can later (in CSS) refer to them as unique things. But what if we want to
group some elements into without putting them into a `<div>`? Here we stumble
upon the second naming tool - the class.

```html
<div id="skills">
  <ul class="lists">
    ...
  </ul>
</div>

<div id="education">
  <ul class="lists">
    ...
  </ul>
</div>
```

The distinction will become clearer later when we start looking at CSS, but
fundamentally the ids allow us to tag elements with a unique name whereas
classes allow us to group similar elements together.

### Sidetrack: Editing HTML and CSS {#sidetrack-editing-html-and-css}

As HTML (and CSS a we will soon find out) are just text documents, you can open
up anything that can edit text and start writing HTML or CSS. To illustrate lets
open the most basic text editing software you have. If you are on Windows open
up Notepad and copy in the HTML snippet of the CV we had above.

Save the text file as `index.html` (you don't have to name it like that but just
roll with it) and make sure that the operating system you are using hasn't added
things like `.txt` at the end (if it has, rename it to just `index.html`). If
you then open this file in the browser, you will see you HTML rendered on
screen. You can then edit the file, save, refresh the browser and bask in the
glory of the HTML you created.

## CSS {#css}

Cascading Style Sheets. We got the point where we can add the content to the
page that we want by writing it in a text file and tagging bits with certain
tags. The next step is then to edit them to fit our creative vision.

There are several ways to do so and I will briefly fly through them from least
convenient (except for small tweaks) to most convenient (for larger projects).
