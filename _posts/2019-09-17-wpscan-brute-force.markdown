---
layout: post
title:  "WPScan Brute Force"
categories: wpscan
---

# Password Brute Force

Password brute forcing is a common attack that hackers have used in the past against WordPress sites at scale. In 2017 Wordfence documented a huge [password brute force attack](https://www.wordfence.com/blog/2017/12/aggressive-brute-force-wordpress-attack/), which saw 14.1 million attacker per hour at its peak.

Attackers are looking for users, preferably administrators, with weak passwords to be able to login to WordPress and compromise the site. Depending on the compromised [user role](https://wordpress.org/support/article/roles-and-capabilities/), once logged in, the attacker could escalate privileges by attacking other users, embed malicious code into the site or compromise the entire server.

## Weak Passwords

Hackers consolidate lists of compromised passwords into one large list with the most common passwords listed first. The original compromised passwords come from the many data breaches from [various organisations](https://haveibeenpwned.com/PwnedWebsites). This increases the attacker's chances as they are no longer guessing random passwords, but using real world data to create a list of the most probable real-world passwords.

One such password list contains the following top 10 passwords:

```
1. 123456
2. password
3. 123456789
4. 12345678
5. 12345
6. qwerty
7. 123123
8. 111111
9. abc123
10. 1234567
```

If you are using any of the passwords above, or anything similar to the above, you should change your password right away.

## WPScan CLI Brute Force

One of the many features of WPScan is password brute forcing. The WPScan CLI (Command Line Interface) tool can be used to iterate over a password list to try to guess a user's password.

To launch a password brute force attack with WPScan CLI, the command looks like this:

`wpscan --url http://test.local/ --passwords passwords.txt`

We pass WPScan the site URL with the `--url` parameter, and the password list, in this case named `passwords.txt`, with the `--passwords` parameter.

![WPScan Brute Force](/assets/posts/wpscan-brute-force/wpscan-brute-force_1.png)

In our case, WPScan automatically found three valid WordPress users (`admin`, `editor` and `author`) and then started to cycle through our password list attempting to login as each of them. As you can see from the screenshot below, WPScan successfully guessed the `admin` user's password, which was `password`.

![WPScan Brute Force](/assets/posts/wpscan-brute-force/wpscan-brute-force_2.png)

## WPScan.io Brute Force

Our [WPScan online WordPress vulnerability scanner](https://wpscan.io/), WPScan.io, will conduct automated password brute force attacks in the `Normal` and `Thorough` scan profiles available to all paid plans. Our Free plan does not include password brute forcing, but it does include some other basic checks free of charge.

When a weak password is found in WPScan.io, it looks like this within your report:

![WPScan Brute Force](/assets/posts/wpscan-brute-force/wpscan-brute-force_3.png)

## Password Brute Force Prevention

The best advice is to not use weak passwords in the first place. Use long and complex passwords that are different for every website that you use. A password manager like [LastPass](https://www.lastpass.com/) is a great tool to help you with this.

You can also use [have i been pwned?](https://haveibeenpwned.com) to see if any of your passwords have already been compromised, or sign up to their email alerts to be notified if a password is leaked in the future.

Reputable WordPress security plugins can also help. We would recommend [Wordfence](https://www.wordfence.com/) or one of the excellent plugins by [WP WhiteSecurity](https://www.wpwhitesecurity.com/).

And of course, you should always verify your security with services such as our [WPScan.io](https://wpscan.io/).
