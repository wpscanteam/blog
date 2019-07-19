---
layout: post
title:  "The end of CSRF in WordPress?"
categories: security wordpress
---

# The end of CSRF in WordPress?

The Google Chrome web browser plans to [set the SameSite attribute on all cookies by default](https://groups.google.com/a/chromium.org/forum/#!msg/blink-dev/AknSSyQTGYs/D5Y3BauOBAAJ) in Chrome version 80. Google Chrome by far controls the [largest share](https://netmarketshare.com/browser-market-share.asp?options=%7B%22filter%22%3A%7B%22%24and%22%3A%5B%7B%22deviceType%22%3A%7B%22%24in%22%3A%5B%22Desktop%2Flaptop%22%5D%7D%7D%5D%7D%2C%22dateLabel%22%3A%22Trend%22%2C%22attributes%22%3A%22share%22%2C%22group%22%3A%22browser%22%2C%22sort%22%3A%7B%22share%22%3A-1%7D%2C%22id%22%3A%22browsersDesktop%22%2C%22dateInterval%22%3A%22Monthly%22%2C%22dateStart%22%3A%222018-07%22%2C%22dateEnd%22%3A%222019-06%22%2C%22segments%22%3A%22-1000%22%7D) of the web browser market. Their changes have a significant impact on the Web. It wouldn't be surprising if other major web browsers also followed their lead, implementing the `SameSite` cookie attribute by default too. How is this change going to affect the Web and WordPress security in particular?

## Cookies

HTTP is a stateless protocol. Cookies were later introduced to keep track of this state. The most important part of security when tracking 'state' is authentication. Cookies help web applications determine if a user is authenticated, or not, for every HTTP request your web browser makes.

Cookies are a very juicy target for hackers, as once the attacker has your cookie, they can then login to the target web application as you. They don't even need your username or password. So over the years, additional security measures have been added to cookies, such as the `HttpOnly` and `Secure` attributes, or "flags", as they are sometimes known.

The [HttpOnly](https://www.owasp.org/index.php/HttpOnly) attribute tells the web browser that the cookie value should not be accessed by JavaScript. This helps protect the cookie value from Cross-Site Scripting (XSS) and other similar attacks.

The [Secure](https://www.owasp.org/index.php/SecureFlag) attribute tells the web browser to not send the cookie over unencrypted HTTP. That it should always be sent over encrypted HTTPS. This helps prevent the cookie from being intercepted by Man-in-the-Middle (MitM) attacker.

## What's Cross-Site Request Forgery (CSRF)?

To understand why the `SameSite` attribute is useful we must first understand what a CSRF attack is.

Web browsers have a security mechanism called the Same Origin Policy (SOP), that isolates one website from another, with some exceptions. One of those exceptions is HTML POST forms being able to POST data from one domain to another, including with the user's cookies.

To simplify, [CSRF](https://portswigger.net/web-security/csrf) is when an attacker places an HTML POST form (usually hidden) on one website, that does a POST request to another that does not have CSRF protections in place. The POST request will contain your authentication cookies, so to the target website, the request will look like it came from your browser, and thus act on the request.

One attack that is quite effective is against the user settings page on a web application that allows the user to change their registered email address, without any CSRF protections (including not requiring the user's current password). Using a CSRF attack, an attacker could change the registered email address to their own. They could then use the web application's forgot password functionality to reset the user's password. And then, bam! They have access to the user's account. This is just one example, there are many, many other attacks that can be carried out with CSRF.

## What is the SameSite cookie attribute?

The SameSite cookie attribute is the new kid on the block, being first [introduced in Google Chrome in 2016](https://caniuse.com/#feat=same-site-cookie-attribute). The SameSite attribute was introduced to help prevent Cross-Site Request Forgery (CSRF) attacks.

There are two SameSite attribute values, `strict` and `lax`.

- Strict - Your cookie is never sent cross-domain

- Lax - Your cookie is not sent cross-domain for the riskiest scenarios, such as HTML POST forms

A cookie with a SameSite flag set looks like the following:

```
Set-Cookie: session=abc123; path=/; SameSite=Lax
```

## Affects on WordPress security

As mentioned in the opening lines of this blog post, Google Chrome plans to add the `SameSite=Lax` cookie by default to all cookies. This is going to eradicate the vast majority of CSRF risks within web applications across the web, including WordPress, and more importantly, its plugins. A lot of authenticated Cross-Site Scripting (XSS) vulnerabilities that affect WordPress and its plugins, currently rely on CSRF to deliver the malicious XSS payload. So this will also have an impact on XSS exploitation. Users who do not update their web browsers to versions that set the `SameSite` flag by default could still be the target of a CSRF attack. But over time, we should find more and more users using web browsers with `SameSite` enabled by default.

So, when will Google Chrome version 80 with the `SameSite` flag by default be released? ~~We have no idea. But if you know, let us know, and we'll update this post. The latest Chrome version available for macOS at the time of writing is version 75, if that helps in some way determine how far away version 80 is.~~ Chrome version 80 stable release is [planned to be released on February 4th 2020](https://chromiumdash.appspot.com/schedule).

For further reading on the `SameSite` cookie, you should check out Scott Helme's great blog posts:

- [Cross-Site Request Forgery is dead!](https://scotthelme.co.uk/csrf-is-dead/)
- [Tough Cookies](https://scotthelme.co.uk/tough-cookies/)

And in the meantime, you should check out our [WordPress security scanner](https://wpscan.io/) and our [WordPress plugin vulnerability database](https://wpvulndb.com/). Oh, and our very own [WordPress security plugin](https://wordpress.org/plugins/wpscan/).

