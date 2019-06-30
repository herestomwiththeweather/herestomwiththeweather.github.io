---
title: "Jekyll and Dat"
layout: post
date: 2019-06-30 13:40:00
---
Inspired by [Dat Night](https://datnight.org/), today I'm working on making my site available on the [Dat network](https://dat.foundation/).  For my [jekyll-indieweb](https://github.com/miklb/jekyll-indieweb) site, the main idea is that I should be able to do

```jekyll build```

```dat share _site```

which should mostly get me there.  I also want to add a [DNS txt record](https://beakerbrowser.com/docs/guides/use-a-domain-name-with-dat#dat-dns-txt-records) and it seems I need to update the javascript my site includes to bring in the webmentions my site has collected from my webmention provider.

With this setup, even if my hosting provider goes down, [Beaker browser](https://beakerbrowser.com/) will be able to browse my site.

**Update 13:28 PST**: DNS now serving DAT via TXT record:

	tbbrown@zero:~$ dig herestomwiththeweather.com TXT

	; <<>> DiG 9.11.4-3ubuntu5.4-Ubuntu <<>> herestomwiththeweather.com TXT
	;; global options: +cmd
	;; Got answer:
	;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 47409
	;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

	;; OPT PSEUDOSECTION:
	; EDNS: version: 0, flags:; udp: 65494
	;; QUESTION SECTION:
	;herestomwiththeweather.com.	IN	TXT

	;; ANSWER SECTION:
	herestomwiththeweather.com. 299	IN	TXT	"datkey=b8ffbddeac2c7eec8b60f1d33774f515f47affe4bef0ad6fec15bc0c6cb1ad78"

	;; Query time: 59 msec
	;; SERVER: 127.0.0.53#53(127.0.0.53)
	;; WHEN: Sun Jun 30 15:23:23 CDT 2019
	;; MSG SIZE  rcvd: 139

**Update 15:50 PST**: [Dats on a Server](https://docs.datproject.org/docs/dat-server) documents how to keep a Dat resource available independent of your laptop. 
