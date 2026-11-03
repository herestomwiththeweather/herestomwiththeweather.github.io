---
title: "Outgoing RFC 9421 Signatures"
layout: post
date: 2026-11-03 13:41:15
---
Previously, I mentioned that support for incoming [RFC 9421](https://www.rfc-editor.org/info/rfc9421/) signatures was added to [Irwin](https://github.com/herestomwiththeweather/irwin) for inbox deliveries.  A week ago, I finished implementing outgoing signatures for both inbox deliveries and "authorized fetch" GET requests.  So, how does it know whether to send a legacy signature or a new RFC 9421 signature?  In general, the [double-knocking technique](https://swicg.github.io/activitypub-http-signature/#how-to-upgrade-supported-versions):

> First, try generating or verifying an HTTP Signature with one version, ideally (but not necessarily) the latest. If the remote server rejects that signature, eg with an HTTP 401 response, or the incoming signature doesn't verify, try with another version. Repeat until a signature passes or you've tried all supported versions.
>
>(Many fediverse servers do process incoming activities asynchronously, but they generally still verify signatures synchronously, so double knocking is still viable when delivering activities to remote inboxes.)

So, first try RFC 9421 and if that returns a 401, then retry with the legacy signature.

However, Irwin also has a hosts database table and a new ```legacy_signature``` boolean was added to it.  By default, it is false.  There is a long tail of fediverse hosts that do not support RFC 9421: many hosts that my fediverse server rarely interacts with and I do not bother with setting their ```legacy_signature``` to true.

However, there are a few that my fediverse server does interact with a lot, so their host records have been updated so that Irwin doesn't make many unnecessary requests. For example, threads.net returns 404 when it is sent a request signed with RFC 9421 rather than the expected 401 that correctly implemented servers return.

So, at the start of [HttpClient#request_headers()](https://github.com/herestomwiththeweather/irwin/blob/main/app/lib/http_client.rb#L42), the legacy signature is sent for such hosts:

```ruby
  def request_headers(method_string, legacy_signature = false)
    if !legacy_signature && Host.find_by(name: @url.host)&.legacy_signature
      Rails.logger.info "#{self.class}##{__method__} setting legacy signature for #{@url.host}"
      legacy_signature = true
    end
```

The API method to construct signature headers for both legacy and RFC 9421 signatures is the same.

```ruby
    klass = legacy_signature ? Serge::FediSignature : Serge::MessageSignature
    Rails.logger.info "#{self.class}##{__method__} signing request with #{klass}"

    headers.merge!(
      klass.headers(
        method: method_string,
        url: @url,
        body: @body,
        public_key_id: @main_key_url,
        private_key: @private_key
      )
    )
``` 
The ruby gem library [serge](https://github.com/herestomwiththeweather/serge) that is being used here uses the [linzer](https://github.com/nomadium/linzer) gem for RFC 9421 signatures and Irwin's old code for the legacy signatures.
