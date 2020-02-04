---
title: "Way of learning through the lens of a hobby project (Huemanity)"
author: ["Art"]
date: 2020-02-04T13:18:00+00:00
tags: ["hobby", "rust", "programming"]
categories: ["programming", "rust", "hobby"]
draft: false
---

{{< figure src="https://github.com/finnkauski/huemanity/raw/master/meta/logo.png" >}}

## TLDR; {#tldr}

I've been working on a new project recently which focuses on controlling Philips
Hue lights from the command line. Did it because was not happy with the other
CLI I used before.

It is now out in an early form and needs people to contribute: [get it here](https://github.com/finnkauski/huemanity)

## Why I built it? {#why-i-built-it}

It is written in Rust and it was motivated by mainly 3 things:

1.  I wanted to know how the Hue lights are controlled in my house and what the
    API is like to interact with.
2.  I have been working on another project which links my midi enabled electric
    drumkit with my hue lights (check it out [here](https://finnkauski.com/posts/drue/))
3.  I had a [JavaScript based tool](https://github.com/bahamas10/hue-cli) that I used before. It was slow so I did not
    want to us it for point `2`.

This is fundamentally why I wanted to build it. Self motivation, plus a desire
to create a fun downstream project for which the right library didn't exists.

Now I am going to caveat my previous statement by saying that other Hue lights
`crates` (libraries) exist in Rust, but they seem to be out of date? For
instance [philipshue](https://github.com/Orangenosecom/philipshue) and [hue.rs](https://github.com/kali/hue.rs) haven't had new updates in years. Ultimately even
if they did I wanted more control, better serialisation etc.

## Did I get what I wanted out? {#did-i-get-what-i-wanted-out}

Yes.

1.  I definitely learned how the API works and how it is structured.
2.  Basically [drue](https://finnkauski.com/posts/drue/) worked out ok and I am developing it further and the
    `huemanity` crate is been essential for that to work.
3.  I am slowly moving onto my new `huemanity` command line tool rather than
    using the JavaScript implementation. Just to show you something quite cool
    here are the time comparisons (very rudamentary) for the same command:

<!--listend-->

```nil
❯ time hue lights
  hue lights  0.10s user 0.03s system 87% cpu 0.146 total
❯ time huemanity info
  huemanity info  0.01s user 0.01s system 46% cpu 0.033 total
```

But this is no surprise. Performance in Rust is sort of a selling point
anyways. And to be fair the gap in performance varies depending on what
command you are sending however Rust wins out every time I tried.

## Lessons learned {#lessons-learned}

As I am writings this I realise that this sort of formalised approach to
learning is a great way to push out content and learn about things that might
interest you.

1.  Setting out loose but tangible and reachible learning outcomes
2.  Working towards the delivery of these through tangible projects
3.  Reflecting on what you've learned

I have also finally caved and tapped into the Rust community on `discord` which
was super helpful and I regret not doing it sooner.

And finally I think the key bit here was having a goal or something personal
that affects you. Tactility and this sort of personalisation of your projects
can help you stay motivated. A guy who lives above me has made an app that takes
some of his friends (?) bands vinyl releases and projects augument reality
visuals to supplement the music onto the vinyl while its spinning.

That kind of personalisation of projects really helps you motivated as you can
see the project affect the way you work, live or relax!

## Next post {#next-post}

I keep telling people conflicting messages. But I think I'm set now on the topic
of my next blog post. It will be on writting high performance algorithms in Rust
and using them in Python. It is a topic I know a little about, but theres is more
to learn and gaps to cover up. So this will be both useful for the wider
community and myself.

Cheers!
