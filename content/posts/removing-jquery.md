---
title: "Removing Jquery"
date: 2021-05-02T10:21:05-04:00
draft: false
author: "Thomas Ruggeri"
categories: [post]
tags: ["performance", "rails", "javascript", "front-end"]
---

As I mentioned in my last post, one of my older Rails apps has been out in production for two years now.
When I wrote it back in 2019, I used jQuery and Bootstrap, along with a few other older technologies.
Reviewing it recently, I updated to the latest version of Rails 5, removed CoffeeScript, and did some more
cleaning and updates. As that work came to a close, I lamented the fact that for all the improvements,
the real slow spot in this app is that it still loads the [jQuery](https://jquery.com/) and
[Bootstrap](https://getbootstrap.com/) assets which are relatively large given the simplicity of the front end.

## Motivation

For those of you asking, _why are we doing this_, let me explain the thinking here. There's nothing wrong with
jQuery. Certainly it's a bit old at this point, but it does provide a great developer experience and still
powers a large part of the internet. The real question is _at what cost_? Because jQuery is shipped to the
client's browser, all of the developer benefits come at the cost of slower webpage loads. If you
read the claims made by any framework, they'll tout how small their bundle is once it's gZipped.
That's all good and fine if you're making good use of the resource. In this case, javascript is hardly
being used at all in this application, so it seems like a big waste of 30kB.
Github decided to make this change as well and have [written about it](https://github.blog/2018-09-06-removing-jquery-from-github-frontend/).

The same can be said of Bootstrap - it offers a lot of styling features, but those come at the cost of the
end user having to load all that css. So if we don't _need_ all of those features, the reasoning is that a
light weight style framework will benefit the end user. In a larger app with multiple developers, this is
probably a good tradeoff, but it's a waste here for such a small little app.

## Level Set

Let's get a sense of where we're starting, so we can see how much of an improvement this work will be.

| Asset | Raw (kB) | Notes |
|---|---|---|
| Application.js | 183 | Includes jQuery |
| Application.css | 201 | Includes Bootstrap |

## Adding Picnic CSS

While one could style the site completely by themselves, there are a [number of very light weight styling
frameworks](https://github.com/troxler/awesome-css-frameworks) that can provide a good trade off between size
and features. In the [OAuth tester project](https://oauth-debugger.truggeri.com), I've used
[WaterCSS](https://watercss.kognise.dev/) which is does not use classes. The right choice for this project
is a framework that's small, but offers the features that the site is making use of - grid layout, modals,
and navbar. I decided to give [Picnic CSS](https://picnicss.com/) a try as it fits this bill well.

## Removing Bootstrap

The next phase will be to remove the references to Bootstrap helpers, and swap them for Picnic.
This means swapping out the `row`/`col` classes for `flex`/`full` and making sure the mobile optimization
stays in place. Updating the navbar wasn't too bad, but took a little bit of work for mobile sizing. Finally,
swapping out modals was very easy - the
[Picnic CSS implementation for modals](https://picnicss.com/documentation#modal) is great.

The final phase is to [remove the Bootstrap Gem](https://github.com/truggeri/chore-schedule/commit/dcabe6f65681eb5f4b3adc1d8021d1fa4bb5b1b1#diff-89cade48462044ee1b672dc5f4c3ec250fbd29effcd8932096a23c1283c6731f),
which removes Bootstrap assets from Sprockets. With this, we now have a smaller CSS load, and a somewhat smaller
JS load, but still more work to do.

## Checking on Progress

Let's take a look at the improvements thus far.

| Asset | Compressed (kB) | Raw (kB) | Notes |
|---|---|---|---|
| Application.js | 34 | 103 | Includes jQuery |
| Application.css | 14 | 60 | Just application styles |
| Picnic.css | 7 | 38 | |

So we've seen a decrease of 103 kB in raw CSS and 80 kB of Javascript. Not too shabby.

## Removing jQuery

Now that we don't have Bootstrap (v4.3.1), we can remove jQuery. This requires adapting the existing javascript
files to stop using the `$()` syntax for selectors and remove the usage of `$.ajax`. The first was easy, swapping
in `document.getElementById()`. The latter wasn't too bad either, but it had me re-tool a bit how forms were
being submitted. I did a bit of a funky thing with the ajax submission of a form followed by a js redirect.
Instead of doing this with vanilla js, I simply made the form submits via html which has a similar effect.

## Final Results

After [removing jQuery](https://github.com/truggeri/chore-schedule/commit/7c0de875), we can now take a look at
the end result.

| Asset | Compressed (kB) | Raw (kB) | Notes |
|---|---|---|---|
| Application.js | 5 | 14 | Application script |
| Application.css | 14 | 60 | Application styles |
| Picnic.css | 7 | 38 | |

A savings up of 89kB of javascript with no changes in behavior of the app.

### Total Savings

| Asset | Raw Saved (kB) | % |
|---|---|---|
| Javascript | 169 | 92% |
| CSS | 103 | 51% |

## Cleaning Up

By changing around some helpers, I broke a few of the test cases. I went through and cleaned up those errors.
While doing this, I also updated [Rubocop](https://github.com/rubocop/rubocop) and fixed any outstanding issues.
Finally, I _finally_ enabled CI using [Github Actions](https://github.com/features/actions). When I originally
wrote this app, Actions were still in beta and I never went back and enabled
[Travis CI](https://www.travis-ci.com/) or [CircleCI](https://circleci.com/) on this project.

By enabling linting and tests with coverage, I now bring this project up to my standards. One can see the
green check on each push to `main`.

## What I Learned

This project was fun because it exposed me to a new css framework, and I got to take a look at some of the old
code that I had written in the beginning of my time with Rails. I got to make a few small app improvements while
I was in there, but I did learn not to take [rails-ujs](https://github.com/rails/rails-ujs) out of the
asset pipeline.

## What Comes Next

Next up will be finishing this project off by upgrading from Rails 5 to 6.1. With this upgrade, I will have
access to [ActionView Component](https://github.com/github/view_component), which I am a big fan of.
