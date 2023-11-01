---
title: "Irwin: Dabbling with ActivityPub"
layout: post
date: 2023-10-31 23:36:34
---
It [has been a year](https://herestomwiththeweather.com/2022/10/25/indieauth-login-history/) since I have blogged about my IndieAuth server [Irwin](https://github.com/herestomwiththeweather/irwin).  Prior to that, in [Minimum Viable IndieAuth Server](https://herestomwiththeweather.com/2022/10/09/minimum-viable-indieauth-server/), I explained my motivation for starting the project.  In the same spirit, I would like an activitypub server as simple to understand as possible.  I thought it might be interesting to add the activitypub and webfinger support to an IndieAuth server so I have created an experimental branch [ap_wip](https://github.com/herestomwiththeweather/irwin/tree/ap_wip).  An important part of this development has been writing specs.  For example, [here are my specs](https://github.com/herestomwiththeweather/irwin/blob/ap_wip/spec/requests/accounts_spec.rb#L23) for handling the "[Move](https://docs.joinmastodon.org/spec/activitypub/#Move)" command, an important Mastodon feature.

I still have about half a dozen items to do before I consider dogfooding this branch but hopefully I can do that soon.
