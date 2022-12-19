---
draft: false
title:  "Creating Tag Pages"
date:   2020-12-19 12:44:00 -0500
author: Thomas Ruggeri
categories: [post, update]
tags: [front-end, jekyll]
---

At the top of posts on this site, you can see the tags for each post. This was a somewhat "out-of-the-box" feature of the Jekyll theme that I chose to use and I always liked the look of them,
but it bugged me that they were just text without a link to anywhere. It made sense to me that they
would be a link to a page that included the posts that also used this tag.

Simple, right?

{{< figure src="https://media.giphy.com/media/Zdg7kl9bnyqXrPH2jq/giphy.gif"
width=480 height=480 class="center" alt="Image of boop" >}}

## New Layout

First, we need to make "tag" pages. This can be configured in the Jekyll config yaml file,

```yaml
collections:
  tags:
    output: true
    permalink: tags/:path/
```

This will create a file structure of `/tags/` with every markdown file that's in the `_tags`
directory of my source. Great, but I don't have any files in there yet. Let's manually create one,
say `front-end.md`, and come back to this later. We now will have a single page at `/tags/front-end`.

{{< figure src="https://media.giphy.com/media/3oKIPtyZ7IGAWlhnxu/giphy.gif"
width=500 height=500 class="center" alt="Image of wink" >}}

## Content

For content on these tag pages, we can assume we're given a `tag-name` in the front-matter. This allows us to create a header with the given tag name.

```html
<h1>{{ page.tag-name }}</h1>

The following posts mention {{ page.tag-name }}
```

Let's add a list of all the pages that match this tag. For this we will have to use the [variables
provided to us from Jekyll](https://jekyllrb.com/docs/variables/).

```html
<ul class="tags-list">
{% for post in site.posts %}
  {% if post.tags contains page.tag-name %}
    <li><a href="{{ post.url }}">{{ post.title }}</a></li>
  {% endif %}
{% endfor %}
</ul>
```

### Performance (aside)

The above code snippet really bothered me for a while. As a full-stack/backend engineer, I first
approached this part the way one would for a dynamic site - that is one that is running some type
of web server. Assuming the page is given a tag name, like `front-end`, it should be able to
"look up" all the pages that have that tag name. In [Rails](https://rubyonrails.org/), this would
look like,

```ruby
Pages.where(tags: Tag.find_by(name: given_tag_name))
```

The problem here is that we're not working with a dynamic site, we're working with Jekyll, a static
site generator (duh). This means that the concept of "looking up" the tags is pushed to
compile/generation time. What this really means is that we have to work with the [variables that
Jekyll gives us](https://jekyllrb.com/docs/variables/).

{{< figure src="https://media.giphy.com/media/xUPGcwqhFVcmQaDwJO/giphy.gif"
width=500 height=406 class="center" alt="Image of looking" >}}

Since they don't provide a pages per tag variable, we'll have to iterate over the pages and select
only those that use the given tag. If we have `n` pages, and `t` tags, that would be `O(n * t)`
to iterate over all pages once per tag. That seems really nasty to me, but we have to consider a few
things.

1. `t` is likely going to be far smaller than `n` overall, which would make this `O(n)` as the number
of pages grows.

2. This is done ahead of time, not during a page request as with a dynamic site.

If Jekyll were to give us a data structure of pages for a given tag, it could be done in `O(n + t)`
by iterating over the pages once and pushing a unique identifier into a hash that is keyed off the
tag, then iterating over the tags to generate the pages.

```ruby
{
  "front-end": [1, 2],
  "css": [2, 3]
}
```

This would be my "typical" answer to an interview question on the manner. A
[hash (dictonary, map)](https://www.interviewcake.com/concept/java/hash-map) is perfect for this
type of work because it allows us to create `O(1)` lookups by tag and remove unnecessary extra
traversals through data.

{{< figure src="https://media.giphy.com/media/3og0IHAQXjVi34wfZu/giphy.gif"
width=480 height=480 class="center" alt="Image of proud graduate" >}}

## Page per Tag (Plugins)

Back on topic, we now have a layout that will display the pages that contain a given tag, but we
would have to manually generate a markdown file for each tag we use. If we forget one, the system
breaks.

{{< figure src="https://media.giphy.com/media/fQJSYE2Qy6OtXfwEuf/giphy.gif"
width=480 height=480 class="center" alt="Image of eww" >}}

That doesn't jive with the easy-breezy automate everything lifestyle I'm going for. A quick Google
to see what others had come up with revealed a
[delightful article](https://blog.lunarlogic.io/2019/managing-tags-in-jekyll-blog-easily/) written
by Anna Ślimak that had a novel solution, use [Jekyll plugins](https://jekyllrb.com/docs/plugins/).
This takes advantage of the underlying Ruby code of Jekyll to extend the behavior. In this case, a
plugin that is run after each post is generated during build that can inspect the file system and
see if we need to write a tag markdown file. Great! We just put this file into our `_plugins`
directory and poof, we're done...

Wait, why are they not running?

{{< figure src="https://media.giphy.com/media/2uIhU2qPuiHJda5BjV/giphy.gif"
width=480 height=480 class="center" alt="Image of confused, looking" >}}

Turns out that [GitHub Pages](https://pages.github.com/) does not support plugins. This makes total
sense as if they did, anyone could run arbitrary Ruby code on their deploy servers. The
[gem](https://github.com/github/pages-gem) that I use therefore does not run plugins locally as well.
Unfortunately, it doesn't _SAY_ that plugins aren't run, so it took me a few minutes to figure that
one out.

{{< figure src="https://media.giphy.com/media/jaPkRECl8T8cuVjuw7/giphy.gif"
width=480 height=480 class="center" alt="Image of going insane" >}}

## Wrapping Up

It was then that I remember that I can write Ruby myself. I wrote a small script that I must run
before deploying to [GitHub Pages](https://pages.github.com/). This isn't perfect, but it works just
fine. The plugin would have had to be run by serving or building the site before commit anyway, so
this seems like a decent trade off.

With that, we now have pages for each unique tag used in the posts that show every article using that
tag. A little quick update to the post layout and css and we're good to go.

{{< figure src="https://media.giphy.com/media/SYcfbJpRiwCvcXWACx/giphy.gif"
width=480 height=480 class="center" alt="Image of success" >}}

## What I Learned

I learned some about the workings of Jekyll, including [plugins](https://jekyllrb.com/docs/plugins/).
I also thought through some amusing static site generation issues.

## What Comes Next

I would love to do more with the tags. Maybe a little javascript component that shows a preview
of the tags.
