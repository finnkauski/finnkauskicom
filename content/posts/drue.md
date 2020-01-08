---
title: "Drue"
date: 2019-12-27T20:59:38Z
author: "Art"
cover: "img/porridge.jpg"
tags: ["rust", "drums", "programming"]
keywords: ["drums", "rust", "learning", "programming"]
description: "So I made a thing!"
showFullContent: false
---

# drue

This is a showcase of a bit of software I've been writing.
I am a newbie drummer and wanted to spice up some practice with a light show so
this links the lights in my room (Philips HUE bulbs) with my drumkit.

I haven't opened the repository for the codebase yet. But if there is demand I
will tidy it up and put it on github.

## Demo:

{{< youtube fEK2DofSwEE >}}

## A few things to note

I only show you 2 'modes' that I've implemented:

1. Change color - Reactive to your hits-per-minute (HPM):
   - orange when idle,
   - 200+ HPM and you get blue
   - 600+ HPM and you get red
   - NOTE: these are sampled every 0.7 seconds and your HPM is estimated from that.
2. Blink - Reactive to a certain drum being hit:
   - In the second shorter clip, the lights blink every time I hit the left cymbal

There is another mode called variety which changes the lights the more different
drums you hit per second. And actually I have Ideas for more of them.

## So what now?

Where is this going then? Well if there is interest I need more people to try
the APP and test it on other drumkits. I also need feature requests and ideas as
well as contributors to the code (written in Rust).

Anyway,
Peace out!
