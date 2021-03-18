---
layout: post
title: "WordPress Debug Log Files"
---

### What are debug log files?

When WordPress developers are working on coding a theme or plugin, it is often useful for them to log important data to a file, such as error messages, so that they can view and fix any problems. In WordPress, the debug log file is created with a known file name, `debug.log`, and usually stored in the publicly accessible `/wp-content/` directory.

To enable debug logging in WordPress, the developer has to set the following constants in the `wp-config.php` file:

```
define( 'WP_DEBUG', true );
define( 'WP_DEBUG_LOG', true );
```

These constants should not be enabled when the WordPress website is live in a production environment as they will expose sensitive data to attackers.

### What are the security risks with WordPress debug log files?

As mentioned above, the debug log files are left in a publicly accessible directory on the webserver with a predictable file name and can easily be accessed by an attacker. All the attacker has to do is guess the correct debug log file name and its directory to download the file. And this is easy, as it is usually within the `/wp-content/debug.log` file.

Debug log files can contain all sorts of juicy information that could aid an attacker. This could include server-side directory paths, server errors, usernames, and in extreme cases, plaintext passwords.

Debug log files are so often left exposed that many can be found on Google when using the correct keywords. One redacted snippet can be found below:

```
[20-Feb-2019 15:04:25 UTC] PHP Parse error:  syntax error, unexpected '<' in F:\redacted\www\REDACTED\wp-content\themes\redacted\contents\content-problemas-identificados.php on line 11

[20-Feb-2019 15:04:25 UTC] PHP Stack trace:

[20-Feb-2019 15:04:25 UTC] PHP   1. {main}() F:\redacted\www\REDACTED\index.php:0

[20-Feb-2019 15:04:26 UTC] PHP   2. require() F:\redacted\www\REDACTED\index.php:17

```

### How to check for debug log files

#### WPScan WordPress Security Scanner

Our WPScan command-line interface [WordPress security scanner](https://wpscan.com/wordpress-security-scanner) can detect debug log files from an attacker's outside perspective.

WPScan will check if the `/wp-content/debug.log` file exists by default, for example, with the following command:

```
wpscan --url http://example.com/
```

You can learn more about how to use the WPScan CLI tool from our [user documentation](https://github.com/wpscanteam/wpscan/wiki/WPScan-User-Documentation).

#### WPScan WordPress Plugin

Our [WordPress security plugin](https://wordpress.org/plugins/wpscan/) will check your public directory for any `debug.log` files, and when it detects one it will show a warning.

### Conclusions

WordPress `debug.log` files are often left on websites when in their production environments and can contain sensitive information that could be useful to an attacker.

You can use our [WordPress security scanner](https://wpscan.com/wordpress-security-scanner) and our [WordPress security plugin](https://wordpress.org/plugins/wpscan/) to check for exposed `debug.log` files.
