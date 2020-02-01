---
layout: post
title:  "WPVulnDB APIv2 Deprecation"
categories: wpvulndb
---

# WPVulnDB APIv2 Deprecation

We released [APIv3](https://wpvulndb.com/api), the successor to APIv2, on March 20th 2018. The new APIv3 requires users to [register a free account](https://wpvulndb.com/users/sign_up) on [wpvulndb.com](https://wpvulndb.com/) and use an API Token to access our API. With the old APIv2, no user registration or API Tokens were required. Requiring API Tokens meant that we could easily identify heavy usage of our API by a particular user, which may have affected other API users, and more easily prevent abuse.

## We will be deprecating APIv2 on October 1st 2019.

On October 1st 2019, APIv2 will no longer be usable. If users want to use our API they will have to [register a free account](https://wpvulndb.com/users/sign_up) on [wpvulndb.com](https://wpvulndb.com/) and use an API Token to access our [API](https://wpvulndb.com/api).

If you are currently using APIv2 then you need to migrate to APIv3 before October 1st 2019. None of the endpoints have changed, just the requirement of an API token. Please refer to the [API documentation](https://wpvulndb.com/api) for technical details on how the API Token should be used. 

If you are already using APIv3, then you do not have to do anything.

Oh, and welcome to our new blog! :)
