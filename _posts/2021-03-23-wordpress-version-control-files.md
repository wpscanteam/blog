---
layout: post
title: "WordPress Version Control Files"
---

### What are version control files?

When developers write code they often use version control software, such as [SVN](https://subversion.apache.org/) or [Git](https://git-scm.com/), to help manage their work.

When version control software is used, it often uses a hidden folder to store data about the source code being written. As this folder is hidden, it often can't be viewed and therefore inadvertently ends up on your website.

The most common folders are named:

- `.svn`
- `.git`

What happens if an attacker is able to view one of these version control folders?

### What are the security risks with version control folders?

As the files contain data about the site's source code, in some cases, an attacker can have access to all of your site's code.

That may include hard coded passwords (secrets). It may reveal pages that you thought were hidden from public. It could even allow the attacker to look for security vulnerabilities within the code to further exploit the website.

### How to check if your website has version control files exposed?

#### WPScan WordPress Plugin

Our [WordPress security plugin](https://wordpress.org/plugins/wpscan/) will show a warning if the website exposes the `.svn` or `.git` folders.

### Conclusions

Version control is used by developers to help them do their jobs. Sometimes version control files can end up on your website. If they do end up on your website, they could give an attacker a copy of your website's code.

If found, it is recommended that these files be deleted from yuor website.
