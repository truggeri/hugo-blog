---
title: "Using Hugo"
date: 2021-02-03T19:05:05-05:00
draft: false
author: "Thomas Ruggeri"
categories: [post, update]
tags: ["front-end", "jam-stack", "go"]
---

After the experience with tag pages, I finally had some motivation to try another
static site generator - [Hugo](https://gohugo.io/). I chose Hugo because it's

1. One of the post popular options
1. Written in [Go](https://golang.org/) - a language I've been trying to work with more

## Unboxing

I followed the [quick start page](https://gohugo.io/getting-started/quick-start/) and used
[HomeBrew](https://brew.sh/) to install Hugo as a binary, then installed the
[PaperMod theme](https://themes.gohugo.io/hugo-papermod/) after taking a look at the
[gallery](https://themes.gohugo.io/) of options. With that, I was off and running with a site that

* Had a homepage with snippets of pages
* Tag pages under the "taxonomy" feature
* User selectable light or dark mode

That's a pretty sweet set of features. While I knew that build times were fast, I was suprised
how much I like the sub-second build times. It made development and deployment feel like you're
just editing the live HTML.

You can see the first site I created; a
[place to store our favorite recipes](https://recipe-book.truggeri.com).

## Looking Further

Let's discuss a few of the facets of Hugo that I've experienced.

### Hugo and Go

So far I hadn't touched a line of Go. Looking more into the
[templates](https://gohugo.io/templates/introduction/), I see a lot of Go-like syntax
in the templating engine which is kind of inconsiquential - it's not like you're really writing Go.
This is very similar to my experience with [Jekyll](https://jekyllrb.com/) which I was originally
drawn to because it's written in Ruby. While you may end up under the hood writing a plugin or
something in the base language, your day-to-day usage floats above most language constructs and
focuses on the templating language.

### Templates

Now that I've mentioned them, the one thing that I'll note is that unlike in Jekyll, I have not written
much to change the existing template yet. When I did, it was very much my experience that I needed to
completely rip out or copy-and-modify the existing template. It's a bit of a first world problem,
but it would be nice if they were a bit more extendable rather than having to fork the template.

### Configuration

A minor point is that Hugo uses [toml](https://github.com/toml-lang/toml) for configuration.
I'm not a big fan of toml because it often feels like someone took yaml,
changed every bit of the syntax, but didn't make any impactful change. If it were really "obvious",
then why am I always Googling "how to xxx in toml" or "yaml to toml?"
I think the choice here is more of a Go thing, but it's a slight minus in my book.

## Wrapping Up

I reallllly like using Hugo because of the out of the box features and great templates.
The crazy fast build speed is a nice plus, and the small number of files in the repo is also
nice to see. I'm excited to do more and keep the ball rolling. More to come in the future
about the deployment process.

## What I Learned

I learned about lots of the features of Hugo including taxonomies, themes and some configuraiton.
I also leared a bit about Cloudinary while I worked to add web optimized images, but that's a
story for another time.

## What Comes Next

An article describing the deployment process and finishing the migration of the main site from
Jekyll to Hugo.
