---
draft: false
title:  "Adding Svelte"
date:   2020-12-13 15:20:00 -0500
author: Thomas Ruggeri
categories: [post, update]
tags: [front-end, svelte, javascript]
---

Wait, wasn't the last post about Vue? How have we switched to Svelte already?!? Let me explain.

[Svelte](https://svelte.dev/) is a fantastic front-end component library developed originally by [Rich Harris](https://github.com/Rich-Harris) of the New York Times. It doesn't have the widespread popularity that Vue, React and Angular have, but it is growing in notoriety, especially since the release of version 3 in 2019. In my limited world view (of the front-end), it stands out because it has always focused on [pre-compilation](https://svelte.dev/blog/frameworks-without-the-framework) to provide a ... svelte ... javascript file rather than including a "run-time" library of javascript code that [manipulates the virtual DOM](https://svelte.dev/blog/virtual-dom-is-pure-overhead). This is a feature that has since been developed into many frameworks/libraries, but it is what makes Svelte unique.

Coming back to this blog, why would I work to include another component library? Simply put, this isn't a webapp, it's a playground for me to explore and try things. This means that sometimes I may want to play with Vue, but other times, like today, I want to play with Svelte. And with that, let's re-create the hello world component.

> Note that the below script is no longer enabled

<div id="svelte_hello_world"></div>

How does this (very) simple component compare between Vue and Svelte? Let's take a quick look at the two components without style.

### Vue

```html
<template>
  <section class="box">
    <div style="text-align: center;">
      <div><button v-on:click="show = !show">Hello</button></div>
      <transition name="fade">
        <p v-if="show" style="text-align: center;">World</p>
      </transition>
    </div>
  </section>
</template>

<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      show: false
    };
  }
}
</script>
```

### Svelte



```html
<script>
  import { fade } from 'svelte/transition';

  let show = false;
</script>

<section class="box">
  <div style="text-align: center;">
    <div><button on:click={() => { show = !show; }}>Hello</button></div>
    {#if show === true}
      <p transition:fade="{\{delay: 250, duration: 300}\}" style="text-align: center;">World</p>
    {/if}
  </div>
</section>
```



You can see a very similar syntax for such simple components. Each have an "if" directive and a simple way to create click handlers. The transition fade is also very easy to add right in the markup.

## What I learned

I had to re-learn the [syntax for Svelte components](https://svelte.dev/docs#Template_syntax) and got to play with Webpack config even more to create [multiple outputs](https://webpack.js.org/configuration/configuration-types/#exporting-multiple-configurations).

## What comes next

I have a few small ideas to play with some css tricks to make some interactive components for the front page.
