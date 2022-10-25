---
title: "IndieAuth login history"
layout: post
image: https://coffeebucks.s3.amazonaws.com/images/image/jekyll/indieauth_login_history.png
date: 2022-10-25 16:51:16
---
In my last post, I mentioned that I planned to add login history to [Irwin](https://github.com/herestomwiththeweather/irwin).  As I was testing my code, I logged into [indieweb.org](https://indieweb.org) and noticed that I needed to update my code to support [5.3.2 Profile URL Response](https://indieauth.spec.indieweb.org/#profile-url-response) of the IndieAuth spec as this IndieAuth client does not need an access token.  Here's what the history looks like on my IndieAuth server:

![IndieAuth login history](https://coffeebucks.s3.amazonaws.com/images/image/jekyll/indieauth_login_history.png)

If I click on a login timestamp, I have the option to revoke the access token associated with the login if it exists and has not already expired.  My next step is to test some other [micropub servers](https://indieweb.org/Micropub/Servers) than the one I use to see what interoperability updates I may need to make.
