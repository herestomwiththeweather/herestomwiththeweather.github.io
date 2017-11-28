---
title: "Jekyll-Indieweb and Micropub"
layout: post
date: 2017-11-27 21:09:38
---
With [jekyll-indieweb](https://github.com/miklb/jekyll-indieweb), posting to your static Jekyll site from a mobile device is easy after initial setup.

We can do that with voxpelli's [webpage-micropub-to-github](https://github.com/voxpelli/webpage-micropub-to-github/) which accepts a post from any micropub compatible client like [quill](https://quill.p3k.io/) and converts the post into a jekyll-friendly git commit including the yaml front matter and content.

If we try to sign into quill prior to setting up the micropub endpoint, these error messages will be returned.
![quill error](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/micropub/quill_error.png)

This means that our jekyll site does not include any links to indieauth endpoints nor a link to a micropub endpoint.  Before we can link to a micropub endpoint, we need to deploy our micropub endpoint (webpage-micropub-to-github).  One way to deploy [webpage-micropub-to-github](https://github.com/voxpelli/webpage-micropub-to-github/) is to use the [Deploy to Heroku button](https://heroku.com/deploy?template=https://github.com/voxpelli/webpage-micropub-to-github) (found on its project page) which will present you with environment variables that need to be configured. You might prefer deploying from the command line with a heroku create, git push and a heroku config:add for each environment variable.  Either one works.

![heroku button](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/micropub/heroku_button.png)

The first variable MICROPUB_GITHUB_TOKEN requires generating a new access token by visiting [https://github.com/settings/tokens](https://github.com/settings/tokens).  This token will authorize webpage-micropub-to-github to push a new commit to your jekyll repo when you make a post from a micropub client (like quill).

![github token](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/micropub/github_token.png)

Assuming your jekyll repository is public, make sure to check the public_repo scope.  Otherwise, you will receive an ambiguous "Your endpoint did not return a Location header" error when you attempt to make a post.

![public repo authorization](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/micropub/public_repo_authorization.png)

The other four env variables are simpler.  MICROPUB_SITE_GITHUB_REPO is just the name of your repo.

At this point, you have deployed webpage-micropub-to-github to heroku either by command line or deploy button, so those links on your jekyll site that quill is looking for can now be added to your [_layouts/default](https://github.com/miklb/jekyll-indieweb/blob/prime/_layouts/default.html) file.

![micropub example](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/micropub/micropub_example.png)

Now, when you sign into quill, you should receive this success message.

![successful login first step](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/micropub/successful_login_first_step.png)

When you click authorize, the [indieauth](https://indieweb.org/IndieAuth) process begins.  If you have not added a rel-me link on your site to twitter (or github) with a link back from twitter (or github) to your site, you will receive an error message.

![error no link back to homepage](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/micropub/error_no_link_back_to_homepage.png)

Luckily, this is easy as your jekyll site's rel-me links are generated from the [_config.yml](https://github.com/miklb/jekyll-indieweb/blob/prime/_config.yml) file when you specify twitter_username and/or github_username.

When you post to your site from quill, don't be surprised if receive "Page not found" because your browser redirected you before your jekyll repo updated your site.

![micropub success](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/micropub/micropub_success.png)

Now you can make posts (with images) from your mobile device to your jekyll site!  It's not too difficult to have your location displayed in the footer of your post as I do from time to time like this [post from the Sharks game](http://herestomwiththeweather.com/social/2017/10/18/7533/).
