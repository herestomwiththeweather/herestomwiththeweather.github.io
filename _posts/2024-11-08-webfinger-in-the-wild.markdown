---
title: "Webfinger in the Wild"
layout: post
date: 2024-11-08 15:50:26
---
Today, a post in my feed included a mention and its webfinger verification threw a WebFinger::BadRequest exception:

```
Nov 08 09:18:49 AM  WebFinger::BadRequest (Bad Request):
Nov 08 09:18:49 AM 
Nov 08 09:18:49 AM  app/models/account.rb:79:in `fetch_and_create_mastodon_account'
Nov 08 09:18:49 AM  app/models/account.rb:367:in `block in create_status!'
Nov 08 09:18:49 AM  app/models/account.rb:364:in `each'
Nov 08 09:18:49 AM  app/models/account.rb:364:in `create_status!'
Nov 08 09:18:49 AM  app/lib/activity_pub/activity/create.rb:20:in `perform'
Nov 08 09:18:49 AM  app/controllers/accounts_controller.rb:148:in `process_item'
Nov 08 09:18:49 AM  app/controllers/accounts_controller.rb:75:in `inbox'
```
The activitypub actor document resided on mastodon.well.com but when a [reverse discovery](https://www.w3.org/community/reports/socialcg/CG-FINAL-apwf-20240608/#reverse-discovery) was performed, the hostname of the subject in the webfinger response was well.com instead of mastodon.well.com.  Making a webfinger request to well.com for the mentioned user returned a 500 Internal Server Error so a WebFinger::BadRequest exception was thrown.  What was going on?

Fortunately, [an issue in the activitypub-webfinger](https://github.com/swicg/activitypub-webfinger/issues/28) had the answer:

> Looks like some are using this host-meta redirect to use a custom domain for actors which is different to the actual domain of the server.

And that is what was happening:

```
curl https://mastodon.well.com/.well-known/host-meta
<?xml version="1.0" encoding="UTF-8"?>
<XRD xmlns="http://docs.oasis-open.org/ns/xri/xrd-1.0">
  <Link rel="lrdd" template="https://mastodon.well.com/.well-known/webfinger?resource={uri}"/>
</XRD>
```

A response in the issue notes

> The use of host-meta as a "second layer of indirection" is something that mostly a holdover from the OStatus days, IIRC. Most projects that aren't Mastodon or Pleroma will not check host-meta at all, and will instead always skip straight to the /.well-known/webfinger endpoint. I don't think it makes sense to unnecessarily pressure everyone into adopting host-meta or supporting variable LRDD endpoints

I can't argue with that so I just [handled the exception without setting the custom domain](https://github.com/herestomwiththeweather/irwin/commit/10a8d622e022bc48521afd11e63fb3962fce71f4).
