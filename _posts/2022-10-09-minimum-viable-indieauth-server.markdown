---
title: "Minimum Viable IndieAuth Server"
layout: post
date: 2022-10-09 20:38:46
---
One of the building blocks of the [Indieweb](https://indieweb.org) is [IndieAuth](https://indieweb.org/IndieAuth). Like many others, I bootstrapped my experience with [indieauth.com](https://indieauth.com/) but as [Marty McGuire explains](https://martymcgui.re/2022/07/31/switching-costs-for-an-indieauth-server/), there are good reasons to switch and even consider building your own.  Because I wanted a server as simple to understand as possible but also wanted to be able to add features that are usually not available, I created a rails project called [Irwin](https://github.com/herestomwiththeweather/irwin) and [recently configured my blog to use it](https://github.com/herestomwiththeweather/herestomwiththeweather.github.io/commit/554dbaad13250a97c2b26c034fbce3dd6908c273).

This is not production ready code.  While I know that [the micropub server I use](https://github.com/voxpelli/webpage-micropub-to-github) works with it, I expect others may not. Also, there is no support for refresh tokens and other things in the spec that I didn't consider high priority.  It does support [PKCE](https://indieweb.org/PKCE) but not the less useful "plain" method.

All of [IndieAuth Spec Updates 2020](https://aaronparecki.com/2020/12/03/1/indieauth-2020) was very clear and helpful.  In one case, I made the server probably too strict (as an easy way to curtail spam registrations).  It requires that the hosts for a blog's authorization endpoint and token endpoint match the host of the IndieAuth server before a user can register an account on the indieauth server.

I plan to add an option for a user to keep a history of logins to indieauth clients soon.  Please let me know if you have any questions or suggestions.
