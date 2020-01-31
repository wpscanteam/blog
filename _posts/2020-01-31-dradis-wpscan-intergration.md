---
layout: post
title:  "Dradis WPScan Integration"
categories: wpscan
---

# Dradis WPScan Integration

We're happy to announce that WPScan's CLI JSON output can now be seamlessly imported into the [Dradis Framework](https://dradisframework.com/)!

![WPScan Dradis](/assets/posts/dradis/NEW-DRADIS-INTEGRATION.png)

The Dradis Framework is a reporting and collaboration tool for InfoSec teams. With the new WPScan integration, it's never been easier to add WordPress security vulnerabilities to your pentest reports.

To export WPScan's CLI findings in JSON format, ready to be imported into the Dradis Framework, use the following command:

```bash
$ wpscan --url www.example.com -o wpscan_results.json -f json --api-token YOUR_WPVULNDB_API_TOKEN
```

The WPScan Team have used the Dradis Framework many times in the past to save time creating pentest reports and to collaborate with other testers working on the same project.

For further information please refer to:

[https://dradisframework.com/blog/2020/01/dradis-wpscan-integration/](https://dradisframework.com/blog/2020/01/dradis-wpscan-integration/)
