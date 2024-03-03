---
title: "Internationalizing the Fediverse"
layout: post
date: 2024-03-03 17:12:47
---
Yesterday, [a request was made](https://fed.xnor.in/notice/AfRZC7jgkXRA1xk6Fc) to like a post from an account with unicode in the username. As Terence Eden notes in [Internationalise The Fediverse](https://shkspr.mobi/blog/2024/02/internationalise-the-fediverse/)

> Mastodon (the largest ActivityPub service) doesn't allow Unicode usernames and has resisted efforts to change.

So, I tried with [Irwin](https://github.com/herestomwiththeweather/irwin) on [otisburg.social](https://otisburg.social) and the code threw an exception when I tried to interact with the account

```
(URI::InvalidURIError) "URI must be ascii only \"https://i18n.viii.fi/@\\u4F60\\u597D\""
```

This is not an acceptable uri to pass to URI.parse().  In this case, I found some help from [a stackoverflow post](https://stackoverflow.com/questions/7015778/is-this-the-best-way-to-unescape-unicode-escape-sequences-in-ruby) and [fixed the code](https://github.com/herestomwiththeweather/irwin/commit/b5899fc090b654383a82674d2d1bf788e2e3ce9f) so I could interact with the account and like the post.

From the comments in Terence's blog post, I saw [Allowed characters in preferredUsername](https://github.com/swicg/activitypub-webfinger/issues/9) has also been identified as an issue with webfinger.
