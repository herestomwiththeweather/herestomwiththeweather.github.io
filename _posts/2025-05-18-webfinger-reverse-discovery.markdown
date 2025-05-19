---
title: "Webfinger Reverse Discovery"
layout: post
date: 2025-05-18 14:49:22
---
Activitypub addresses the problem of participating in a decentralized social network with a low barrier to entry. You participate through the server you have joined but often times the people you want to interact with reside on other servers. For instance, if you want to follow a friend, visiting that friend's url does not provide a simple follow button.  That simple follow button is on your own server but you need to navigate to your server's profile page for your friend who is on a remote server.  An easy way to do this is to perform a search on your friend's webfinger address which looks like an email address. Your server can make a [forward discovery](https://www.w3.org/community/reports/socialcg/CG-FINAL-apwf-20240608/#forward-discovery) request to ask for the url of your friend's actor document so that you can visit your server's profile page for your friend.

Your server needs to do more than forward discovery to validate that the actor url actually belongs to the requested webfinger address in case the domain of the webfinger address is different than the domain of the actor url.  In this case, after fetching the actor url, your server needs to construct a 2nd webfinger address composed of the preferredUsername it found in the actor document followed by the domain of the actor url.  Your server can make a webfinger request to this 2nd address and use the response to verify that the subject matches the original webfinger address that you submitted in your search.  If they don't match, your server can display the profile associated with the 2nd address and ignore the original webfinger address since the validation failed.
 
I wrote a [should use the custom domain](https://github.com/herestomwiththeweather/irwin/commit/330765dc10054156cd26c397ed1973c93ed30760#diff-e66847e10576724ed355f062a794f1953374908bf08e248a62f3ccde6682de42R25-R34) example spec to make sure the server can accommodate a custom domain different than the domain in the actor url.

In the example spec, we are given bob@example.com whose webfinger points to an actor document at activitypub.test:

	let(:bob_webfinger_info) { {"subject" => "acct:bob@example.com", "links"=>[{"rel"=>"self", "type"=>"application/activity+json", "href"=>"https://activitypub.test/users/bob" }]} }

It is not enough to fetch the actor document and assume bob is at activitypub.test.  Instead, [as Mastodon does](https://docs.joinmastodon.org/spec/webfinger/#mastodons-requirements-for-webfinger), a reverse discovery should be performed by constructing a new WebFinger address by combining the preferredUsername from the actor document and the hostname of the id of the actor document.

In the example spec, this new WebFinger address would be bob@activitypub.test and, in this case, the test host activitypub.test returns a webfinger response that confirms that the subject is bob@example.com that was requested with forward discovery.

Another example spec [should not use the custom domain if subject returned by activitypub server is different than the original subject](https://github.com/herestomwiththeweather/irwin/commit/8f4b4f3abb8e47bd1c9d144439874f25ed6cf0c1#diff-e66847e10576724ed355f062a794f1953374908bf08e248a62f3ccde6682de42R61-R93) tests when george@example.com is not recognized by the host activitypub.test who george points his webfinger address to:

	let(:george_webfinger_info) { {"subject" => "acct:george@example.com", "links"=>[{"rel"=>"self", "type"=>"application/activity+json", "href"=>"https://activitypub.test/users/george" }]} }

In this case, the validation fails because the host returns acct:george@activitypub.test in the 2nd webfinger request instead of acct:george@example.com so example.com is discarded and the domain of the account should fall back to activitypub.test.
