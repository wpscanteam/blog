---
layout: post
title: "WordPress Secret Keys"
---

### What are WordPress Secret Keys?

WordPress secret keys are random long bits of text that are stored in the `wp-config.php` file. They help with encrypting and hashing important data within WordPress. They are used to help secure your authentication cookies and to create secure numbers to protect against attacks.

WordPress have their own [WordPress Secret Key Generator](https://api.wordpress.org/secret-key/1.1/salt/) that will output random secret keys for you, like the ones below:

```
define('AUTH_KEY',         't9fifQwgYZEbMzUjLlUjEV^Lh|WD*Q,1xo/W6xo*cf5P&+0%37t:PlH&c:CvnL-`');
define('SECURE_AUTH_KEY',  'Rgd6n 6saRlcIbCI`-0M,GL %t8O)e%G=*QCi[Z,SD?]Y7@miO:S?vib%|J.!uqF');
define('LOGGED_IN_KEY',    'j@G0N`yDWu[/RY!YnD,>F?6wIzW:?3Z6>DQ&L{KM|F<MU2E(bF;K2<x`+m@_[1#{');
define('NONCE_KEY',        ':*/r8|^STxAsrBOq:}L)U82= ?,ftiQu9p!q>O3_I|DPY0:0qSxI6?&fr5!|MsBm');
define('AUTH_SALT',        'fuhatRk},B>-* x#bzunOm}>x/g|%X~Q*V1@wT-ArVXJ$7-Fhj@+?;:]23+>tn7Q');
define('SECURE_AUTH_SALT', 't#Y%ouo}g5?ug9&;?YUQ=:<VI=;6>{3``Z,ZMr$qP KKm33vU-|`+KAL0[] !d;-');
define('LOGGED_IN_SALT',   '|E^Y+x8e:tDp8JvIZg,ZHs&si?;(<?X0[kMJtE#3[+<-hD#z2dz@+ZF<[#<7!tVC');
define('NONCE_SALT',       '=p:/q9;M=3/$.*jQm9I1HI)[]y >fm+S&4aA5?7E*0MBXi8R?s1Y&-O@>_+6!9ds');
```

Do not use the example given above for your website, instead, ensure that the ones you use are randomly generated.

### What are the security risks with secret keys?

If you use predictable secret keys it weakens the security of your authentication cookies and wherever else they may be used, such as within [Cross-Site Request Forgery (CSRF)](https://owasp.org/www-community/attacks/csrf) nonces.

If WordPress is unable to write to your `wp-config.php` file during installation, WordPress will populate your secret keys with the following text `put your unique phrase here`.

This is very predictable and weakens your site's security. But don't worry about it too much, it's not the end of the world. In most cases, WordPress has already thought about the risks of people using predictable secret keys and implemented further security measures, such as using parts of your password.

That being said, it is always advisable that your secret keys not be the default ones and be random.

### How to check if your website uses the default secret keys?

#### Read your wp-config.php file

If you have access to your website's file system, simply open the wp-config.php file and check that the keys are random and not the default `put your unique phrase here`. 

![WordPress Secret Keys](/assets/posts/secret-keys/wordpress-secret-keys.png)

#### WPScan WordPress Plugin

Our [WordPress security plugin](https://wordpress.org/plugins/wpscan/) will show a warning if the website uses the default secret keys.

![WPScan WordPress Secret Keys](/assets/posts/secret-keys/wpscan-wordpress-secret-keys.png)

### Conclusions

Your WordPress secret keys should be random as they can make important data, such as authentication cookies, less secure if they are predictable. But, if you find you have been using default secret keys by accident for a while, don't worry too much, WordPress has additional security measures built in.
