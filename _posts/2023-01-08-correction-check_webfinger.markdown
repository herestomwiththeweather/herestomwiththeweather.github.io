---
title: "Correction: check_webfinger!"
layout: post
date: 2023-01-08 18:52:24
---
Mastodon is not the fediverse and in my [check_webfinger!](https://herestomwiththeweather.com/2022/12/23/check_webfinger/) post, I'm afraid I made that assumption.  In particular, I concluded

> So, from the perspective of mastodon, the domain component of your identifier you are known as is determined by which domain serves your actor document rather than the domain serving the original “well known” webfinger document.

which is not necessarily true if you consider the fediverse outside of Mastodon.

Instead, it seems that I should have said that the domain component of your identifier is determined by the domain component of the subject field returned in the webfinger response from the domain that serves your actor document when mastodon makes its 2nd webfinger request which is done in the [check_webfinger!](https://github.com/herestomwiththeweather/mastodon/blob/main/app/services/activitypub/fetch_remote_actor_service.rb#L48) method.

```
  def check_webfinger!
    webfinger                            = webfinger!("acct:#{@username}@#{@domain}")
    confirmed_username, confirmed_domain = split_acct(webfinger.subject)
```

In the code above, the @domain passed to webfinger! is the domain of the server providing the activitypub actor document but the confirmed_domain can be different (e.g. your personal domain) if your original "well known" webfinger document was not pointing to a Mastodon server for providing the actor document.

Therefore, if you have a static personal website, it is not necessary to also host the actor document there as long as the fediverse node providing the actor document is smart enough to provide your personal domain in the subject when mastodon makes a webfinger call to it.  A caveat is that such a fediverse node accomodating personal domains would not be able to distinguish between bob@a.com and bob@b.com when mastodon webfingers server.com for bob@server.com. 
