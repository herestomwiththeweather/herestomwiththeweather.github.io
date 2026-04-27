---
title: "Follow button with Activity Intents"
layout: post
date: 2026-04-26 19:45:06
---
I don't want to brag but I finally added a follow button to my static jekyll blog. Because it uses [Activity Intents](https://codeberg.org/fediverse/fep/src/branch/main/fep/3b86/fep-3b86.md), a visitor can *remotely* follow my fediverse account regardless of where their host server lives as long as their server supports Activity Intents.  The good news is that [mastodon.social](https://mastodon.social) already supports this as it is running the nightly build. It will be included in the next major release (4.6) as mentioned in [Trunk & Tidbits, March 2026](https://blog.joinmastodon.org/2026/04/trunk-tidbits-march-2026/) so that other Mastodon servers will support it.

The code was added to Mastodon in [Add support for FEP-3b86 (Activity Intents) (#38120)](https://github.com/mastodon/mastodon/commit/69b1f60f4e46cf58e7240c2bfb81588accc1af6f) and it seems there are 2 different values for "rel" a home server may offer to accept a follow: [4.10 Follow Intent](https://codeberg.org/fediverse/fep/src/branch/main/fep/3b86/fep-3b86.md#4-10-follow-intent) and [5.1 Object Intent](https://codeberg.org/fediverse/fep/src/branch/main/fep/3b86/fep-3b86.md#5-1-object-intent) so my button [accepts 2 different values](https://github.com/herestomwiththeweather/herestomwiththeweather.github.io/blob/master/_includes/follow.html#L42).

	var rels = ['https://w3id.org/fep/3b86/Follow', 'https://w3id.org/fep/3b86/Object'];

Intents are for all activities but it seems there is a tendency for fediverse home servers to support just a subset of activities at the moment. Earlier this week, I added [support just for follow and like](https://github.com/herestomwiththeweather/irwin/commit/6d06e99d93d00317ee6f532a0a336f8ccdc2c4f4) for [my home server](https://otisburg.social).  Since my webfinger identifier has a different domain than my fediverse server, I also had to [add intents to webfinger](https://github.com/herestomwiththeweather/herestomwiththeweather.github.io/commit/d11d2354bea1782a2df16f2a0c5d3118ffa454d2) in my jekyll software as well as [allow webfinger to respond to CORS request](https://github.com/herestomwiththeweather/herestomwiththeweather.github.io/commit/e7001989233c08d7afc1d3835dc846e6e65e2eb5). 
