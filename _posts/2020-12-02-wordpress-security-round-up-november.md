---
layout: post
title: "WordPress Security Roundup November 2020"
---

### WPScan

It's that time of year again where we donate 2% of our profits to a charity that positively impacts climate change, and this year we chose [Sea Shepherd France](https://www.seashepherd.fr/) again. We do this every year as part of our Hack the Planet pledge.

We launched several new versions of our WPScan [WordPress security plugin](https://wordpress.org/plugins/wpscan/), which now contains additional security checks, rather than just the API checks. This included the following checks:

- Check for debug.log files
- Check for wp-config.php backup files
- Check if XML-RPC is enabled
- Check for code repository files
- Check if default secret keys are used
- Check for exported database files

We also made some other small changes and fixed some bugs in the plugin, so make sure you update to the latest version, which is version 1.13.1 at the time of writing. We plan to include many more checks in the up and coming versions and continue to improve the plugin overall. We also passed 5,000+ active installations this month! We hope that we can smash 10,000+ active installations soon!

Our newly designed [WordPress vulnerability statistics](https://wpscan.com/statistics) page went live, which looks much better than the old one and has much more useful data. Take a look at the charts below that show that 87% of WordPress vulnerabilities in our database affect WordPress plugins!

![WordPress vulnerability statistics](/assets/posts/roundup-november/stats.png)

Speaking of new pages, we also have a new [status page](https://status.wpscan.com/) where you can see if Ryan has deployed any changes that broke our servers! ;)

### WordPress Core

Not much to report about WordPress core this month as there have not been any new releases during November. However, the core team have been busy with the imminent release of [WordPress 5.6](https://kinsta.com/blog/wordpress-5-6/), which is scheduled for release on December 8th.

WordPress 5.6 comes with a few new important security features, the first being auto-updates for WordPress core major releases. For now this will be an opt-in feature, which will be disabled by default, however the end goal is to have auto-updates buttery smooth so that they can finally one day be opt-out, rather than opt-in.

Another major new security feature for WordPress 5.6 is Application Passwords. These new passwords will allow users to generate secure passwords for accessing the WordPress REST API and older XML-RPC API. Currently there no plans to implement Application Passwords into the main wp-login.php form.

So we have two new big security features coming to WordPress 5.6. Both a small step in the direction to making WordPress, and its users, more secure.

### WordPress Plugins

This month we added a total of 28 new WordPress plugin vulnerabilities to our database.

This month our vulnerability research team discovered an [actively exploited 0day vulnerability](https://wpscan.com/vulnerability/10471) affecting the AIT CSV Import / Export premium WordPress plugin. On November 12th we saw probes to the `/wp-content/plugins/ait-csv-import-export/admin/upload-handler.php` file within our honeypots. We reviewed the code and discovered an obvious Unauthenticated Arbitrary File Upload vulnerability, amongst others. We informed the vendor but at the time of writing it still does not look like there is a patch available. Although, we have also not seen any more attack probes for it since we publicly exposed the vulnerability on wpscan.com.

BuddyPress, versions less than 6.4.0, were affected by a [Lack of Capability Check on Profile Page](https://wpscan.com/vulnerability/10485) security issue.

The Ultimate Member WordPress plugin, versions less than 2.1.12, were affected by [multiple](https://wpscan.com/vulnerability/10463) [serious](https://wpscan.com/vulnerability/10465) [security issues](https://wpscan.com/vulnerability/10464).

The WPJobBoard premium WordPress plugin, versions less than 5.7.0, were also affected by an [Unauthenticated SQL Injection](https://wpscan.com/vulnerability/10482) and [Unauthenticated Reflected XSS & XFS](https://wpscan.com/vulnerability/10481) vulnerabilities.

For the full list of vulnerabilities that we added to our database during the month of November, we [listed them on our blog](https://blog.wpscan.com/roundup/2020/12/01/november-2020-vulnerability-roundup.html).

### WordPress Themes

We only saw three WordPress theme vulnerabilities this month, affecting the [Winar](https://wpscan.com/vulnerability/10486) and [Love Travel](https://wpscan.com/vulnerability/10468) themes.

For the full list of vulnerabilities that we added to our database during the month of November, we [listed them on our blog](https://blog.wpscan.com/roundup/2020/12/01/november-2020-vulnerability-roundup.html).

## How to protect yourself

Protect your WordPress website from the vulnerabilities discussed above with our WPScan [WordPress security plugin](https://wordpress.org/plugins/wpscan/).

We would also like to recommend a third-party WordPress security plugin developed by our friends over at WP White Security called [WP Activity Log](https://wpactivitylog.com/). It is the most comprehensive WordPress activity log plugin to keep a record of user changes, ease troubleshooting & identify suspicious behaviour early to thwart malicious hacks. Go download and install it and let us know how you found it!
