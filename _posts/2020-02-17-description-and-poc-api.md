---
layout: post
title:  "New Description and PoC fields in API"
categories: wpvulndb
---

# New Description and PoC fields in API

From today we have two new fields output in our API for enterprise users, the `description` and `poc` fields.

We have been displaying this data on the [wpvulndb.com](https://wpvulndb.com/) website since almost the beginning of the project, but excluded the data from the API due to concerns of the extra bandwidth costs.

We have had a number of users request the data be output within the API over the years, and quite a few recently.

A test API response can be found below:

```json
{
  "id": 1,
  "title": "test",
  "created_at": "2020-02-17T08:50:25.000Z",
  "updated_at": "2020-02-17T08:58:50.000Z",
  "published_date": "2020-02-17T00:00:00.000Z",
  "description": "THIS IS A TEST DESCRIPTION\r\n\r\nTHIS IS A TEST DESCRIPTION 2\r\n\r\nTHIS IS A TEST DESCRIPTION 3",
  "poc": "<script>alert(1)</script>\r\n\r\n<script>alert(1)</script>\r\n\r\n<script>alert(1)</script>",
  "vuln_type": "TRAVERSAL",
  "references": {},
  "plugins": {
    "test": {
      "fixed_in": null
    }
  },
  "themes": {},
  "wordpresses": {}
}
```

As you can see, `\r\n\r\n` are used for newlines in the new `description` and `poc` fields. If they are empty, they will return `null` as their values.

If we have a Proof of Concept (PoC) but are not going to disclose it until a certain date in the future the `poc` field will contain the following text until the poc release date:

`The PoC will be displayed once the issue has been remediated`

or

`The PoC will be displayed on DATE, to give users the time to update.`

For non-enterprise users the `description` and `poc` fields will be completely emitted.

This data is only available to enterprise users for now, and is available right away at no additional charge.
