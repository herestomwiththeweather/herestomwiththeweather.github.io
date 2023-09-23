---
title: "Webfinger Expectations"
layout: post
date: 2023-09-22 14:34:49
---
In an [earlier post](https://herestomwiththeweather.com/2023/01/08/correction-check_webfinger/) this year, I documented a problem I found and this post attempts to describe the issue a little more clearly and a plan to work around it.

I have chosen [@tom@herestomwiththeweather.com](https://herestomwiththeweather.com/.well-known/webfinger/?acct=tom@herestomwiththeweather.com) as my personal identifier on the fediverse.  If I decide I want to move from one activitypub server (e.g. Mastodon) to another, I would like to keep my same personal identifier.  It follows that my activitypub server should not have to reside at the same domain as my personal identifier.  I should be able to swap one activitypub server for another at any time.  Certainly, I don't expect every activitypub server to support this but I'm not obligated to use one that does not.

Unfortunately, although my domain returns the chosen personal identifier in the subject field, because the JRD document returns a rel=self link to [a Mastodon server](https://mastodon.social/users/herestomwiththeweather) to provide my [actor document](https://www.w3.org/TR/activitypub/#actor-objects), the mastodon servers do not seem to use my chosen personal identifier for anything other than resolving a search for my personal identifier to the mastodon profile to which it is currently associated.  From that point forward, a completely new personal identifier with the domain component set to the domain of the mastodon server is used.  In other words, a personal identifier that has been chosen for me by someone else is kept in a particular server's database table.  I can later choose a different activitypub server but I may not be able to keep my preferred username because it may already be taken on the new server.  In any case, choosing a new server means my personal identifier within the mastodon network also changes.  Unless...I don't use a mastodon server in the first place.  Then, my personal identifier will be used as I would like by the mastodon network and I can potentially swap activitypub servers without ever having to change my personal identifier with my own domain.

The two most relevant documents for understanding webfinger as it is currently used seem to be [RFC 7033: WebFinger](https://datatracker.ietf.org/doc/html/rfc7033) and [Mastodon's documentation](https://docs.joinmastodon.org/spec/webfinger/) and it is this mastodon documentation (in the section [Mastodon's requirements for WebFinger](https://docs.joinmastodon.org/spec/webfinger/#mastodons-requirements-for-webfinger)) that now describes the behavior (a problem for me) that I documented earlier.  The new section explains

	if the subject contains a different canonical account URI, then Mastodon will perform an additional Webfinger request for that canonical account URI in order to ensure that this new resource links to the same ActivityPub actor with the same criteria being checked.

This behavior makes sense if you assume that if you are using a mastodon server, then you inherit a personal identifier tied to that server.  This makes validating a webfinger address simple for mastodon so advocating a change in this behavior in mastodon seems like it would be challenging.  However, as I mentioned in the earlier post, instead of choosing mastodon as your activitypub server, your personal identifier with your own domain can be accepted by mastodon servers in a desirable way

	as long as the fediverse node providing the actor document is smart enough to provide your personal domain in the subject when mastodon makes a webfinger call to it.

The problem here is that it seems that I would not be able to be "tom" on such an activitypub server if, for instance, tom@example.com was already pointing to that server unless the server could assign me a subdomain, for example.

