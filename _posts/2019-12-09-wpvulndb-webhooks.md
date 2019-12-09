---
layout: post
title:  "New WPVulnDB Webhooks"
categories: wpvulndb
---

# New WPVulnDB Webhooks

We have just launched a new feature on our [WordPress Vulnerability Database](https://wpvulndb.com/) that will allow Enterprise API users to configure a Webhook that will be triggered every time a new vulnerability is added to our database.

This has been a much requested feature by our Enterprise users and we are happy to be able to supply a solution.

The new Webhook feature will allow Enterprise users to rely on us telling them when we add a new vulnerability, rather than them having to continually check if there are any new vulnerabilities in our database.

The new Webhook functionality is available from Enterprise users' profile pages and it looks like this:

![WPVulnDB Webhooks](/assets/posts/wpvulndb-webhooks/wpvulndb_webhooks.png)

When a Webhook is configured, we will send a POST request with all the vulnerability data in JSON format, every time a new vulnerability is added to our database. We will also continually check the status of the Webhook to ensure that it is working. To know that it is working we expect the Webhook to respond with a `200` HTTP status code. If we find that your configured Webhook is not working, we will send you an email to inform you.

That's all for now. We hope that the new functionality is useful and we appreciate any feedback.

If you want to become a WPVulnDB API Enterprise user, [contact us](https://wpvulndb.com/contact).
