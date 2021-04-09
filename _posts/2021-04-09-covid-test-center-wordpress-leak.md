---
layout: post
title: "Covid Test Centers Leak Personal Information via WordPress API"
---

Over 14,000 covid test patients were affected by a [data leak](https://www.golem.de/news/coronapandemie-neues-datenleck-bei-corona-testzentren-2104-155604.html) in Germany this week, due to using incremental identifiers in their custom WordPress REST API endpoint.

![Loginizer Plugin](/assets/posts/covid-leak/covid-wordpress.jpg)

According to [the researchers](https://zerforschung.org/posts/eventus-testzentren/), a company in Germany named Eventus Media International (EMI) was found to operate the test centres using customised WordPress installations.

Calling the `/wp-json/wp/v2/registration/` API endpoint would return data in JSON format, including a 10 digit number. Which also happens to be the patient's covid test numbers used to retrieve their test results.

The leaked 10 digit codes could then be entered into the website to retrieve the patient's covid test results, which also included a lot of other personal information, such as the patient's name, address, date of birth, phone number and more.

What's worse is that the 10 digit registration numbers were incremented, so to access the data of another patient, an attacker could just increment the number by one. On top of that, there was no rate limiting in place. Allowing a potential attacker to cycle through all the patient data without anything trying to stop them.

Once made aware of the issue Eventus Media International (EMI) fixed the security vulnerability on the same day. They now require the user's name or email address, as well as the 10 digit code. And all previous codes were changed.

This is the [second time](https://zerforschung.org/posts/medicus/) the researchers have found serious security issues in covid test centre software.

The vulnerability in this instance was not due to WordPress itself, but instead, a custom REST API endpoint. The vulnerabilities were business logic issues, [Insecure Direct Object Reference (IDOR)](https://portswigger.net/web-security/access-control/idor) and the lack of rate-limiting.

WPScan Founder & CEO, Ryan Dewhurst, notes:

> It is very unlikely that a [WordPress security plugin](https://wordpress.org/plugins/wpscan/) would have prevented the attack. Keeping your WordPress websites and their plugins up to date certainly helps, but would not have prevented the attack either. In this case, the only way the attack could have been prevented was better security within the design phase of the software development and subsequent penetration testing.

If you need WordPress penetration testing services, [get in touch](https://wpscan.com/contact). We have a dedicated team of WordPress security professionals each with 10+ years of security testing experience.
