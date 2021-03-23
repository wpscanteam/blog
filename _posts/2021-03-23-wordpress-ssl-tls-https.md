---
layout: post
title: "WordPress SSL/TLS HTTPS Encryption"
---

### What is SSL/TLS HTTPS Encryption?

Not so long ago the web's communications were mostly un-encrypted, allowing anyone who could eavesdrop on the traffic to read them. In recent years, the web has seen a dramatic change from mostly being un-encrypted to encrypted.

When your website has HTTPS enabled all communication traffic from your user's computers to your website are encrypted. This prevents any attackers, whether they be in a coffee shop trying to steal payment details, or nation state governments, from reading your user's communications.

Not only does HTTPS offer your users more security, search engines like [Google also rank websites](https://developers.google.com/search/blog/2014/08/https-as-ranking-signal) that use HTTPS higher than those that don't, resulting in more traffic from Google and others.

### What are the security risks with not using HTTPS?

As mentioned above, an attacker could eavesdrop on your user's communications if you don't use HTTPS. The risks can range from someone stealing your user's credit card details while they are making a purchase on your website, to potentially someone being punished for reading content outlawed in their country.

Not only can attacker's read un-encrypted communications, but they can also modify them. So let's say that you are writing a new WordPress blog post at the airport while waiting for your plane (pre-covid!), and an attacker has managed to conduct a [Man-in-the-Middle (MitM) attack](https://en.wikipedia.org/wiki/Man-in-the-middle_attack). The attacker could read and modify your blog post's content, the attacker could steal your authentication cookie and login in as you, they could even inject malicious code into your own website's responses to further exploit your computer.

### How to check if your website is encrypted?

#### Web browser

Most modern web browsers will show a lock icon in the address bar if the connection is encrypted. They may also show warnings if the connection is not encrypted. Browsers are changing these indicators on a regular basis, so the icon that is shown today, or warning given today, may be different in 6 months time. So be careful when trying to interpret these icons.

![WordPress HTTPS](/assets/posts/wpscan-https/wpscan-https.png)

#### WPScan WordPress Plugin

Our [WordPress security plugin](https://wordpress.org/plugins/wpscan/) will show a warning if the website is not using HTTPS.

![WPScan Plugin HTTPS](/assets/posts/wpscan-https/wpscan-plugin-https.png)

### Conclusions

Enabling HTTPS on WordPress today is super easy and can even be done for free!

Sine WordPress version 5.7, WordPress has made it even easier to [migrate from HTTP to HTTPS](https://portswigger.net/daily-swig/wordpress-5-7-offers-one-click-http-to-https-site-upgrade-feature).

Free SSL/TLS certificates can be obtained free of charge from [Let's Encrypt](https://letsencrypt.org/).