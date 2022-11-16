---
title: "Mastodon Discovery"
layout: post
date: 2022-11-15 16:42:33
---
Making notes is helpful when reading and running unfamiliar code for the first time.  I usually start with happy paths.  Here's some notes I made while learning about Mastodon account search and discovery.  It's really cool to poke around the code that so many people are using every day to find each other.

When you search on an account identifier on Mastodon, your browser makes a request to your Mastodon instance:

> /api/v2/search?q=%40herestomwiththeweather%40mastodon.social&resolve=true&limit=5

The resolve=true parameter tells your Mastodon instance to make a webfinger request to the target Mastodon instance if necessary. The search controller [makes a call to the SearchService](https://github.com/herestomwiththeweather/mastodon/blob/main/app/controllers/api/v2/search_controller.rb#L32)

```
  def search_results
    SearchService.new.call(
      params[:q],
      current_account,
      limit_param(RESULTS_LIMIT),
      search_params.merge(resolve: truthy_param?(:resolve), exclude_unreviewed: truthy_param?(:exclude_unreviewed))
    )
  end
```

and since resolve=true, SearchService [makes a call to the ResolveAccountService](https://github.com/herestomwiththeweather/mastodon/blob/main/app/services/account_search_service.rb#L36)

```
      if options[:resolve]
        ResolveAccountService.new.call(query)
```

The purpose of ResolveAccountService is to "Find or create an account record for a remote user" and return an account object to the search controller. It includes [WebfingerHelper](https://github.com/herestomwiththeweather/mastodon/blob/main/app/helpers/webfinger_helper.rb) which is a trivial module with just one one-line method named webfinger!()

```
module WebfingerHelper
  def webfinger!(uri)
    Webfinger.new(uri).perform
  end
end
```

This method returns a webfinger object. Rather than call it directly, ResolveAccountService [invokes process_webfinger!](https://github.com/herestomwiththeweather/mastodon/blob/main/app/services/resolve_account_service.rb#L34) which [invokes it](https://github.com/herestomwiththeweather/mastodon/blob/main/app/services/resolve_account_service.rb#L86) and then asks the returned webfinger object's subject method for its username and domain and makes them instance variables of the service object.

```
  def process_webfinger!(uri)
    @webfinger                           = webfinger!("acct:#{uri}")
    confirmed_username, confirmed_domain = split_acct(@webfinger.subject)

    if confirmed_username.casecmp(@username).zero? && confirmed_domain.casecmp(@domain).zero?
      @username = confirmed_username
      @domain   = confirmed_domain
      return
    end
```

If the Mastodon instance does not already know about this account, ResolveAccountService [invokes fetch_account!](https://github.com/herestomwiththeweather/mastodon/blob/main/app/services/resolve_account_service.rb#L55) which [calls the ActivityPub::FetchRemoteAccountService](https://github.com/herestomwiththeweather/mastodon/blob/main/app/services/resolve_account_service.rb#L114) which inherits from [ActivityPub::FetchRemoteActorService](https://github.com/herestomwiththeweather/mastodon/blob/main/app/services/activitypub/fetch_remote_actor_service.rb)

```
      @account = ActivityPub::FetchRemoteAccountService.new.call(actor_url, suppress_errors: @options[:suppress_errors])
```

The actor_url will look something like

> https://mastodon.social/users/herestomwiththeweather

The ActivityPub::FetchRemoteActorService [passes the actor_url parameter to fetch_resource](https://github.com/mastodon/mastodon/blob/main/app/services/activitypub/fetch_remote_actor_service.rb#L19) to receive a json response for the remote account.

```
    @json = begin
      if prefetched_body.nil?
        fetch_resource(uri, id)
      else

```

The response includes a lot of information including name, summary, publicKey, images and urls to fetch more information like followers and following.

Finally, the ActivityPub::FetchRemoteActorService [calls the ActivityPub::ProcessAccountService](https://github.com/mastodon/mastodon/blob/main/app/services/activitypub/fetch_remote_actor_service.rb#L38), passing it the json response. 

```
    ActivityPub::ProcessAccountService.new.call(@username, @domain, @json, only_key: only_key, verified_webfinger: !only_key)
```

If the Mastodon instance does not know about the account, ActivityPub::ProcessAccountService [invokes create_account and update_account](https://github.com/mastodon/mastodon/blob/main/app/services/activitypub/process_account_service.rb#L28) to save the username, domain and all the associated urls from the json response to a new account record in the database.

```
      create_account if @account.nil?
      update_account
```

I have several questions about how following others works and will probably look at that soon.  I may start out by reading [A highly opinionated guide to learning about ActivityPub](https://tinysubversions.com/notes/reading-activitypub/) which I bookmarked a while ago.
