---
layout: post
title:  "WordPress 5.2.4 Security Release Breakdown"
categories: wordpress security release
---

# WordPress 5.2.4 Security Release Breakdown

Yesterday, October 14th 2019, WordPress released version 5.2.4 as a security release. According to WordPress, WordPress version 5.2.4 fixes 6 security issues.

- [WordPress <= 5.2.3 - Stored XSS in Customizer](https://wpvulndb.com/vulnerabilities/9908)
- [WordPress <= 5.2.3 - Unauthenticated View Private/Draft Posts](https://wpvulndb.com/vulnerabilities/9909)
- [WordPress <= 5.2.3 - Stored XSS in Style Tags](https://wpvulndb.com/vulnerabilities/9910)
- [WordPress <= 5.2.3 - JSON Request Cache Poisoning](https://wpvulndb.com/vulnerabilities/9911)
- [WordPress <= 5.2.3 - Server-Side Request Forgery (SSRF) in URL Validation](https://wpvulndb.com/vulnerabilities/9912)
- [WordPress <= 5.2.3 - Admin Referrer Validation](https://wpvulndb.com/vulnerabilities/9913)

From our own research, we identified that 9 files in this release had been modified.

![Modified Files](/assets/posts/wordpress-524-release/files-modified.png)

## WordPress <= 5.2.3 - Stored XSS in Customizer

This fix is regarding a Stored Cross-Site Scripting (XSS) vulnerability within the WordPress Customizer reported by [Evan Ricafort](https://evanricafort.com/).

The WordPress Customizer allows authenticated users to make changes to the WordPress theme to directly customise the interface. It looks like this:

![WordPress Customizer](/assets/posts/wordpress-524-release/wordpress-customizer.png)

This vulnerability has been added to the WordPress Vulnerability Database here: [WordPress <= 5.2.3 - Stored XSS in Customizer](https://wpvulndb.com/vulnerabilities/9908)

## WordPress <= 5.2.3 - Unauthenticated View Private/Draft Posts

This vulnerability could allow unauthenticated users to view private or draft posts, which otherwise should not be viewable. This issue was reported by [J.D. Grimes](https://codesymphony.co/) to WordPress' bug bounty program on [HackerOne](https://hackerone.com/wordpress).

The related commit can be found [here](https://github.com/WordPress/WordPress/commit/f82ed753cf00329a5e41f2cb6dc521085136f308).

![Static Fix](/assets/posts/wordpress-524-release/static.png)

This vulnerability has been added to the WordPress Vulnerability Database here: [WordPress <= 5.2.3 - Unauthenticated View Private/Draft Posts](https://wpvulndb.com/vulnerabilities/9909)

## WordPress <= 5.2.3 - Stored XSS in Style Tags

This fix patches another Stored Cross-Site Scripting (XSS) vulnerability, this time affecting `style` HTML tags. The [HTML style tag](https://www.w3schools.com/tags/tag_style.asp) is used to add inline CSS to a HTML document. This vulnerability was reported by [Weston Ruter](https://weston.ruter.net/).

This vulnerability has been added to the WordPress Vulnerability Database here: [WordPress <= 5.2.3 - Stored XSS in Style Tags](https://wpvulndb.com/vulnerabilities/9910)

## WordPress <= 5.2.3 - JSON Request Cache Poisoning

This fixes a way to poison the cache of JSON GET requests via the `Vary: Origin` HTTP header.

This has to do with [Cross-Origin Resource Sharing (CORS)](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) and how Content Delivery Networks (CDNs) parse the CORS `Origin` HTTP request header.

James Kettle of Portswigger has written a great blog post on [Practical Web Cache Poisoning](https://portswigger.net/research/practical-web-cache-poisoning) for those who are interested in more in-depth technical details about the attack.

The fix for this issue was to reply with the `Vary: Origin` HTTP response header even if the `Origin` HTTP request header was not white listed. The commit for this fix can be found [here](https://github.com/WordPress/WordPress/commit/b224c251adfa16a5f84074a3c0886270c9df38de).

![Vary Header](/assets/posts/wordpress-524-release/vary-header.png)

This vulnerability has been added to the WordPress Vulnerability Database here: [WordPress <= 5.2.3 - JSON Request Cache Poisoning](https://wpvulndb.com/vulnerabilities/9911)

## WordPress <= 5.2.3 - Server-Side Request Forgery (SSRF) in URL Validation

Server-Side Request Forgery (SSRF) is a vulnerability where an attacker can manipulate a HTTP client into making requests. For example, an attacker may be able to send HTTP requests to the web server's Local Area Network (LAN), or to other websites and services on the Internet.

You can read more about Server-Side Request Forgery (SSRF) on [Portswigger's Web Security Academy](https://portswigger.net/web-security/ssrf).

We believe, but are not 100% sure at this point, that this commit for this fix is [this one](https://github.com/WordPress/WordPress/commit/9db44754b9e4044690a6c32fd74b9d5fe26b07b2).

![WordPress SSRF](/assets/posts/wordpress-524-release/wordpress-ssrf.png)

This vulnerability was reported by [Eugene Kolodenker](https://eugenekolo.com/blog/).

This vulnerability has been added to the WordPress Vulnerability Database here: [WordPress <= 5.2.3 - Server-Side Request Forgery (SSRF) in URL Validation](https://wpvulndb.com/vulnerabilities/9912)

## WordPress <= 5.2.3 - Admin Referrer Validation

This vulnerability affects the [check_admin_referer()](https://developer.wordpress.org/reference/functions/check_admin_referer/) WordPress function. According to the official WordPress documentation it "makes sure that a user was referred from another admin page".

The commit that fixes this issue can be found [here](https://github.com/WordPress/WordPress/commit/b183fd1cca0b44a92f0264823dd9f22d2fd8b8d0).

![Check Admin Referer](/assets/posts/wordpress-524-release/check-admin-referer.png)

As you can see, the change was to change the use of PHP's equal comparison operator `==` to the identical comparison operator `===`. When using the equal comparison operator `==`, PHP uses [type juggling](https://www.php.net/manual/en/language.types.type-juggling.php) where it can assume the variable's type. Whereas the identical comparison operator `===` will ensure both values of the comparison are of the same type.

For further details regarding type juggling vulnerabilities we recommend the [Detailed Explanation of PHP Type Juggling Vulnerabilities](https://www.netsparker.com/blog/web-security/php-type-juggling-vulnerabilities/) by Netsparker.

This issue looks as though type juggling could be exploited to bypass Cross-Site Request Forgery (CSRF) checks.

This vulnerability has been added to the WordPress Vulnerability Database here: [WordPress <= 5.2.3 - Admin Referrer Validation](https://wpvulndb.com/vulnerabilities/9913)

## Conclusions

A varied type of vulnerabilities for this security release. It is difficult to know the severity of these issues without the Proof of Concept (PoC) code. A PoC could be created for each issue with more research, or the original vulnerability researchers themselves may release them in future, once enough WordPress users have updated to version 5.2.4.

Since all of these issues have been added to our [WordPress Vulnerability Database](https://wpvulndb.com/), all of our [WPScan.io](https://wpscan.io/), [WPScan CLI](http://wpscan.org/), [WPVulnDB API](https://wpvulndb.com/api) and [WPScan WordPress Plugin](https://wordpress.org/plugins/wpscan/) users will be alerted.

Read the full official release blog post [https://wordpress.org/news/2019/10/wordpress-5-2-4-security-release/](here).
