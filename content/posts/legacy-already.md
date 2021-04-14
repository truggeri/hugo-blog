---
title: "Legacy Already"
date: 2021-04-14T09:33:15-04:00
draft: false
author: "Thomas Ruggeri"
categories: [post]
tags: ["ruby", "rails", "maintenance", "front-end"]
---

The other night I was looking over an old app of mine, the [Chore Scheduler](https://choreplan.truggeri.com/).
It started as a desire to rip out references to unused Rails engines, such as ActionCable and ActionMailer.
Since these are not used by the app, but were included by the basic `rails new` command. Off we go on another adventure. You can take a look at the [repo](https://github.com/truggeri/chore-schedule) as it's open on Github.

## Runtime Dependencies

I started out by ripping out the `require rails/all` in my `application.rb`. This `all` file requires a bunch of
pieces, such as ActiveRecord, ActiveStorage, ActionCable, etc. So by removing it, and manually adding each engine
that I actually want to use, I can reduce the amount of code loaded at runtime. Not a _huge_ win, but why not?

Because I ripped out some engines, I can also remove their config. This is pretty simple to do, just look for lines
in the environment config files that make reference to one of these. If all else fails, the app will throw an
exception on boot if you miss one.

## Upgrading Gems

I also took this as an opportunity to do some pruning of my Gemfile. I upgraded to the last release of Rails 5,
and took a hard look at some of the older and unused gems that I included - again, from all the way back when
the `rails new` command was first run. I removed Turbolinks as it wasn't being used, and upgraded Bootsnap
and the Scss gem when I noticed a deprecation warning. But while I was in there, I noticed an old wart.

## Removing CoffeeScript

[CoffeeScript](https://coffeescript.org/) is a pre-processor for Javascript that makes it more palatable,
especially for Rubiests. It had its hay day back in the early 2010s, when Rails was at its peak. Looking
at it in 2021 though... there's not a good reason to keep this tool around. Especially in this repo,
where there is very little Javascript, it feels like almost purely tech debt.

It's easy enough to remove, as theres [a website that will convert between the two](http://js2.coffee/).
Copying the files name and using the `js` extension, sprockets will automatically load them. Easy-peasy.

Well, technically I had an issue with my asset build as it was cached in Heroku. A simple lookup made
it easy to [dump my Heroku build cache](https://help.heroku.com/18PI5RSY/how-do-i-clear-the-build-cache) with,

```bash
heroku plugins:install heroku-builds
heroku builds:cache:purge -a appname
```

Once that was done, we're good and no longer using old pre-processors.

## Looking at the Results

With all that done, I sat back and looked at what I had done. The site functions the exact same, but
in theory the memory footprint should be lower. I cannot confirm that because metrics are disabled on
the free tier. Nothing else has substantially changed.

Well, I popped open the network tab to take a peek at what was going on. That's when reality set in.
This app uses jQuery and Bootstrap. That was a decision I made when I first started the app. I was trying
to write something quickly and I use jQuery and Bootstrap at work, so the choice seemed obvious.

Looking at it now, this is the apps biggest performance gains can be made by replacing these libraries.
The uncompressed size of the two are 201KB for css and 183KB for javascript. That's nuts for such a simple
app that has next to no dynamic behavior and about five total pages.

The issue is that removing these dependencies without changing the apps behavior is a much bigger job.
Mostly because of the integration of the Bootstrap styling, it will take some work to unwind this decision.
So for now, we have made some small improvements, but big ones will be for another day.

I'm mainly surprised at how quickly this app has become a "legacy" app. I guess the second you stop active
development, the legacy tag goes on.
