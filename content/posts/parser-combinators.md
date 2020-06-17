---
title: "Parser combinators"
author: ["Art"]
date: 2020-06-17T10:36:00+01:00
tags: ["rust", "haskell", "programming", "parsing", "projects"]
categories: ["rust", "haskell", "projects"]
draft: false
---

## Introduction {#introduction}

These are truly weird times. Those who are fortunate enough to have
spare time and not be troubled by the horrors and consequences of the pandemic
are be finding themselves spending a lot more time at home trying to occupy
themselves. What really resonated with me is this [great video](https://www.youtube.com/watch?v=snAhsXyO3Ck) by CGP Grey which
reminded me that we need to get out of this situation better than we went in (if
personal circumstances allow it).

Hobby projects are a great way to learn new skills and I always encourage all
the people I work with to keep building `STUFF` (but thats maybe just me being
[mastery driven](https://www.youtube.com/watch?v=u6XAPnuFjJc)).

This post will cover a recent project of mine on parser combinators which is a
hobby project that allowed me to learn stuff during lockdown.

## Motivation {#motivation}

I spent some time learning Haskell a while back and one of the great pieces of
software written in Haskell is [Pandoc](https://pandoc.org/). Pandoc is a file format converter
allowing you to convert files from one markup to another with ease[^fn:1].

Pandoc (as far as I understand) uses <span class="underline">parser combinators</span> to parse different
formats and convert them between each other. I saw this concept of parser
combinators also pop up in the [haskell book](https://haskellbook.com/) that I used to teach myself the
language. Lastly, it popped up as a video a while back in [Tsoding's video on
writing a JSON parser in 111 lines of haskell code](https://www.youtube.com/watch?v=N9RUqGYuGfw).

I finally caved and decided to write a simple scripting language (if you can
call it that) for scripting my lights[^fn:2]. Check it out [here](https://github.com/finnkauski/lightshow).

Note that I will introduce the concepts, but I don't think I can teach and
provide in depth explanations for them. Mainly because I don't know them well
enough to do it justice. And also do keep in mind that there is probably a
parser combinator library in most languages (and if not, go write it) and you
will have to find that library yourself.

We won't be implementing parsers from scratch, if you are curious and want to
have a bash in implementing this from scratch, here are the 2 resources that I
followed along with that helped me:

1.  [Learning Parser Combinators With Rust](https://bodil.lol/parser-combinators/)
2.  [Tsodings JSON parser from scratch in 111 lines](https://www.youtube.com/watch?v=N9RUqGYuGfw)

## Parsers {#parsers}

Firstly, let's cover what a parser is. In simple words, parser is:

1.  A callable object (usually a function)
2.  The types signature of which would be something along the lines o[^fn:3]f

    ```haskell
       parser :: String -> Either SomeStructure ParsingError
    ```

    Essentially it takes in some string data (or a stream of bytes or whatever
    else you choose to represent your text in) and spits out either a structure
    build from that text or an error if it fails to parse it.

So the key concept here is that a parser can **fail** or it can return a parsed
object that you are after alongside whatever else it didn't parse.
To clarify that last bit, often parsers return not only the structure that they
parse, but also the rest of the input that was left after they extracted that
structure.

For example if our parser `alphanumeric1` is able to parse a arbitrary length
alphanumeric string of length 1 or more, and then we apply it to a string we
might get something along the lines of the following:

```haskell
alphanumeric1 :: String -> Either (RestOfInput, ParsedStructure) ParsingError
alphanumeric1 = ...

inputString = "Hello how are you doing?"

alphanumeric inputString
-- Output: Left (" how are you doing?", "Hello")
-- Note: our parser in this example doesn't handle spaces
```

Other common parsers could be something along the lines of `multispace0` which
could parse 0 or more of anything that we consider as a space (tabs, newline,
regular spaces). Or a parser that parses a specific `tag` of our choice for
example `{` or `}`.

Thats neat, but on its own these parsers are very small and useless. This is
where the concepts of <span class="underline">parser combinators</span> come into play.

## Parser combinators {#parser-combinators}

Firstly, lets set out the scene where we will use combinators and then it will
be easier to explain what they are and why we need them but in short a
combinator is a way to combine multiple parsers into a larger parser.

Also let us move away from Haskell and its beautifully clean syntax and start
looking at Rust which is what I used to implement my small scripting language.

Now for our case lets first check out the syntax for the language I was
implementing.

```nil
c: seq = {
    color ff0000;
    wait 1;
    blink 2 1 ff0000;
    wait 1;
    color 0000ff;
};
a: act = color 00ff00;
trigger c;
trigger a;
```

Without going into detail, I wanted to have the following:

1.  a set of actions that the lights could perform - blink, change color, wait etc.
2.  a way to store sequences of actions - the `c: seq`
3.  a way to store single actions - the `a: act`
4.  a way trigger these stored actions

Now perhaps its not the most comprehensive and turing complete language, but it
served its purpose of being a lighthearted application for learning how to do
parser combinators.

Back to our parsers! Firstly lets explore how I parse a single command e.g.
`color ff0000;`. Deconstructing its constituent parts we get:

- the word: `color`
- followed by one of more spaces
- a color string which is the main bit we want
- followed by a colon
- followed by optional newline or spaces

[nom](https://crates.io/crates/nom) - a Rust library that provides speedy parser combinators has quite a few
parsers already implemented. The `tag` parser can be used to parse the word
'color', the `hex_digit1` parser will help us deal with a string of hex values
for our color string, `space1` will help us parse 1 or more spaces and finally
we'll use `tag` again to deal with the colon. We will skip the newline for now.

Lets look at how the simplified parser implementation for color looks like and
decompose it into both the parsers we described above and the combinators we use.

```rust
fn color(i: &str) -> IResult<&str, &str> {
    let identifier = preceded(tag("color"), space1);
    let color_string = preceded(identifier, hex_digit1);
    let semicolon = terminated(tag(";"), multispace0);

    terminated(color_string, semicolon)(i)
}
```

Firstly, the input to the function is a string and the output is a type of
`Result`. A Result type in Rust basically tells you - something can return a
value or a failure state. And actually coming back to the Haskell snippets
mentioned previously - `IResult<&str, &str>` is equivilent to `Either (String, String) Error`. The type signature out of the way, we move onto the actual body
of the function.

Lets first look at the `terminated` and `preceded` parser combinators. The ones
we see here are very similar and are basically inverted versions of each other.
The `preceded` combinator takes in two parsers and <span class="underline">combines</span> them into one
parser where the the second parser is **preceeded** by the first one. Using this
knowledge we can now see that `identifier` is a parser that will return 1 or
more spaces only if they were preceeded by the word color.

To clarify, this parser will return a parsed input of 1 or more spaces only if
its preceeding parser `tag("color")` **<span class="underline">succeeds</span>**. Note that we're not really
interested in parsing the spaces themselves and what we're really trying to do
is to make a parser that fail when the pattern we are looking for isn't there
and its results are irrelevant to us in this case. So if one of the parsers
within the combinator fails, the whole combined parser fails.

Moving onto the `color_string` parser we can see it is quite similar. However
notice that now we are saying - get me a string of hexidecimal digits of length
1 or more that are preceeded by the `identifier`. So this parser first looks for
spaces preceeded by the word color and if that succeeds, it will look for the
hex string for the color. In contrast to the previous parser where we really
didn't care for the output of the parser (the 1 or more spaces), we actually
want the color output from this parser if it succeeds. Finally the `semicolon`
parser is a parser that takes the tag parser for the semicolon and then only
returns a result if the `multispace0` finds 0 or more space-like string after
the semicolon (hello optional newline!). Bus similarly to before, we aren't
interested in the semicolon itself, we will use this parser succeeding as an
indicator that something is followed by a semicolor and optional spaces.

Lastly we move onto the last combinator. Here we use the `color_string` parser
which will give us the string value of the color and say that it has to be
followed by a successful parsing of the semicolor parser. Also not that we're
finally passing the input string to this parser - `(i)` and returning the result
(in Rust no colon in the last statement of a function returns that value).

So in summary, we built a bunch of smaller parsers for which we mostly didn't
care if they returned some parsed value and we just cared if they succeeded in
matching bits of the string that they are designed to do. Then we build a
parsers that actually got us what we wanted, namely the hex string and then we
build a larger parser by combining these smaller bits.

We could extract the constituent parsers into their own functions and reuse
them! We can also test these smaller parsers individually to ensure quality.
Furthermore there are tons of other useful combintors for example `many1` which
basically takes a parser and keeps trying to reuse this parser until it fails
and return the results in a vector. So if you had multiple commands in a new
line and you wrote a generic command parser, you could use this many combinator
to suddenly be able to parse `n` number of these commands. Ultimately the whole
parser for my language and all of its bits and bobs turned out to look like
this:

```rust
/// Root parser for whole documents
pub fn root(i: &str) -> IResult<&str, Entities> {
    all_consuming(many0(terminated(
        alt((trigger, sequence, action)),
        many0(newline),
    )))(i)
}
```

Lets briefly run through this! So firstly we have the `all_consuming` combinator
which fails the whole parsing process if there are bits of the input left over
that haven't been parsed (meaning we're missing the ability to parse something).

Then we have have a `many0` (again very similar to like regex quantifiers) which
looks for a `many0(newline)` terminated lines containing one of the following
parsers (the `alt` parser is basically an `one of` across parsers):

- `trigger` - the trigger statement parser
- `sequence` - the parser to parse the sequence structure
- `action` - a standalone assigned action

So in the end it will return this `Entities` type, which I won't go into but it
is a collection of outputs from this parser all stacked together in a vector.
Which then my interpreter can deal with by looking at what each entity is
(statement, sequence assignment, action assignment).

## Conclusion {#conclusion}

Hopefully my write up of this inspires the more curious people to try this out
for themselves. In general writting a scripting language like this was quite a
fun experience. Ultimately I think what I enjoyed from this project is the
process of being aware of a technology and then finding a good use case to
practice this tech in some form of fun but educational project.

[^fn:1]: I wrote this blog in org-mode markdown and then there was some intermediary process that converted it to html. This is the type of stuff pandoc is really good at.
[^fn:2]: I had written a simple API wrapper and CLI for Philips Hue lights that this scripting language uses. See more: <https://github.com/finnkauski/lighthouse>
[^fn:3]: For demonstration purposes I use haskell as its a very clean looking language to start with. Later I will move onto Rust, but the same concepts translate.
