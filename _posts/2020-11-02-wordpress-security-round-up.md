---
layout: post
title: "WordPress Security Roundup for October 2020"
---

## What's happened this month in the world of WordPress security?

![WordPress Vulnerability Roundup](/assets/posts/roundup-october/header.jpg)

### WPScan

Here at WPScan we launched [our brand new website](https://blog.wpscan.com/2020/10/09/new-wpscan-website.html), which we're super happy with, and feedback so far has been overwhelmingly positive!

We released three new versions of our WPScan [WordPress security scanner](https://github.com/wpscanteam/wpscan), adding the `login-uri` option to specify the `wp-login.php` file location.

We also released two new versions of our [WordPress security plugin](https://wordpress.org/plugins/wpscan/), implementing new features such as the ability to configure the scan time.

We completed a number of WordPress penetration tests for clients, including the WPCloudDeploy plugin, who did a [write up](https://wpclouddeploy.com/wpclouddeploy-v-4-1-0-a-security-focused-update/) about the process and results.

We also welcomed [3,766](https://twitter.com/_WPScan_/status/1323172998870958081) new users to the wpscan.com website!

### WordPress Core

This month we have seen the [WordPress 5.5.2 security release](https://blog.wpscan.com/2020/10/30/wordpress-5.5.2-security-release.html) and subsequent [emergency WordPress 5.5.3 release](https://www.wordfence.com/blog/2020/10/emergency-wp-5-5-3-release/).

WordPress 5.5.2 reportedly patched 10 security issues, including a [Cross-Site Request Forgery (CSRF) vulnerability](https://wpscan.com/vulnerability/10454) from our very own security researcher, Erwan. The vulnerability could allow attackers to change theme backgrounds in certain circumstances.

A day after the 5.5.2 release, WordPress released version 5.5.3, which fixed a bug where WordPress was prevented to be installed on brand new WordPress installations without a database connection.

### WordPress Plugins

In October we added 32 new WordPress plugin vulnerabilities to [our database](https://wpscan.org). Some that stand out are discussed below.

#### Loginizer

![Loginizer Plugin](/assets/posts/roundup-october/loginizer.jpg)

This month we saw the forced update of the [Loginizer WordPress plugin](https://en-gb.wordpress.org/plugins/loginizer/), that was affected by an [unauthenticated SQL Injection vulnerability](https://wpscan.com/vulnerability/10441). You can read the full write up on [ZDNet](https://www.zdnet.com/article/wordpress-deploys-forced-security-update-for-dangerous-bug-in-popular-plugin/), which includes quotes from Ryan from our team. According to the plugin author, 89% of the plugin's installations were successfully updated. With an install base of more than a million websites, this still leaves at least 110,000 websites still vulnerable. Although, we have not witnessed any active exploitation of this vulnerability at the time of writing.

#### SuperStoreFinder

What we have seen is probes for the SuperStoreFinder plugins, that allow [Unauthenticated Arbitrary File Upload](https://wpscan.com/vulnerability/10439). The original vulnerabilities could have first been discovered by an internal penetration test that were mentioned in the changelog. In total there were three plugins affected and all three have been updated to patch the issue.

#### Ninja Forms

![Loginizer Plugin](/assets/posts/roundup-october/ninjaforms.png)

The popular Ninja Forms plugin was affected by a [particularly nasty issue](https://wpscan.com/vulnerability/10424) that allowed arbitrary plugins from the official WordPress repository to be installed by leveraging a Cross-Site Request Forgery (CSRF) vulnerability. This vulnerability was fixed in version 3.4.27.1.

#### WPBakery Page Builder

Wordfence discovered an [Authenticated Stored Cross-Site Scripting (XSS)](https://wpscan.com/vulnerability/10422) security vulnerability within the WPBakery Page Builder WordPress plugin. The vulnerability could allow a low privileged user, such as contributor, to inject malicious JavaScript into posts. This vulnerability was fixed in version 6.4.1.

### WordPress Themes

In October we added 18 new WordPress theme vulnerabilities to [our database](https://wpscan.org). Some that stand out are discussed below.

#### Greenmart

Due to an incomplete fix of [CVE-2020-16140](https://wpscan.com/vulnerability/10444), a reflected Cross-Site Scripting (XSS) attack was still possible by unauthenticated users, by extracting the search_nonce from the source of the homepage and adding it to the original payload. This is possible because WordPress nonces are tied to the logged in user ID, however in the case of unauthenticated users, their ID is always `0` so they will have the same nonce.

#### Real Estate 7

Verion 3.0.4 of the Real Estate 7 theme was found to be affected an [Unauthenticated Reflected Cross-Site Scripting (XSS)](https://wpscan.com/vulnerability/10421) vulnerability, which was fixed but the version number of the theme was not updated.

## How to protect yourself

You should update your WordPress blog to the latest version as soon as possible, including all plugins and themes. Additionally, you can sign up to our email alerts to get instant email notifications about security vulnerabilities in WordPress. You can install our [WordPress security plugin](https://wordpress.org/plugins/wpscan/), or use our [WordPress security scanner](https://wpscan.org/).
