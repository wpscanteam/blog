---
layout: post
title: "Is WordPress XMLRPC a security problem?"
---

### What is WordPress XMLRPC?

WordPress XMLRPC allows other websites and software to interact with your WordPress website. Also known as an API. Some examples include creating new posts, adding comments, deleting pages and probably most commonly used in WordPress, pingbacks.

As the name suggests, XMLRPC works by sending and receiving XML data. In WordPress, the file responsible for XMLRPC is called xmlrpc.php. This is the file that will receive XML data, process it and return the response, also in XML.

### What does an XML-RPC request look like?

A typical API request body looks like the following:

```xml
<?xml version="1.0" encoding="iso-8859-1"?>
<methodCall>
  <methodName>demo.sayHello</methodName>
  <params>
   <param></param>
  </params>
</methodCall>
```

The xmlrpc.php file needs the valid XML sent to it as a POST request. The easiest way to do this in Linux is to use cURL. The following command will send the XML contained within the 'demo.sayHello.txt' file as a POST request to the remote WordPress API:

```sh
curl --data @demo.sayHello.txt http://www.example.com/xmlrpc.php
```

Which should return a response that looks like this:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<methodResponse>
  <params>
    <param>
      <value>
      <string>Hello!</string>
      </value>
    </param>
  </params>
</methodResponse>
```

### What are the security risks with leaving WordPress XMLRPC enabled?

Over the years there have been many security issues that have affected the WordPress XMLRPC API. A quick search on [wpscan.com](https://wpscan.com/) shows the following vulnerabilities:

![XML-RPC Search on wpscan.com](/assets/posts/xmlrpc/xmlrpc-wordpresss-vulnerabilities.png)

The vulnerabilities go as far back as [WordPress 1.5.1.2](https://wpscan.com/wordpress/1512) and include [SQL Injection vulnerabilities](https://wpscan.com/vulnerability/7a81eb8e-a536-4b51-aebb-10bfbfaa93d7), [Server-Side Request Forgery (CSRF) vulnerabilities](https://wpscan.com/vulnerability/21079a9f-7256-4ec0-b93d-44b489720cde), [Denial of Service (DoS) vulnerabilities](https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/) and others.

### How to disable XML-RPC

There are many security plugins available that will attempt to disable WordPress's XML-RPC interface, such as the [Disable XML-RPC](https://wordpress.org/plugins/disable-xml-rpc/) plugin. However, we found out during implementing the XMLRPC check in our own [WordPress security plugin](https://wordpress.org/plugins/wpscan/), that many of the plugins that claim to disable XMLRPC don't do so completely. Instead, they only prevent authentication on the XMLRPC interface, which only blocks access to the authenticated methods, leaving the unauthenticated methods still publicly accessible.

Disabling authentication on the XMLRPC interface does decrease the attack surface significantly, as most of the dangerous functions require authentication. But this still leaves the unauthenticated methods wide open, and we have seen very serious vulnerabilities affect the unauthenticated methods in the past, such as the [pingback Server-Side Request Forgery vulnerability](https://www.tenable.com/plugins/nessus/64453).

The only way to be 100% sure that access to the xmlrpc.php file is completely blocked is to do so from the webserver configuration. Some examples for the most popular webservers are given below.

#### Nginx

To block access to xmlrpc in nginx use the following configuration:

```
location = /xmlrpc.php {
    deny all;
    return 404;
}
```

#### Apache

If you have access to your main Apache configuration file, use the code below there. Alternatively, create a file named `.htaccess` in your WordPress directory with the following contents:

```xml
<Files "xmlrpc.php">  
  Require all denied
</Files>
```

### Checking if XML-RPC is disabled

Our [WordPress security plugin](https://wordpress.org/plugins/wpscan/) will detect if XMLRPC is enabled or not. Our plugin will also go as far as testing if both authenticated and unauthenticated access is blocked, or not. As we mentioned above, most plugins will still allow unauthenticated methods, which have been known to be affected by serious security issues in the past.

We will show this warning if XMLRPC is completely enabled (both authenticated and unauthenticated):

![WordPress XMLRPC enabled](/assets/posts/xmlrpc/wpscan-xmlrpc-enabled.png)

We will show this warning if XMLRPC is partially disabled (still allows unauthenticated methods):

![WordPress XMLRPC enabled](/assets/posts/xmlrpc/wpscan-xmlrpc-enabled-2.png)


### Conclusions

When enabled, XMLRPC increases your WordPress website's attack surface, as hackers have more "windows" to try to break through. We can be pretty confident that in the latest version of WordPress that XMLRPC is secure enough. That being said, we do recommend that it be disabled with webserver configurations, as in the majority of cases, WordPress XMLRPC is hardly used.

Some plugins may claim that they have disabled the XML-RPC interface, but in some cases, this can be misleading, and leave unauthenticated methods accessible.

To check if your WordPress XML-RPC is properly disabled, run a free scan with our [WordPress security plugin](https://wordpress.org/plugins/wpscan/).
