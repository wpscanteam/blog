---
layout: post
title: "WordPress Configuration File Backups"
---

### What are Configuration File Backups?

WordPress has a special file named [wp-config.php](https://wordpress.org/support/article/editing-wp-config-php/) that stores sensitive configuration information for your website.

By default, the `wp-config.php` file stores the following information:

- MySQL settings
- Secret keys
- Database table prefix
- ABSPATH

Developers can also store other sensitive information in the file.

The `wp-config.php` file can be manually backed up, or often times, the file can be automatically backed up by editing software without warning the developer when this is done. This could leave the file and its contents exposed to attackers.

### What are the security risks with Configuration File Backups?

As mentioned above, if a backup copy of the `wp-config.php` file is publicly accessible to attackers, it could expose sensitive configuration information about your website.

This could include your database username and password, which if miss-configured, could allow an attacker to access the entire contents of your database, which could be devastating.

Other sensitive data such as the [WordPress Secret Keys](https://blog.wpscan.com/2021/03/23/wordpress-secret-keys.html), and more, could also be exposed.

### How to check if your website has Configuration File Backups exposed?

#### WPScan WordPress Security Scanner

Our WPScan command-line interface [WordPress security scanner](https://wpscan.com/wordpress-security-scanner) can detect publicly exposed `wp-config` files from an attacker's outside perspective.

The command to run to enumerate publicly exposed `wp-config` files is:

```
wpscan --url example.com -e cb
```

You can learn more about how to use the WPScan CLI tool from our [user documentation](https://github.com/wpscanteam/wpscan/wiki/WPScan-User-Documentation).

#### WPScan WordPress Plugin

Our [WordPress security plugin](https://wordpress.org/plugins/wpscan/) will show a warning if the website exposes any `wp-config.*` files publicly.

### Conclusions

The `wp-config.php` contains sensitive configuration information about your WordPress website and can sometimes be inadvertently publicly exposed.

If exposed, the configuration information leaked could be used to facilitate in further attacks against your website or its users.

If your `wp-config.php` file has been exposed publicly, we recommend that you change your secret keys and database password.
