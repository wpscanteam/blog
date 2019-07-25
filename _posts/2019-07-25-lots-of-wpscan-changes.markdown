---
layout: post
title:  "Lots of WPScan CLI Changes"
categories: wpscan
---

## Lots of WPScan CLI Changes

Well, in fact, there is just one change, but it's a big one. Recently we released some big changes to [WPVulnDB](https://wpvulndb.com/), which we [recently blogged about](https://blog.wpscan.org/wpvulndb/2019/07/12/lots-of-wpvulndb-changes.html). Now, we want to tell you about a big change that we are going to be making to the [WPScan CLI tool](https://github.com/wpscanteam/wpscan) in version 3.7.0, which will be released sometime within the next few weeks.

## Vulnerability Data API Tokens

From version 3.7.0 of the WPScan CLI tool, if you want to display vulnerability data, users will need to use and configure an API token to retrieve the latest vulnerability data from the [WPVulnDB API](https://wpvulndb.com/api).

All of the other WPScan CLI tool functionality will work as normal if you don't use or configure a WPVulnDB API token, but when a WordPress version, plugin version, or theme version is detected, you wouldn't get the associated vulnerability data, as that comes from the WPVulnDB API.

Functionality such as WordPress version enumeration, plugin enumeration, theme enumeration, backup file enumeration, weak password brute forcing, Timthumb enumeration, and all of the other WPScan CLI functionality will work as normal, without the need for an API token.

The benefit to users from using the WPVulnDB API is that you will get the vulnerability data immediately after we enter into the database. Right now, with the current implementation, users have to wait until the next morning to use the latest vulnerability data.

The API will allow users to make 50 daily requests free of charge, we expect that this will be plenty for individual blog owners and penetration testers (our original target audience). However, we also offer Paid API usage, which costs €25 per month for up to 250 daily API requests. And if any users require more than 250 daily API requests, we offer enterprise API usage, whose cost will be assessed on an individual basis.

So to break it down, these will be the API usage limits:

- Free usage: 50 requests per day limit
- Paid usage: 250 requests per day limit for €25 per month
- Enterprise usage: Anything more than 250 requests per day, charged on a case by case basis

## Technical Implementation

# Input

When WPScan CLI version 3.7.0 is released in the next few weeks, you will need to [register an account on WPVulnDB](https://wpvulndb.com/users/sign_up) to get your API token.

You can then pass your API token to the WPScan CLI tool in various ways. You can pass the API token via the CLI options by using the `--api-token` option, or you can configure the API token in the `cli_options.yml|.json` file and place the file in the current working directory, or the `~/.wpscan/` directory. Please refer to the [WPScan CLI documentation](https://github.com/wpscanteam/wpscan#usage) for further information.

# Output

The API token functionality will produce the following output from the WPScan CLI.

The API is working, the user has an enterprise plan, the scan made 4 API requests and the user has unlimited requests remaining:

```
[+] WPVulnDB API OK
 | Plan: enterprise
 | Requests Done (during the scan): 4
 | Requests Remaining: Unlimited

[+] Finished: Tue Jul 23 19:31:53 2019
[+] Requests Done: 59
[+] Cached Requests: 6
[+] Data Sent: 12.801 KB
[+] Data Received: 861.299 KB
[+] Memory used: 94.051 MB
[+] Elapsed time: 00:00:02
```

No WPVulnDB API token was provided:

```
[!] No WPVulnDB API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 50 daily requests by registering at https://wpvulndb.com/register.

[+] Finished: Tue Jul 23 20:19:55 2019
[+] Requests Done: 55
[+] Cached Requests: 5
[+] Data Sent: 11.883 KB
[+] Data Received: 856.77 KB
[+] Memory used: 93.523 MB
[+] Elapsed time: 00:00:02
```

And the JSON output will look like the following when an API token was provided:

```json
"vuln_api": {
    "plan": "enterprise",
    "requests_done_during_scan": 3,
    "requests_remaining": "Unlimited"
  }
```

The JSON output will look like the following when no API token is provided:

```json
"vuln_api": {
    "error": "No WPVulnDB API Token given, as a result vulnerability data has not been output.\nYou can get a free API token with 50 daily requests by registering at https://wpvulndb.com/register."
  }
```

# New Status API Endpoint

We will also be releasing a new `status` WPVulnDB API endpoint, which can be used to check your API token validity and API usage, without affecting your daily API limit.

The endpoint will be at:

 - `https://wpvulndb.com/api/v3/status`

 And will return the following for a valid API token:

 ```json
 {
  "success": "true",
  "plan": "free",
  "requests_remaining": 50
}
```

And the following for an invalid API token:

```json
{"error":"HTTP Token: Access denied."}
```

## Conclusions

This will be a big change to the WPScan CLI tool, which will ensure users get the most up to date vulnerability data. It will also help us accommodate users with higher API usage needs. And also help generate income for us to be able to add more vulnerabilities to our [vulnerability database](https://wpvulndb.com/) and improve the quality of our content.

Please note that as the changes have yet to be released, some of the technical specifications may change slightly when actually released.

Don't forget that we will be [deprecating APIv2 on October 1st 2019](https://blog.wpscan.org/wpvulndb/2019/07/05/wpvulndb-apiv2-deprecation.html)!
