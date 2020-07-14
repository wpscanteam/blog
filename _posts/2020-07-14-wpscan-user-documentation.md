---
layout: post
title: "WPScan User Documentation"
categories: wpscan
---

## WPScan User Documentation

This is a copy of the [WPScan User Documentation](https://github.com/wpscanteam/wpscan/wiki/WPScan-User-Documentation). Please refer to the Github Wiki version for the most up to date information.

## Introduction

WPScan is a free, for non-commercial use, black box WordPress security scanner written for security professionals and blog maintainers to test the security of their sites.

WPScan is written in the Ruby programming language. The first version of WPScan was released on the [16th of June 2011](https://blog.dewhurstsecurity.com/2011/06/16/introducing-wpscan-wordpress-security-scanner.html).

## What can WPScan check for?

- The version of WordPress installed and any associated vulnerabilities
- What plugins are installed and any associated vulnerabilities
- What themes are installed and any associated vulnerabilities
- Username enumeration
- Users with weak passwords via password brute forcing
- Backed up and publicly accessible wp-config.php files
- Database dumps that may be publicly accessible
- If error logs are exposed by plugins
- Media file enumeration
- Vulnerable Timthumb files
- If the WordPress readme file is present
- If WP-Cron is enabled
- If user registration is enabled
- Full Path Disclose
- Upload directory listing
- And much more...

## License

WPScan is *not* Open Source software. WPScan is licensed with a custom license that requires a fee to be paid if used commercially. Please find the full license terms [here](https://github.com/wpscanteam/wpscan/blob/master/LICENSE).

## Installation

### Ruby Gem

WPScan is shipped as a Ruby gem, and can be installed with the following command:

`gem install wpscan`

### Docker

We also support Docker. Pull the repo with:

`docker pull wpscanteam/wpscan`

Example Docker command to enumerate usernames:

`docker run -it --rm wpscanteam/wpscan --url https://example.com/ --enumerate u`

### Homebrew (macOS)

`brew install wpscan`

## Updating

### WPScan

To update the WPScan software:

`gem update wpscan`

### Kali Linux

To update WPScan in Kali Linux:

`apt-get update && apt-get upgrade`

### Metadata Data

WPScan keeps a local database of metadata that is used to output useful information, such as the latest version of a plugin. The local database can be updated with the following command:

`wpscan --update`

_Please note that this data does not include the vulnerability data. See [Vulnerability Database](https://github.com/wpscanteam/wpscan/wiki/WPScan-User-Documentation#vulnerability-database) for information on the vulnerability data._

## Enumeration Modes

When enumerating the WordPress version, installed plugins or installed themes, you can use three different "modes", which are:

- passive
- aggressive
- mixed

If you want the most results use the "mixed" mode. However, if you are worried that the server may not be able to handle a large number of requests, use the "passive" mode. The default mode is "mixed", with the exception of plugin enumeration, which is "passive". You will need to manually override the plugin detection mode, if you want to use anything other than the default, with the `--plugins-detection` option.

## Enumeration Options

WPScan can enumerate various things from a remote WordPress application, such as plugins, themes, usernames, backed up files wp-config.php files, Timthumb files, database exports and more. To use WPScan's enumeration capabilities supply the `-e` option.

The following enumeration options exist:

 - `vp`   (Vulnerable plugins)
 - `ap`   (All plugins)
 - `p`    (Popular plugins)
 - `vt`   (Vulnerable themes)
 - `at`   (All themes)
 - `t`    (Popular themes)
 - `tt`   (Timthumbs)
 - `cb`   (Config backups)
 - `dbe`  (Db exports)
 - `u`    (User IDs range. e.g: u1-5)
 - `m`    (Media IDs range. e.g m1-15)

If no option is supplied to the `-e` flag, then the default will be: `vp,vt,tt,cb,dbe,u,m`


## Cheat Sheet

Here we have put together a bunch of common commands that will help you get started quickly.

_NOTE: Get your API token from [wpvulndb.com](https://wpvulndb.com/) if you also want the vulnerabilities associated with the detected plugin displaying._

- For all plugins with known vulnerabilities:

`wpscan --url example.com -e vp --plugins-detection mixed --api-token YOUR_TOKEN`

- For all plugins in our database (could take a very long time):

`wpscan --url example.com -e ap --plugins-detection mixed --api-token YOUR_TOKEN`

- Password brute force attack

`wpscan --url example.com -e u --passwords /path/to/password_file.txt`

### Docker Cheat Sheet

- Pull the Docker repository

`docker pull wpscanteam/wpscan`

- Run WPScan and enumerate usernames

`docker run -it --rm wpscanteam/wpscan --url https://target.tld/ --enumerate u`

- When using `--output` flag along with the WPScan Docker image, a bind mount must be used. Otherwise, the file is written inside the Docker container, which is then thrown away.

```
mkdir ~/docker-bind
docker run --rm --mount type=bind,source=$HOME/docker-bind,target=/output wpscanteam/wpscan:latest -o /output/wpscan-output.txt --url 'https://example.com'
```

The `wpscan-output.txt` file now exists on the host machine at `~/docker-bind/wpscan-output.txt`.

## Vulnerability Database

WPScan uses the [WordPress Vulnerability Database](https://wpvulndb.com/api) API in real time to retrieve known vulnerabilities that affect WordPress core, plugins and themes.

For the vulnerability information to be shown within WPScan you will need to supply an API token with the `--api-token YOUR_TOKEN` option. Alternatively, you can supply the API token from a WPScan configuration file.

A free API token is available, as well as paid plans, depending on your usage needs.

If you do not supply an API token, WPScan will work as normal, with the exception that when a WordPress version, plugin or theme is detected, the associated known vulnerabilities will not be displayed.

## Bypassing Simple WAFs

To bypass some simple WAFs you can try the `--random-user-agent` option.

## Troubleshooting

If WPScan is not working as expected, you can use the `--proxy` option, and use a web proxy to inspect WPScan's HTTP requests, and the remote server's HTTP responses. This is useful when you do not know why you are getting false positives, or false negatives.

## Keeping Informed

We blog here - [https://blog.wpscan.org/](https://blog.wpscan.org/)

We tweet here - [https://twitter.com/\_wpscan\_](https://twitter.com/\_wpscan\_)

