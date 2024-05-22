---
title: "Feditesting!"
layout: post
date: 2024-05-22 17:29:29
---
It's cool to see the progress of the [FediTest](https://feditest.org/) project.  On March 7, there was a [show-and-tell online meeting](https://fedidevs.org/notes/2024-03-07/) and at the end of April, a [FediTest implementation update](https://feditest.org/blog/2024-04-30-update/) included a [Quickstart](https://feditest.org/docs/quickstart/) to try out some examples.

I was pleasantly surprised by the experience (including the specification annotations referencing each test) and the organization of the testing framework even at an early stage.  I was able to get all the tests for the [sass-imp-webfinger-server testplan](https://github.com/fediverse-devnet/feditest-tests-fediverse/blob/develop/example-testplans/saas-imp-webfinger-server.json) passing last night for [Irwin](https://github.com/herestomwiththeweather/irwin).  For each failing test, I created an issue and referenced the test (e.g. [ Well-known webfinger should respond with access-control-allow-origin header #15 ](https://github.com/herestomwiththeweather/irwin/issues/15) ).

Here's the output of this example testplan:

```
TAP version 14
# test plan: Unnamed
# started: 2024-05-22 06:33:53.423983+00:00
# ended: 2024-05-22 06:34:42.924770+00:00
# platform: Linux-6.5.0-28-generic-x86_64-with-glibc2.35
# username: tbbrown
# hostname: agency
# session: Unnamed
# constellation: Unnamed
#   roles:
#     - name: client
#       driver: imp.ImpInProcessNodeDriver
#     - name: server
#       driver: saas.SaasFediverseNodeDriver
ok 1 - webfinger.server.4_1__2_parameter_ordering_not_significant::parameter_ordering
ok 2 - webfinger.server.4_2__14_must_only_redirect_to_https::must_only_redirect_to_https
ok 3 - webfinger.server.4_2__3_requires_resource_uri::requires_resource_uri
ok 4 - webfinger.server.4_2__4_do_not_accept_malformed_resource_parameters::double_equals
ok 5 - webfinger.server.4_2__4_do_not_accept_malformed_resource_parameters::not_percent_encoded
ok 6 - webfinger.server.4_2__5_status_404_for_nonexisting_resources::status_404_for_nonexisting_resources
ok 7 - webfinger.server.4_5__1_any_uri_scheme_for_resource_identifiers::any_uri_scheme_for_resource_identifiers
ok 8 - webfinger.server.4__1_accepts_all_link_rels_in_query::accepts_combined_link_rels_in_query
ok 9 - webfinger.server.4__1_accepts_all_link_rels_in_query::accepts_known_link_rels_in_query
ok 10 - webfinger.server.4__1_accepts_all_link_rels_in_query::accepts_unknown_link_rels_in_query
ok 11 - webfinger.server.4__3_only_returns_jrd_in_response_to_https_requests::only_returns_jrd_in_response_to_https
ok 12 - webfinger.server.5_1_cors_header_required::cors_header_required
1..12
# test run summary:
#   total: 12
#   passed: 12
#   failed: 0
#   skipped: 0
#   errors: 0
```

Getting these tests passing certainly improved the code and my understanding of the webfinger spec. Thanks to Johannes Ernst and the [Fediverse Developer Network](https://fedidevs.org) community for this.
