---
title: LinkKit API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - rules
  - errors

search: true
---

# Introduction

Welcome to the LinkKit API! You can use our API to create unique redirect links for tracking.

# Authentication

> To authorize, use this code:

```shell
curl "https://api.linkkit.io/v1/links" -u super_secret_key:
```

> Make sure to replace `super_secret_key` with your API key.

LinkKit uses API keys to allow access to the API. You can get a new LinkKit API key [by getting in touch](mailto:admin@linkkit.io).

Authentication to the API is performed with HTTP Basic Auth. Provide your API key as the username value.

# Links

## POST /links

```shell
curl "https://api.linkkit.io/v1/links" \
  -u my_api_key: \
  -X POST \
  -H "Content-Type: application/json" \
  -d '{"url":"https://snaplytics.io"}'
```

> The above command returns JSON structured like exactly like GET /links/:id

Create a new link. The simplest form of a link simply contains a `url` which will be redirected to. Read the [Rules section](#rules) to understand how you can apply advanced redirect or response logic based on headers or other facts.

### HTTP Request

`POST https://api.linkkit.io/v1/links`

### Body Parameters

Parameter | Description
--------- | -----------
url | The url to redirect to
rules | Link evaluation rules

## GET /links/:id

```shell
curl "https://api.linkkit.io/v1/links/3953276c-d9d8-47aa-a420-1019efecc5dd" \
  -u my_api_key:
```

> The above command returns JSON structured like this:

```json
{
  "id": "3953276c-d9d8-47aa-a420-1019efecc5dd",
  "short_id": "a8zD",
  "original_url": "https://snaplytics.io",
  "redirect_url": "http://l.linkkit.io/a8zD",
  "rules": null
}
```

This endpoint retrieves a specific link.

### HTTP Request

`GET https://api.linkkit.io/v1/links/:id`

### URL Parameters

Parameter | Description
--------- | -----------
id | The id of the link to retrieve

## GET /links/:id/stats

```shell
curl "https://api.linkkit.io/v1/links/3953276c-d9d8-47aa-a420-1019efecc5dd/stats" \
  -u my_api_key:
```

> The above command returns JSON structured like this:

```json
{
  "total_visitor_count": 2317238,
  "unique_visitor_count_by_remote_ip": 151282
}
```

This endpoint retrieves stats for a specific link.

### HTTP Request

`GET https://api.linkkit.io/v1/links/:id/stats`

### URL Parameters

Parameter | Description
--------- | -----------
id | The id of the link

## GET /links/:id/visitors

```shell
curl "https://api.linkkit.io/v1/links/3953276c-d9d8-47aa-a420-1019efecc5dd/visitors" \
  -u my_api_key:
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      /*
        Visitor that triggered default action,
        notice no matched_rule_id
      */
      "id": "94991fea-d3ea-4905-acde-f0c07cd4910c",
      "timestamp": 1507652351713,
      "matched_rule_id": null,
      "facts": {
        "method": "GET",
        "headers": {
          "user-agent": "Mozilla/5.0",
          "accept-language": "en-US,en;q=0.8,da;q=0.6"
        },
        "remote_ip": "8.8.8.8"
      },
      "action": {
        "type": "redirect",
        "params": {
          "url": "https://gogle.com"
        }
      }
    },
    {
      /*
       * Visitor triggered a custom action via facts
       * matching one of your defined rules
       */
      "id": "1f9c1b1c-3e9b-40c2-addf-0c7e69bce5f0",
      "timestamp": 1507652351613,
      "matched_rule_id": "unique-rule-1",
      "facts": {
        "method": "GET",
        "headers": {
          "user-agent": "Chrome/5.0"
        },
        "remote_ip": "1.2.3.4"
      },
      "action": {
        "type": "respond",
        "params": {
          "body": "Hello, Dave"
        }
      }
    },
    {...},
    {...},
  ],
  "paging": {
    "previous": null,
    "next": "https://api.linkkit.io/v1/links/3953276c-d9d8-47aa-a420-1019efecc5dd/visitors?after=94991fea-d3ea-4905-acde-f0c07cd4910c&limit=100"
  }
}
```

Retrieves a list of raw visitor data for link. No filters, spam or otherwise, applied. Results are always latest (timestamp) first.

### HTTP Request

`GET https://api.linkkit.io/v1/links/:id/visitors`

### URL Parameters

Parameter | Description
--------- | -----------
id | The id of the link to retrieve

### Query Parameters

Parameter | Description
--------- | -----------
limit | number of items to return, default 100, max 1000
after | return items after this id

## DELETE /links/:id

```shell
curl "https://api.linkkit.io/v1/links/3953276c-d9d8-47aa-a420-1019efecc5dd" \
  -X DELETE \
  -u my_api_key:
```

This endpoint deletes a specified link.

### HTTP Request

`DELETE https://api.linkkit.io/v1/links/:id`

### URL Parameters

Parameter | Description
--------- | -----------
id | The id of the link to delete
