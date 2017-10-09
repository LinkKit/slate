---
title: LinkKit API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
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

Create a new link. The simplest form of a link simply contains a `url` which will be redirected to. Read the Rules section to understand how you can apply advanced redirect or response logic based on headers or other facts.

### HTTP Request

`POST https://api.linkkit.io/v1/links`

### Body Parameters

Parameter | Description
--------- | -----------
url | The url to redirect to
rules | Link evaluation rules

### Rules

Rules let you define a list of conditions resulting in actions.

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
