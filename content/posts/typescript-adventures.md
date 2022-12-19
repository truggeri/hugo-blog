---
draft: false
title:  "Adventures with Typescript"
date:   2020-12-30 14:05:00 -0500
author: Thomas Ruggeri
categories: [post, update]
tags: [front-end, javascript, typescript, golang]
---

The latest adventure I've been on is one with [Typescript](https://www.typescriptlang.org). The
technology was developed by Microsoft as a way to add typing to Javascript. It's popularity has
been [increasing over the past few years](https://visualstudiomagazine.com/articles/2020/03/04/redmonk-feb20.aspx).
I had some experience with it working on Angular 2 projects during my time at Snag. Having gone
from that environment to my current job and projects written with more pure Javascript, it's been
very interesting to come back to it and have the experience anew.

While working on a new little project (more on that later ...maybe), I realized it was a good time
to integrate Typescript, even though it's not necessary.

## Installation

Let's just start with the nuts and bolts of adding it. In your `webpack.config.js`:

```javascript
module.exports = {
  entry: {
    bundle: ["./src/main.js"]
  },
  resolve: {
    extensions: [".tsx", ".ts", ".mjs", ".js"],
  },
  module: {
    rules: [
      {
        test: /\.tsx?$/,
        use: "ts-loader",
        exclude: /node_modules/,
      }
    ]
  }
};
```

Don't forget to install the node package,

```bash
npm install --save typescript
```

That should be it. Renaming a plain Javascript file from `.js` to `.ts` will make it Typescript.
Because Typescript is a superset of Javascript, it makes for an easy to upgrade from existing
code to Typescript.

## Why would I use Typescript?

Typescript is useful because it helps you put some bumpers around Javascript. Adding types greatly
reduces the testing space and can help provide some contracts around our component edges. I
particularly like this feature because it ensures I don't send a number into a function that needs
and array of objects. Easy checks that nothing disastrous is going to happen.

There is also some nice tooling that integrates with a language server which makes working with
your code a bit easier. Between inference and libraries that come with Typescript support,
you can get some great intellisense in your editor.

## Similarites to another langauge

While writing some little scripts with my new setup, I noticed something. The more that I
looked at the functions I was writing, the more they looked like
[Golang](https://tour.golang.org/list) to me. I know it seems odd to compare the two languages,
but let's take a look at an example.

For an example let's look at a method that adds a given set of numbers.

```go
func valueExample(input []int) int {
  result := 0
  for _, num := range input {
    result = result + num
  }
  return result
}
```

```typescript
function valueExample(input: number[]): number {
  let result = 0;
  input.forEach((num) => {
    result += num;
  });
  return result;
}
```

One can see here a basic similarity between the structure. Sure, some of the keywords are different,
but if you were reading the code you can see that they're very similar. I think having the type
_after_ the variable really stands out. I know it took me a long time to get used to that in Golang.
You can also also see some similarities between some of the for loops, especially if you use the
traditional syntax,

```go
for i :=0; i < len(input); i++ {
  ...
}
```

This isn't really here nor there, but I found it interesting, especially considering how different
I think of Go and Javascript.

## What I Learned

I learned a few things about the
[Typescript language](https://www.typescriptlang.org/docs/handbook/intro.html), such as `enum`,
`readonly` and [differences in closures between anonymous functions and arrow
functions](https://www.typescriptlang.org/docs/handbook/functions.html#this-and-arrow-functions).

## What Comes Next

I'm going to continue to write projects with Typescript so that I can keep those skills fresh. I'm
still looking to write more small Javascript/component type projects.
