---
draft: false
title:  "Static Sites"
date:   2019-12-19 20:58:00 -0500
author: "Thomas Ruggeri"
categories: ["interesting"]
tags: ["front-end", "jam-stack", "cheap-hosting"]
---
I've been wanting to get this developer blog going for a while. Lots of things get in the way, work, home life, pets,
family, other side projects. But I'm happy to make this first post! Why do I call this a _developer_ blog?
I make the distinction because posts here will focus on developer, software engineering topics. It's meant to
remove the burden of doing a full [Dev.to](https://www.dev.to) or [Medium](https://www.medium.com) post 
that may require more time and research. This is meant to be more fast, rough and just for the knowledge.

So with all that said, this is the first post. What better topic than the very technology that this blog is written
with - [Jekyll](https://jekyllrb.com/). This static site is made using Jekyll and hosted on 
[Github pages](https://pages.github.com/). Let's break it down piece by piece.

## Static Sites and the JAM stack

I've recently been researching more on the [JAM stack (Javascript, Api's, Markup)](https://jamstack.org/). 
I think I first found an avenue into this topic from 
[a conference video on YouTube](https://www.youtube.com/watch?v=QeNLdiTHy4M). That got me into the idea which I 
followed until I landed on Jekyll. I found this concept interesting for a few reasons.

### 1. Static sites are inherently more simple than "dynamic" ones

A static site doesn't have the runtime dependencies that a "dynamic" one would - such as the Ruby, .NET or node runtime.
While it does build, a static site won't be subject to nearly the number of runtime errors, logs
and statistics that traditional apps are. In software, as is also the case in much of life, simpler is better.

### 2. Deploying static sites is easy

Serving static files, that is html, css and javascript, is much easier than running a traditional app. Put the files in
a service for files and call it good. No need to deal with package managers, run times, and compute resources.

### 3. Static sites can be cheaper

Because these services are simpler, they're often cheaper. One doesn't need to pay for constant compute resources
along with the accompanying metrics host and log aggregator, etc. Just throw them in an 
[Aws S3 bucket](https://aws.amazon.com/s3/pricing/) or use another
service ([Netlify](https://www.netlify.com/pricing/) for example) which are very affordable.

### 4. Moves much of the complexity out

Some common tasks of apps are well known and have good "understood" solutions. Think of authorization, image conversion,
etc. Static sites make use of existing api's that will perform these duties. This is the real ["service oriented
architecture" pattern](https://martinfowler.com/articles/microservices.html) that is in vogue at the moment.
That is to say, encapsulate functionality and provide well defined interfaces.

I could go on and on, but the takeaway is that I've found this new stack very interesting. Surprising considering I'm 
a backend engineer and the JAM stack really aims to abstract away a lot of that work. For another great overview 
of the JAM stack, check out [this conference video](https://www.youtube.com/watch?v=dqqMFpR7sMk).

## Jekyll

What is [Jekyll](https://jekyllrb.com/) and how does it fit into all of this? It's a static site generator, 
written with [Ruby](https://www.ruby-lang.org/en/). My current job at ZipRecruiter is primarily Ruby, 
and I've really taken to the language. I'll probably write another post on this at some point.
It was [originally written by one of the Github co-founders](https://en.wikipedia.org/wiki/Jekyll_(software)#History).

There's nothing per se that is unique about Jekyll. It offers the ability to write or use a theme and then quickly
create pages, like this one, using only [Markdown](https://en.wikipedia.org/wiki/Markdown) - another 
of my favorite technologies. This makes it super easy to quickly create pages using just Gitops. Any setup is done
using Ruby which I have no problem with.

## Github Pages

The last piece of this puzzle is [Github pages](https://pages.github.com/). This service is a great resource 
that Github offers. Essentially, using a Github provided gem and a few clicks in the settings of a repository 
creates a free hosted site. How can you argue with free? Since I'm already using Github to host the code, it's
a no-brainer to use it for hosting too.

## What did I learn?

I learned that the JAM stack is on the rise with older tools like [Jekyll](https://jekyllrb.com/) and [
Hugo](https://gohugo.io/) along with exciting new comers like [Gatsby.js](https://www.gatsbyjs.org/).
Their are also some really interesting hosting services like [Netlify](https://www.netlify.com/)
and [Firebase](https://firebase.google.com/). Even though a backend dev could write these off as nothing but fluff,
I think they really could be a big piece of the future of app development. It may just be that their will be more
of a divide between the front end and back end services.

## What's next?

I'm not sure about that. I think perhaps a deeper investigation into some of the app tools like
[Firebase](https://firebase.google.com/) and [Aws Amplify](https://aws.amazon.com/amplify/)

