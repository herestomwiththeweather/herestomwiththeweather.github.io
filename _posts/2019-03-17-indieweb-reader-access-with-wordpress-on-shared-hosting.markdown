---
title: "Indieweb reader access with Wordpress on shared hosting"
layout: post
date: 2019-03-17 14:03:31
---
An [Indieweb reader](https://indieweb.org/reader) (like [Monocle](https://monocle.p3k.io/), [Together](https://alltogethernow.io/) or [Indigenous](https://indieweb.org/Indigenous)) authenticates you with your personal website which also tells the indieweb reader where to fetch your feeds.  [Wordpress](https://wordpress.org/) is a popular option for a personal website.

Although I'm not an active wordpress user, I've helped a few wordpress users interested in adding [Indieweb](https://indieweb.org) functionality.  I haven't had much of a problem with the indieweb wordpress [plugins](https://indieweb.org/WordPress/Plugins) and [themes](https://indieweb.org/WordPress/Themes) but from time to time, I've needed a little help from the usual suspects on the [indieweb-wordpress](https://chat.indieweb.org/wordpress/) chat.

I also have very little experience with [shared hosting](https://indieweb.org/web_hosting#Shared_Hosting) but it seems that with their one-click wordpress installers, this could be a good option to experience the benefits of an indieweb reader while avoiding more complex hosting options.  Shared hosting seems to be much more affordable, too.

At [IndiewebCamp Austin 2019](https://indieweb.org/2019/Austin), I investigated [Dreamhost](https://www.dreamhost.com/) and [Reclaim Hosting](https://reclaimhosting.com/) as two shared hosting options for interoperability with indieweb readers.

It turned out that what I thought was a missing piece was already available.  Your website needs to declare where you keep your feeds so that your indieweb reader can find them.  On wordpress, this can be accomplished with the [aperture plugin](https://wordpress.org/plugins/aperture/).  So, I added and activated this plugin to my existing indieweb wordpress test site hosted on dreamhost.  The plugin automates the creation of an [aperture](https://aperture.p3k.io/) account and adds a [microsub](https://indieweb.org/Microsub) link tag to your web page which points to your aperture url so that your indieweb reader can find your feeds.  This ability to declare where you are storing your feeds allows you to change the microsub provider that is hosting your feeds.  There are already several microsub providers to choose from.

Next, I tried logging into the Monocle indieweb reader and received a "bearer token not supplied" error.

![Bearer token not supplied](https://s3.amazonaws.com/stark-moon-46/images/bearer_token_not_supplied.png)

Aaron Parecki who was nearby explained to me that the web server running wordpress was unfortunately stripping out the authorization request header.  A quick search revealed a [workaround for this issue](https://islandinthenet.com/aperture/#comment-13386) by adding the following line to your .htaccess file:

	SetEnvIf Authorization "(.*)" HTTP_AUTHORIZATION=$1

At this point, my unfamiliarity with shared hosting bit me as I had purposely avoided looking under the hood of my account.  So, I assumed I could just use my normal credentials to upload the needed .htaccess file but my changes were ignored.  It did not occur to me that there was another dreamhost user account under which the one-click wordpress was installed and I needed to use those credentials to make the changes.  

As I didn't immediately figure this out, I decided to try [Reclaim Hosting](https://reclaimhosting.com/) as an alternative shared hosting provider and installed wordpress and the appropriate indieweb plugins and an indieweb-friendly theme.  I had better luck this time (and much more affordable).  Again, I ran into the "bearer token not supplied" error when logging into the indieweb reader, but luckily there was no multiple user account confusion with reclaim hosting.  I could easily add the .htaccess file and confirm that it fixed the "bearer token not supplied" issue.  

However, somehow I found myself in a coherency problem state as I received a "This token was issued to a different user" error where aperture was saying my microsub url was different than my website was claiming (different aperture user accounts).  I was not able to resolve this issue by reinstalling the aperture wordpress plugin but I was able to resolve it by changing the <span style="background-color: rgb(234, 239, 239); color: rgb(51, 51, 51);">aperture_microsub_url</span> option name in the <span style="background-color: rgb(234, 239, 239); color: rgb(51, 51, 51);">wp_options</span> database table to match the one aperture was expecting. 

