---
layout: post
title: "WordPress Database Backup File Exports"
---

### What are database backup files?

There are many tools and WordPress plugins that allow you to create a backup of your database and export it to a file. Sometimes these backup files can end up in publicly accessible locations and with predictable names, such as `backup.sql`, `database.sql`, `example.com.sql`, and so on.

### What are the security risks with WordPress database backup file exports?

As mentioned above, if these database backup file exports are left in a publicly accessible directory on the webserver with a predictable file name, then they could easily be accessed by an attacker. All the attacker has to do is guess the correct backup file name and its directory to download the file.

Database backup files usually contain the entire contents of the WordPress database. This could include hashed user passwords, private blog posts, the URLs of sensitive media, sensitive customer data, payment details, and so on.

If an attacker can access a backup of your WordPress database, it could result in your website being entirely compromised, or your users seeking litigation due to privacy laws such as the European [General Data Protection Regulation (GDPR)](https://gdpr-info.eu/).

Back in 2018, Ryan Dewhurst, a WPScan founder, [did some security research](http://webcache.googleusercontent.com/search?q=cache%3A%2F%2Fblog.dewhurstsecurity.com%2F2018%2F06%2F07%2Fdatabase-sql-backup-files-alexa-top-1-million.html&oq=cache%3A%2F%2Fblog.dewhurstsecurity.com%2F2018%2F06%2F07%2Fdatabase-sql-backup-files-alexa-top-1-million.html&aqs=chrome..69i57j69i58.606j0j4&sourceid=chrome&ie=UTF-8) to assess the risk of database backup files being exported to publicly accessible locations. During his research he identified 736 publicly accessible database backup files within the Alexa Top 1 Million websites, and that 39% of the affected websites were running WordPress.

### How to check for publicly accessible backup files

#### WPScan WordPress Security Scanner

Our WPScan command line interface [WordPress security scanner](https://github.com/wpscanteam/wpscan) can enumerate backup files from an attacker's outside perspective, as shown below.

![WPScan WordPress Security Plugin](/assets/posts/backup-files/wpscan-wordpress-security-scanner.png)

The command to run to enumerate database backup export files is:

```
wpscan --url http://example.com/ -e dbe
```

You can learn more about how to use the WPScan CLI tool from our [user documentation](https://github.com/wpscanteam/wpscan/wiki/WPScan-User-Documentation).

#### WPScan WordPress Plugin

Our [WordPress security plugin](https://wordpress.org/plugins/wpscan/) will check for 36 different backup file names and locations. When it detects one it will show the warning as seen below.

![WPScan WordPress Security Plugin](/assets/posts/backup-files/wordpress-security-plugin.png)

### Conclusions

If you ever use a tool or WordPress plugin to export your database to a file, make sure that you don't export it to a publicly accessible location. Also ensure that the export file name is random so that it can not be guessed, in the event that it does end up in a publicly accessible location.

You can use our [WordPress security scanner](https://github.com/wpscanteam/wpscan) and our [WordPress security plugin](https://wordpress.org/plugins/wpscan/) to check for exported database files.

If you find that your database was publicly accessible, check your webserver access log files to see if anyone has accessed the file in the past. If someone has accessed the file in the past, you will need to change all of your and your users' passwords, inform your users of the breach, and follow any other security and privacy laws of your specific country, this may involve notifying your [Information Commissioner's Office (ICO)](https://ico.org.uk/).