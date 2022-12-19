---
draft: false
title:  "Aws Serverless"
date:   2022-12-18 15:20:00 -0800
author: Thomas Ruggeri
categories: [post, update]
tags: [aws, serverless, "cheap-hosting"]
---

Serverless technology isn't something I've covered much in previous articles. As a reader of this blog would recognize,
I love tech that scales to zero because it enables for easy experimentation with little or zero cost. In this article,
I'm going to give an overview of what I've been looking at lately and a few articles I'd like to write in the near
future. This will _not be an authoritative piece, it's meant to be an editorial_ - meaning these are my opinions.

## What is serverless

Serverless is a term for a type of cloud computing. The sentiment is that the developers pay for what they use and
do not have to consider sizing of instances. Of course, there are really servers working behind the scenes. The
naming comes from the fact that the cloud provider (Aws, GCP, Azure, etc) are the ones that are concerned with
the specifics of the servers and the developers only need to concern themselves with application code.

Depending on which cloud provider you're using, there will be different service names that are conical to
serverless. We'll be using Amazon Web Services (Aws) from here on our, so the following are the cornerstone
pieces that you'll see references to frequently.

**Lambda** - The linchpin of nearly all serverless, [Lambda](https://aws.amazon.com/lambda/) is a cloud function service.
There are many available run times or you can compile your own binary. The concept here is each lambda is a simple
piece of code (not necessarily a single function) that does a discrete task.

**Api Gateway** - In order to get ingress from the internet, one needs some type of gateway. In traditional web architectures,
this could be [nginx](https://www.nginx.com/resources/glossary/nginx/) or [Apache httpd)[https://httpd.apache.org/].
In Aws serverless, the [Api Gateway](https://aws.amazon.com/api-gateway/) service offers ingress, routing,
and authorization via http. There is even a websocket offering.

**S3** - Probably the most used service of Aws, [Simple Storage Service (S3)](https://aws.amazon.com/s3/) allows for blob
storage - meaning files. S3 is pretty versatile in that you can store html assets in a bucket, user uploaded or
generate files, or long term storage data in formats like xml, json or [Apache parquet](https://parquet.apache.org/).

**DynamoDB** - Most web apps make use of a database, but historically there are few if any options for a serverless relational
database (Postgres, mysql, etc.). [DynamoDB](https://aws.amazon.com/dynamodb/) is an offering for a non-relational
database. It's unique to Aws, meaning it doesn't work the same as MongoDB or Firebase, but for simplicity you can think
of them as the same type of database.

**Simple Queue and Notification Service** - [SQS](https://aws.amazon.com/sqs/) and [SNS](https://aws.amazon.com/sns/) are the Aws serverless option for queue's and
notifications.

There are many more serverless offerings; a full survey would be lengthy. If you can at least recognize the above,
we should be able to talk about architectures for serverless apps.

## Common criticisms

If you read much about serverless, it won't take you long to find an opinion that is not favorable. While there are
many legitimate shortcomings and considerations, there are often the same tired points that I'll address here.

### There are servers

For anyone that attended a technical lecture, there was always a person that raised their hands and started with the
prase "well actually." The comment that there **are** servers running serverless workloads feels like it's straight
out of the mouth of _that person_. It's a clever fact sparing point that serves no purpose. Congratulations, you got
em. I can't believe no one noticed this before. My mind is blown.... said no one... ever.

Yes, there are servers behind all of these services. The point here is the relationship between the developer and
servers. With serverless, the developer is more concerned with time and invocations rather than server size and
quantity. That's actually a good way to identify something as serverless - is the model based on pure usage or
do you need to have a minimum sized unit that's always running?

### Serverless is expensive

The question of cost in the cloud is at most interesting when it's framed around _value_, not pure dollars. The
discussion should be framed around what you get for the money. In the case of serverless, there are a few cost
advantages over traditional methods.

* Small or very infrequent work loads could end up being cheaper if the cost to "spin up" the necessary services is
more expensive than running the actual workload, or if you are forced to keep a server up and running constantly.
Ex: Keeping a SQL database up 24/7 is expensive and not a good cost per operation if not used frequently.
* Apps that have low usage but may scale up. The classic example of this is a start up that doesn't yet have a large
customer base, but may "blow up" any day. Having to pay for lots of availability when you're not using it is expensive,
and having to scale up quickly may be very expensive as well (having to quickly provision massive instances).
* Having to pay for a service that's not used very much. For example, paying for a message queue that your app
doesn't use it much.

You can see that many of the above examples could be simplified as paying for a resource that you're not using
efficiently.

### Vendor lock in

This is perhaps the most annoying argument around cloud computing in my opinion. The idea that an organization would
pick up and move from one cloud provider to another is absurd. There _is_ something to be said for reducing the
impact of a single provider changing their offerings, pricing or having an outage. I could and maybe will write an
entire article against this point.

The basic summary is yes, there will be a high degree of vendor lock in with these services and that is worth taking
into consideration, but that alone is not a good argument for not using a segment of technology in 99.9% of use cases.

## What comes next?

The purpose of this article is to set the ground work for a set of future articles that relate to serverless. I'd like
to cover the following topics,

* How to write a simple webapp using Aws serverless from the perspective of a Ruby-on-Rails Heroku developer
* A primer on deploying from your local machine using Aws and 
[SAM (Serverless application model)](https://aws.amazon.com/serverless/sam/) CLIs
* How to deploy a javascript framework using Aws serverless
* Using [Aws Application Composer](https://aws.amazon.com/application-composer/) to write serverless infrastructure as
code (IaC)

Look for those articles upcoming.
