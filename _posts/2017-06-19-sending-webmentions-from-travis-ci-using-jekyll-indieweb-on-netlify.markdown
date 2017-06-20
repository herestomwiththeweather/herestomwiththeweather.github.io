---
title: "Sending webmentions from Travis CI using Jekyll-Indieweb on Netlify"
layout: post
date: 2017-06-19 10:50:34
---
Webmention is a [W3C recommendation](https://www.w3.org/TR/2017/REC-webmention-20170112/) that is useful for notifying a website that you have linked to it.  It can also enable comments, rsvps or conversations.  After multiple iterations and some good advice from the Indieweb community, I chose the approach I'm describing here for my own blog.  I'm interested in how to make this simpler.

First, I will describe setting up Jekyll-Indieweb on Netlify with a custom domain and https.  Then, we can use an [experimental branch](https://github.com/code12atx/jekyll-indieweb/tree/travis_ci) of Jekyll-Indieweb for the purpose of demonstrating how to send webmentions with Travis-CI.

To get started, click Github's "Fork" button on the [demo repository](https://github.com/code12atx/jekyll-indieweb).  As a public open source github repo, Travis-CI will not charge you anything to use their service.

Once you have forked the jekyll-indieweb project to your own github account name, visit [https://app.netlify.com/start](https://app.netlify.com/start) after registering a Netlify account.

![Create a new site](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_step1.png)

Check the checkbox for limiting access.  After choosing Github, you are asked to authorize Netlify with limited access to your Github resources.

![Consent](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_consent.png)

Note that Netlify is asking for write access to commit status but that seems okay.  After consenting to the authorization, let's take the default settings shown below.  When we're ready to try out the webmention experimental branch, we can update the branch choice. 

![Configure site](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_step_2.png)

Click deploy site!  Now you have a working website with a random sub-domain name.  My site was initially named waiter-spikes-27203.netlify.com.

![Deploy complete](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_deploy_complete.png)

I registered tbbrown.net using namecheap.

![Namecheap](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_namecheap.png)

Back to Netlify, on the Settings tab, I clicked "Configure domain" which allows me to specify my domain name.

![Custom domain](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_custom_domain.png)

Back to Namecheap, I used the Advanced DNS tab on my new domain to configure my CNAME to point to Netlify.  Note that in the screenshot below, I used the alternative setup rather than the recommended setup as described in [Using a Custom Domain](https://www.netlify.com/docs/custom-domains/).

![Advanced DNS](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_namecheap_advanced_dns.png)

Now, you should be able to visit your jekyll-indieweb site using your custom domain.  Let's add a [letsencrypt](https://letsencrypt.org/) certificate.  The Settings helpfully displays a green "Enable HTTPS" button.

![Lets Encrypt](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_ssl.png)

Just click "Let's Encrypt certificate" button and then check the "Force TLS connections" checkbox and we're now ready to set up webmentions.

![Certificate](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_lets_encrypt.png)

Let's use the experimental branch that includes the code to send webmentions.  In Settings, we're going to change the branch Netlify uses for deployment from master to travis_ci.

![Deploy Branch](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_travis_ci_branch.png)

Now, when we push a new commit to the experimental branch, the idea is that Travis CI will be notified and execute a build as specified by the .travis.yml file.  For that to happen, we first need to log into [https://travis-ci.org](https://travis-ci.org) and enable our github repository for CI builds.  Below, in the projects to choose from, the project owner is shown as code12atx but your page will display your github account name instead.  Activate your jekyll-indieweb project as shown below.

![Travis Activate](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_travis_ci_activate.png)

Now that the build has been activated for jekyll-indieweb, we need to set three environment variables in the settings for the activated project on travis-ci for the webmention code to work.  Replace PROJECT_OWNER with your Github account name, PROJECT_NAME should be set to jekyll-indieweb and use your own custom domain (with the feed.xml path) for the RSS_URL like shown below.

![Env variables](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_env_vars.png)

Now, when you push a new commit to the travis_ci branch for your first blog post, a build will run and after the build successfully completes, the .travis.yml file tells travis-ci.org to run the webmention rake task to send webmentions for all the links in the posts since the last build.  Below shows the build output of a new commit that does not include a post so there are no webmentions output.

![Travis output](https://s3.amazonaws.com/coffeebucks/images/image/jekyll/netlify_travis_webmention_output.png)
