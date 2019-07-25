---
layout: post
title:  "Lots of WPVulnDB Changes"
categories: wpvulndb
---

# Lots of WPVulnDB Changes

Recently we have been working on some big improvements to WPVulnDB, which you will see being released over the next few weeks. Below is a list of the improvements which will impact users the most.

## Paid API Usage

~~Important note: This does not affect the [WPScan CLI tool](https://github.com/wpscanteam/wpscan).~~

We have been giving away unlimited free access for non-commercial use to the WPVulnDB API since 2014. Initially WPVulnDB was created to mostly serve the WPScan CLI tool, but it has since become a full project in its own right. Our APIs are widely used by many individuals and companies around the world. To give an example of API usage, on Friday 28th June, we served 732,000 API requests. That's 8.5 per second, every second, for 24 hours.

To help fund the growing project costs we rely on the money generated charging for commercial use of the WPVulnDB API. As we give free users and commercial users the exact same API access, data and endpoints, it is almost impossible to enforce. We simply do not know if an API request is a commercial one or not. We rely on individual businesses' good will, to inform us that they are using our API commercially. And many businesses have done this, but we suspect that there are many more who simply do not tell us, or may not even know that they are required to pay for commercial use.

Shortly we will be introducing paid API usage for anyone wanting to make more than ~~100~~ 50 daily API requests. Paid API usage will cost €25 per month and will be limited to up to ~~2000~~ 250 daily requests. And anyone who needs more than ~~2000~~ 250 requests per day, will be assessed on an individual basis, much like our current commercial users.

So to break it down, these will be the new API usage limits:

- Free usage: ~~100~~ 50 requests per day limit
- Paid usage: ~~2000~~ 250 requests per day limit for €25 per month
- Enterprise usage: Anything more than ~~2000~~ 250 requests per day, charged on a case by case basis

(All of our current commercial API users will be classed as enterprise users)

We hope that this will allow most users to use our API free of charge, but will also generate some income for the project to be able to make it a little more sustainable for the future, and further increase the amount of data and the quality of data that we provide.

## Latest vulnerabilities API endpoint for paid users

Having API endpoints where users can access the latest WordPress, Plugin and Theme vulnerabilities has been a feature that has been requested a lot from our users.

Shortly we will be releasing these new API endpoints for paid users:

- https://wpvulndb.com/api/v3/all/latest
- https://wpvulndb.com/api/v3/wordpresses/latest
- https://wpvulndb.com/api/v3/plugins/latest
- https://wpvulndb.com/api/v3/themes/latest

## Proof of Concepts (PoC)

We try to provide a Proof of Concept (PoC) with our vulnerabilities wherever possible and with time permitting. We wanted to be able to release a vulnerability's advisory, but delay the release of the PoC, to allow users the time to update. The PoC is very useful for those wanting to create Web Application Firewall (WAF) rules, or for researchers to fully understand the vulnerability at a granular level.

Shortly we will be releasing new functionality that will allow us to automatically control when the PoC is displayed. By default the PoC will not be displayed until 2 weeks after the vulnerability is published on WPVulnDB. But this will be configurable as not all vulnerabilities are the same, we may want to delay releasing the PoC further on some more severe vulnerabilities.

We believe the changes to displaying the PoC on wpvulndb.com will further protect WordPress users, while also not shying away from releasing the full technical details.

## RIPS CodeRisk

[RIPS](https://www.ripstech.com/) is truly an incredible commercial PHP Static Code Analysis tool, which has great support for finding WordPress plugin vulnerabilities. RIPS use their scanner to continuously scan the plugins on the WordPress plugin repository, and assigns each plugin with a code risk based on the scanner's findings. The code risk is supposed to give an idea of how risky some parts of the plugin's code base may be. You can find RIPS' own detailed description [here](https://coderisk.com/about).

Here's an example of our own [WPScan WordPress plugin's](https://wordpress.org/plugins/wpscan/) CodeRisk [https://coderisk.com/wp/plugin/wpscan](https://coderisk.com/wp/plugin/wpscan).

Shortly we will be displaying the CodeRisk badge on each of the WPVulnDB's individual plugin's pages.

The badge looks like this:

[![RIPS CodeRisk](https://coderisk.com/wp/plugin/wpscan/badge "RIPS CodeRisk")](https://coderisk.com/wp/plugin/wpscan)

If you are a WordPress plugin developer, RIPS will supply you with their scan reports free of charge, if your plugin is more than one month old and below 200k Lines of Code. This is a very valuable resource, for free!

## Vulnerability Disclosure Policy

We removed our Vulnerability Disclosure Policy text from our website as it wasn't seeing much use, but we are planning on updating our policy and putting it back on the wpvulndb.com website soon, to be transparent on how we handle vulnerabilities within the database.

## Misc

Apart from the above changes that will be noticeable to end users, we've also made many changes to our backend, which will make it easier for us to manage vulnerabilities in the future. The code that generates the API output will be optimised, to make API requests faster and more efficient. And there will be some slight styling changes here and there across WPVulnDB.

We hope these up and coming changes will make our service better for our users and make it easier for us to manage.

That's it for now!

Oh, and don't forget that we will be [deprecating APIv2 on October 1st 2019](https://blog.wpscan.org/wpvulndb/2019/07/05/wpvulndb-apiv2-deprecation.html)!
