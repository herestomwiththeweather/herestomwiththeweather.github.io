---
title: "IndieAuth with Yubikey"
layout: post
date: 2017-09-24 17:35:39
---
On Friday, Stina Ehrensvärd blogged [Firefox Nightly enables support for FIDO U2F Security Keys](https://www.yubico.com/2017/09/firefox-nightly-enables-support-fido-u2f-security-keys/).  She notes that "in the near future, 80% of the world’s desktop users...will benefit from the open authentication standard and YubiKey support out of the box."

Strong authentication support in the browser is important, not just for private high value transactions but also for public authenticity.  Your identity need not be confined to one website that supports strong authentication.   You can easily use strong authentication when logging into to any website that supports [IndieAuth](https://indieauth.com/) with your own domain name as your login identifier using your [yubikey device](https://www.yubico.com/products/yubikey-hardware/) instead of just using a password or a weak 2nd factor.  Although people viewing your activity or content on that site cannot confirm that you used strong authentication, you can greatly reduce the risk that you will be impersonated.  Impersonation isn't a big problem until it happens.

Let's see how this works with Github which [supports yubikeys](https://www.yubico.com/support/knowledge-base/categories/articles/use-yubikey-github/).  To [set up Indieauth](https://indieauth.com/setup) on your personal domain, include a [rel-me](http://indieweb.org/rel-me) link to your github url.  If you view source right now, you'll find the link I use:

	<a href="https://github.com/herestomwiththeweather" class="icon github" title="GitHub" rel="me">

Then, make sure there is a link back from your github page to your personal domain.  At this point, you should be able to use Indieauth with your personal domain.  

If you haven't already configured 2 factor auth for Github, you'll first need to do that either with [SMS](https://help.github.com/articles/configuring-two-factor-authentication-via-text-message) or a [mobile app](https://help.github.com/articles/configuring-two-factor-authentication-via-a-totp-mobile-app/).  After that you can add your yubikey device by visiting [https://github.com/settings/security/](https://github.com/settings/security/):

![pic](https://s3.amazonaws.com/coffeebucks/yubikey1.png)

If you click the edit button on the right under two-factor authentication, you can register a new device in security keys:

![pic](https://s3.amazonaws.com/coffeebucks/yubikey2.png)

You are asked to give the device a name:

![pic](https://s3.amazonaws.com/coffeebucks/yubikey3.png)

Then, with it plugged into a usb port, register your yubikey by tapping it:

![pic](https://s3.amazonaws.com/coffeebucks/yubikey4.png)

Your yubikey has been succesfully registered:

![pic](https://s3.amazonaws.com/coffeebucks/yubikey5.png)

Now let's log into the woodwind.xyz indieweb reader:

![pic](https://s3.amazonaws.com/coffeebucks/yubikey6.png)

Click login to sign in with indieauth:

![pic](https://s3.amazonaws.com/coffeebucks/yubikey7.png)

Let's use strong authentication with a yubikey so click on the green button that says github.com.

![pic](https://s3.amazonaws.com/coffeebucks/yubikey8.png)

For this example, the browser is not logged into github yet, so both a password and yubikey are required to log into github.  After providing a password, github redirects us to a page that requires pressing the usb attached yubikey.

![pic](https://s3.amazonaws.com/coffeebucks/yubikey9.png)

Github returns an OAuth token to IndieAuth.com:

![pic](https://s3.amazonaws.com/coffeebucks/yubikey10.png)

And we're logged into woodwind.xyz!

![pic](https://s3.amazonaws.com/coffeebucks/yubikey11.png)


