---
title: "check_webfinger!"
layout: post
date: 2022-12-23 11:37:50
---
The notes I made in [Mastodon Discovery](https://herestomwiththeweather.com/2022/11/15/mastodon-discovery/) skipped over a noteworthy step.  In general, after mastodon fetches and parses the "well known" webfinger document (the so-called JSON Resource Descriptor), there is a 3 step process to learn about the actor referenced in that document.

1. fetch_resource
2. check_webfinger!
3. create_account

As mentioned previously, in the first step, a very comprehensive json document for the actor is fetched and in the third step, an account is created for that actor if does not already exist.  However, between those two steps, mastodon does another webfinger lookup since, for instance, the domain serving the actor document may be a different domain than the one that originally served the first "well known" webfinger document.  Prior to this check, [some instance variables are set](https://github.com/herestomwiththeweather/mastodon/blob/main/app/services/activitypub/fetch_remote_actor_service.rb#L32):

```
    @uri      = @json['id']
    @username = @json['preferredUsername']
    @domain   = Addressable::URI.parse(@uri).normalized_host
```
The @uri instance variable is the location of the actor document and the @domain instance variable is the domain that serves the actor document.  After these variables are set, [the check is performed](https://github.com/herestomwiththeweather/mastodon/blob/main/app/services/activitypub/fetch_remote_actor_service.rb#L36):

```
    check_webfinger! unless only_key
```
This check enforces that the domain component of your identifier is the domain that serves your actor document. (It [inspects the subject of the "well known" document](https://github.com/herestomwiththeweather/mastodon/blob/main/app/services/activitypub/fetch_remote_actor_service.rb#L48) and if the username and domain of the subject match the instance variables above, the 'self' resource link is required to be the same as the @uri instance variable.  If the subject does not match, one more webfinger lookup for the redirection is allowed.)
 
So, from the perspective of mastodon, the domain component of your identifier you are known as is determined by which domain serves your actor document rather than the domain serving the original "well known" webfinger document.  It seems if your domain is a static site and you want to be known by an identifier associated with your domain, your domain needs to serve the actor document in addition to "well known" webfinger document. 
