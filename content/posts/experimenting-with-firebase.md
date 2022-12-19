---
draft: false
title:  "Experimenting with Firebase"
date:   2019-12-29 22:27:00 -0500
author: Thomas Ruggeri
categories: ["interesting"]
tags: ["front-end", "nosql", "cheap-hosting", "javascript"]
---

As I've hinted in [a previous posts]({{< ref "first-post" >}}), 
I've been experimenting with [Firebase](https://firebase.google.com). 
While I'm no expert, I have learned a few things that I'd like to share and document.

## Firebase

What is Firebase? It's a unique hosting platform that is owned by Google and runs on the 
[Google Cloud](https://cloud.google.com). What's unique about it? In overly simple terms, 
it's a tool set for offering a backend as a service. You write an app (javascript, iOS or Android) and use the 
[Firebase SDK](https://firebase.google.com/docs/reference/js/) to perform the backend tasks such as 
[authentication](https://firebase.google.com/products/auth/) and 
[data persistance](https://firebase.google.com/products/firestore/). The marketing makes the case for this 
to be a great platform for small applications to get off the ground and then it can continue to grow. 
There're also many more features and services around A/B testing, analytics and more.

## Why Am I Using Firebase?

As a "backend" engineer it may seem strange that I would spend time looking this product that aims to simplify and 
abstract the backend. So why would I spend my time on it?

### Cloud Firestore

The [NoSQL database](https://en.wikipedia.org/wiki/NoSQL) that Google offers with Firebase is called 
[Cloud Firestore](https://firebase.google.com/products/firestore/). It's a NoSQL database with similarities to 
[MongoDB](https://www.mongodb.com/), Aws DynamoDB and CouchDB - all technologies I've used in the past. 
I view Firestore as a very interesting database that takes a few new approaches.

**Index everything** - [Every field you add is indexed](https://firebase.google.com/docs/firestore/query-data/index-overview#an_index_behind_every_query), so reads are "always fast" at the expense of writes.

**No partial documents** - Unlike a `SELECT x FROM` sql statement, or a [projection in MongoDB](https://docs.mongodb.com/manual/tutorial/project-fields-from-query-results/), you must retrieve an entire document, not just a select number of fields.

**Querying across fields requires a composite index** - Despite having an index behind each field, you must create a [composite index](https://firebase.google.com/docs/firestore/query-data/index-overview#composite_indexes) for queries that involve multip fields. This keeps the statement "an index for every query" true.

I've worked with NoSQL databases before, mostly MongoDB at Snagajob, and I enjoy changing my view of the world. 
When you work with [Ruby on Rails](https://guides.rubyonrails.org/v2.3/getting_started.html#configuring-a-database), 
your thinking aligns to relational data. I'm trying to avoid letting my skill set for NoSQL wilt by continuing to 
experiment with other technologies.

So far I've enjoyed my time with Cloud Firestore. It's a little different for me, but I can appreciate why they 
make some of the choices that they did. For example, don't think about if a query will be slow, 
just ensure a query must be indexed. This may not be the best thing for each and every usecase, 
but for many it makes sense and it helps get small projects off the ground quickly.

### Other Services

Another reason to use Firebase for personal projects is that it offers a number of other services included in the SDK.
For example, I've made use of authentication through Google. So without managing another account for something
like [Auth0](https://www.auth0.com), I can setup a login via Google in 60 seconds. Hard to beat that when you're
working on a personal project.

There are a number of additional features that I have yet to take advantage of, but hope to going forward.
One is the use of Cloud Functions which is great because you can write code in many languages without investing
an entire project in it. I'm hoping to write some services in [Golang](https://golang.org/) at some point in 2020.

### Cost of Development

The more of these posts you read, the more you find that I like to find free hosting tools. Firebase is great for this
as it offers a completely free tier. I love that because I can use the services and experiment with a very small
number of queries and a small data set.

## Front End

Did I mention the front end stack that I'm using for this? I will get to that in another post. :D

## What's next?

More development and more learnings from it. I need to put together a small page design and make some more 
progress on the basics of the app.