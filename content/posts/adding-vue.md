---
draft: false
title:  "Adding Vue"
date:   2020-09-11 20:20:00 -0500
author: Thomas Ruggeri
categories: [post, update]
tags: [front-end, vue, javascript]
---

It's been a while. I plan on writing more posts on all the things I've been up to in the last six months. One of them was finally experimenting with [VueJS](https://vuejs.org/). I'll cover my thoughts more in a future post.

For now I want to introduce my first Vue component (on this blog). I plan on expanding my playground by using VueJS on this very blog. This will have two benefits,

1. A playground to experiment with. Using the blog will keep me from having to create whole projects to practice. Think of it as [learning in public](https://hanselminutes.com/753/leaning-into-learning-in-public-with-monica-powell).

2. Allow for more backend experimentation. Hear me out on this one. If I can put Vue components on the blog, then I can point them to new, small, backend projects. This allows me to try things out in smaller, bite-sized, chunks.

My goals is to show off more work and make things more operational. I have a habit of starting a big project with grand plans only to see it relighted to github archives.

And with that, hello world.

> Note that the below script is no longer enabled

<div id="app">
  <hello-world></hello-world>
</div>

## How to add VueJS

This is a Jekyll blog, which is a statically generated site. The backend of [Jekyll](https://jekyllrb.com/) is Ruby based. Vue components need to be built separately from the static site. This is done through webpack.

I started the process by reading this helpful [blog post](https://hackwild.com/article/jekyll-ssg-with-vue-single-file-components/). It walked me through the setup of a webpack config, something that's notoriously difficult, and the node packages that are required. The end result is that there are Vue components in a folder and a `package.json` and `webpack.config.js` in the root directory. One can then run,

```bash
npm run build
```

to generate the javascript file output. This is then included in the Jekyll template along with my new component tag, `<hello-world></hello-world>` (seen above).

The upside of this approach is that the process is fairly minimal and only requires a rebuild before committing and pushing to Github pages.

The downside is that if you forget to run the production build, the build will be "broken" and it does require having all the node modules. Just see how large the `package-lock.json` file is to see how many dependencies there are. Finally, the javascript file for the resulting component is rather large - that's why I didn't include it on the index page.

I did hit a heck of a bug that took me a while to figure out. When I did everything the same as the article, my component styles were being built, but not loaded into the DOM. The blog post is a bit out of date, so something must have changed between then and now. I finally figured out that I needed to include [style-loader](https://webpack.js.org/loaders/style-loader/#root) into my webpack config.

Hopefully I can bootstrap from here and come up with more uses for Vue in this blog. Stay tuned.

## What I learned

I learned a lot about the structure of [`webpack.config.js`](https://webpack.js.org/concepts/configuration/). I also learned a little bit about [component registration](https://vuejs.org/v2/guide/components-registration.html) in Vue.

## What comes next

Add more components that do something with an API. Hopefully I can write a small app to host somewhere that can be interacted with via a component.
