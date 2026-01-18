---
title: "Codes of Conduct"
layout: post
date: 2026-01-18 13:12:48
---
Yesterday, I [checked in some code](https://github.com/herestomwiththeweather/irwin/commit/4fb2e14740d0b07fb4ceae22f5540f9ff5caf0b7) so that [my Fediverse server](https://otisburg.social) can respond to [requests to the api/v2/instance endpoint](https://docs.joinmastodon.org/methods/instance/#v2) so that code of conduct rules can be fetched.

<script src="https://asciinema.org/a/769178.js" id="asciicast-769178" async="true"></script>

Although the code is already running, I plan to add more on this feature so that the server can better communicate the code of conduct to its peers.  This is related to what Robert W. Gehl calls the "covenantal fediverse" in his book [Move Slowly and Build Bridges](https://openlibrary.org/isbn/9780197776681).  I still have the last third of the book left to read but I have noticed that Gehl does not mention [Bluesky](https://en.wikipedia.org/wiki/Bluesky) until the epilogue.  People on the [Fediverse](https://en.wikipedia.org/wiki/Fediverse) can communicate with people on Bluesky through [Bridgy Fed](https://fed.brid.gy/) and about a year ago, I [added Bluesky support](https://github.com/herestomwiththeweather/irwin/issues/37) through Bridgy Fed to my Fediverse server.

Although it seems Bluesky does not have API support for requests to fetch code of conduct rules, it does have [Community guidelines](https://bsky.social/about/support/community-guidelines) and it seems there could be consequences including account termination when an account does not follow the guidelines.  This weekend, people on the Fediverse have been sharing [instructions to block Bluesky](https://m.ai6yr.org/@ai6yr/115907898577599466) since Bluesky gave a blue check to [ICE](https://en.wikipedia.org/wiki/United_States_Immigration_and_Customs_Enforcement).  Considering the history of behavior by [X](https://en.wikipedia.org/wiki/Twitter) accounts related to ICE, this is a completely reasonable response.  It will be interesting to see if this new Bluesky account can last a week without breaking Bluesky's community guidelines.  If they do misbehave and Bluesky does not aggressively respond to the infraction, Bluesky will be blocked on my server.
