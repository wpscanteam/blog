---
layout: post
title:  "CVSS Risk Scores and More"
categories: wpvulndb
---

# CVSS Risk Scores

Since we launched our [WordPress vulnerability database](https://wpvulndb.com/) in 2014, we have been lacking one important factor, vulnerability risk scores. This was partly due to not being able to decide on which risk scoring system to use, not having the time to implement the system, and not having the time to assign risk scores to new vulnerabilities, if the system was implemented.

Today we're happy to announce that all new [WordPress vulnerability database](https://wpvulndb.com/) vulnerabilities will come with a CVSS risk score. However, these will be limited to the API and to Enterprise users for now.

There were many different vulnerability risk scoring systems that we could have chosen from, but in the end we decided to use the most widely accepted and used, the [Common Vulnerability Scoring System (CVSS)](https://www.first.org/cvss/calculator/3.1).

We even ran a poll on Twitter for our users to help us decide, where the top voted CVSS helped finalise our decision:

<blockquote class="twitter-tweet"><p lang="en" dir="ltr">What risk scoring system should be we use for our vulnerabilities?</p>&mdash; WPScan (@_WPScan_) <a href="https://twitter.com/_WPScan_/status/1236986510152552449?ref_src=twsrc%5Etfw">March 9, 2020</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>

So, if you're an Enterprise user, from today, the API will output the CVSS risk score like below for all new vulnerabilities:

```json
{
  "widget-settings-importexport": {
    "friendly_name": "Widget Settings Importer/Exporter",
    "latest_version": "1.5.3",
    "last_updated": "2017-02-01T22:51:00.000Z",
    "popular": false,
    "vulnerabilities": [
      {
        "id": 10180,
        "title": "Widget Settings Importer/Exporter <= 1.5.3 - Authenticated Stored XSS",
        "created_at": "2020-04-15T15:42:26.000Z",
        "updated_at": "2020-04-16T05:00:05.000Z",
        "published_date": "2020-04-15T00:00:00.000Z",
        "description": "\"This flaw allowed an authenticated attacker with minimal, subscriber-level permissions to import and activate custom widgets containing arbitrary JavaScript into a site with the plugin installed.\"",
        "cvss": {
          "score": "7.4",
          "vector": "CVSS:3.1/AV:N/AC:L/PR:L/UI:N/S:C/C:L/I:L/A:L"
        },
        "poc": null,
        "vuln_type": "XSS",
        "references": {
          "url": [
            "https://www.wordfence.com/blog/2020/04/unpatched-high-severity-vulnerability-in-widget-settings-importer-exporter-plugin/"
          ]
        },
        "fixed_in": null
      }
    ]
  }
}
```

You can test the new CVSS JSON output with the following command:

`$ curl -s -H "Authorization: Token token=YOUR_TOKEN" https://wpvulndb.com/api/v3/plugins/widget-settings-importexport | jq`

All vulnerabilities added to the database up until now *will not* have CVSS scores assigned. We hope to one day be able to add CVSS scores to all our of our older vulnerabilities eventually, but this will be a very big undertaking.

From version 3.8.1 of the [WPScan CLI tool](https://wpscan.org/), it will output the CVSS scores in its STDOUT and JSON output, if the API token provided belongs to an Enterprise user. We also hope to implement the scores into our [WordPress Security Plugin](https://wordpress.org/plugins/wpscan/) and [Online WordPress Security Scanner](https://wpscan.io/) in the near future.

# Youtube Video References

We have implemented a new `youtube` reference type that automatically displays the link as a video in our web UI:

![Webhooks on updates](/assets/posts/cvss-scores/youtube.png)


The `youtube` reference type is also output within our API for all users like below, to allow our users to display Youtube videos more easily in their own web UIs:

```json
[..SNIP..]
{
        "id": 9150,
        "title": "Yoast SEO <= 9.1 - Authenticated Race Condition",
        "created_at": "2018-11-20T10:42:14.000Z",
        "updated_at": "2020-04-15T11:10:06.000Z",
        "published_date": "2018-11-20T00:00:00.000Z",
        "description": "According to the changelog,\r\n\r\n\"Race Condition which leads to command execution, by users with SEO Manager roles.\"",
        "cvss": null,
        "poc": null,
        "vuln_type": "RCE",
        "references": {
          "url": [
            "https://plugins.trac.wordpress.org/changeset/1977260/wordpress-seo",
            "https://packetstormsecurity.com/files/150497/",
            "https://github.com/Yoast/wordpress-seo/pull/11502/commits/3bfa70a143f5ea3ee1934f3a1703bb5caf139ffa"
          ],
          "cve": [
            "2018-19370"
          ],
          "youtube": [
            "nL141dcDGCY"
          ]
        },
        "fixed_in": "9.2"
      }
[..SNIP..]
```

You can test the new `youtube` reference output with the following command:

`curl -H "Authorization: Token token=YOUR_TOKEN" https://wpvulndb.com/api/v3/plugins/wordpress-seo`

# More

Users can now update their billing details right from our [WordPress vulnerability database](https://wpvulndb.com/) web UI, as well as upgrade and downgrade subscription plans from the web UI.

We also have some other new improvements and features that we are currently working on, which will be ready in the next few weeks.
