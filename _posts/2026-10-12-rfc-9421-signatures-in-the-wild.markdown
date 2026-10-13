---
title: "RFC 9421 Signatures in the Wild"
layout: post
date: 2026-10-12 19:02:18
---
Like many others building software that communicates with servers on the Fediverse, I followed the [How to make friends and verify requests](https://blog.joinmastodon.org/2018/07/how-to-make-friends-and-verify-requests/) tutorial that demonstrated how a server can easily verify a digital signature to accept a message from another server.  It was good enough [to get started](https://herestomwiththeweather.com/2023/10/31/irwin-dabbling-with-activitypub/)!  Meanwhile, an IETF working group was producing a standard for HTTP Message Signatures that became [RFC 9421](https://www.rfc-editor.org/info/rfc9421/) and was published in February, 2024.  In August, 2026, [Mastodon 4.7 was released](https://blog.joinmastodon.org/2026/08/mastodon-4.7/) which began sending signatures with this new IETF standard:

> We now emit RFC 9421 signatures when requests fail using the previous draft specification, allowing compatibility between Mastodon 4.7 and servers that only implement the final RFC 9421 specification, while retaining compatibility with servers that only implement the previous draft specification

So, in September, I updated [Irwin](https://github.com/herestomwiththeweather/irwin) to [process incoming RFC 9421 signatures](https://github.com/herestomwiththeweather/irwin/issues/92).  However, because Irwin supports both legacy and standard signatures, the Mastodon 4.7 severs keep sending Irwin the legacy signatures but this is a post about RFC 9421 signatures in the wild, so where are they coming from?

In new versions of WordPress, [RFC 9421 is on by default](https://github.com/swicg/activitypub-http-signature/pull/71).  I am seeing several WordPress sites send them but here is one example request from [socialwebfoundation.org](https://socialwebfoundation.org) in the Irwin logs showing the ```signature-input``` request header which is a request header that does not exist in the legacy signatures:

	Serge::InboxDelivery#initialize signature-input: wp=(\"@method\" \"@target-uri\" \"@authority\" \"collection-synchronization\" \"content-digest\");created=1791401927;keyid=\"https://socialwebfoundation.org/author/evanprodromou/#main-key\";alg=\"rsa-v1_5-sha256\""

The ```Serge::InboxDelivery``` is the main class that I extracted into the [Serge](https://github.com/herestomwiththeweather/serge) gem library which handles both formats with one programming interface and is a work in progress.  As the name of the class suggests, it is only for inbox deliveries and not designed for "authorized fetch" GET requests.

[Mastering HTTP Signatures with UberEther’s CTO, Justin Richer](https://www.youtube.com/watch?v=jIVPNMrs3OY) is a good explanation of RFC 9421 signatures from the co-author of the specification.

What's next: outgoing RFC 9421 signatures.
