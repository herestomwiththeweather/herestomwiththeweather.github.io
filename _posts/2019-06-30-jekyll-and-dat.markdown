---
title: "Jekyll and Dat"
layout: post
date: 2019-06-30 13:40:00
---
Inspired by [Dat Night](https://datnight.org/), today I'm working on making my site available on the [Dat network](https://dat.foundation/).  For my [jekyll-indieweb](https://github.com/miklb/jekyll-indieweb) site, the main idea is that I should be able to do

```jekyll build```

```dat share _site```

which should mostly get me there.  I also want to add a [DNS txt record](https://beakerbrowser.com/docs/guides/use-a-domain-name-with-dat#dat-dns-txt-records) and it seems I need to update the javascript my site includes to bring in the webmentions my site has collected from my webmention provider.

With this setup, even if my hosting provider goes down, [Beaker browser](https://beakerbrowser.com/) will be able to browse my site.
