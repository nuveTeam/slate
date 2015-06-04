---
title: Nuve API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='http://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

## Summary
The Nuve Asset Protection API adheres to REST design principals. URLs aim to be predictable, resource oriented, use
standard HTTP verbs, and return standard HTTP status codes to indicate success or failure.

JSON encoding is used for request and response payloads.

## Date & time

Date and time are specified and returned in ISO 8601 format:

`YYYY-MM-DDTHH:MM:SSZ`

# Authentication

> To authorize, use this code:

```shell
curl "api_endpoint_here"
  -H "Authorization: nuve.api.token"
```

> Make sure to replace `nuve.api.token` with your API token.

API requests are authenticated by specifying your API key on each request. Provide your API key as the basic auth username.
The password portion is ignored.

* All API requests MUST be made over HTTPS. All other requests will be dropped.
* All requests must be authenticated.

`Authorization: nuve.api.token`

<aside class="notice">
You must replace <code>nuve.api.token</code> with your Nuve API token.
</aside>

# Events

## Get all events

```shell
curl "https://api.nuve.us/v/events"
  -H "Authorization: nuve.api.token"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "id": "1",
      "type": "location",
      "attributes": {
        "location": {
          "coordinates": [45.850875, -84.62456]
        },
        "gps": {
          "fixTime": "2015-06-03T22:56:14.256Z"
        },
        "metrics": {
          "fuel.left": 1
        },
        "created": "2015-06-03T22:56:14.148Z",
        "modified": "2015-06-03T22:56:14.148Z"
      },
      "relationships": {
        "asset": {
          "links": {
            "related": "https://api.nuve.us/events/1/asset"
          },
          "data": {
            "type": "asset",
            "id": "1"
          }
        }
      }
    }
  ]
}
```

This endpoint retrieves all events for all assets in your organizations. The request additionally can be filtered to
only include particular assets over a particular time period.

### HTTP request

`GET https://api.nuve.us/v/events`

### Query parameters

Parameter | Description
--------- | -----------
filter[asset] | Comma seperated list of assets to include in the response
interval | If provided, the ISO-8601 time interval for which events will be included in the response

## Get a specific event

```shell
curl "https://api.nuve.us/v/events/3"
  -H "Authorization: nuve.api.token"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "id": "3",
      "type": "location",
      "attributes": {
        "location": {
          "coordinates": [45.850875, -84.62456]
        },
        "gps": {
          "fixTime": "2015-06-03T22:56:14.256Z"
        },
        "metrics": {
          "fuel.left": 1
        },
        "created": "2015-06-03T22:56:14.148Z",
        "modified": "2015-06-03T22:56:14.148Z"
      },
      "relationships": {
        "asset": {
          "links": {
            "related": "https://api.nuve.us/events/1/asset"
          },
          "data": {
            "type": "asset",
            "id": "1"
          }
        }
      }
    }
  ]
}
```

This endpoint retrieves an event specified by its unique identifier.

### HTTP Request

`GET https://api.nuve.us/v/events/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the event to retrieve

# Assets

## Get all assets

```shell
curl "https://api.nuve.us/v/assets"
  -H "Authorization: nuve.api.token"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "id": "1",
      "type": "asset",
      "attributes": {
        "classification": "truck",
        "name": "Truck 110",
        "slug": null,
        "description": null,
        "created": "2015-06-04T17:30:22.309Z",
        "modified": "2015-06-04T17:30:22.309Z",
        "tags": [],
        "permissions": []
      },
      "relationships": {
        "authorizations": {
          "links": {
            "related": "https://api.nuve.us/assets/1/authorizations"
          },
          "data": [
            {
              "type": "authorization",
              "id": "19"
            }
          ]
        },
        "org": {
          "links": {
            "related": "https://api.nuve.us/assets/1/org"
          },
          "data": {
            "type": "org",
            "id": "17"
          }
        }
      }
    }
  ]
}
```

This endpoint retrieves all assets in your organization. The request can also be filtered to particular organizations
under your organization.

### HTTP request

`GET https://api.nuve.us/v/assets`

### Query parameters

Parameter | Description
--------- | -----------
filter[org] | Comma seperated list of organization IDs to include assets for in the response

## Get a specific asset

```shell
curl "https://api.nuve.us/v/assets/3"
  -H "Authorization: nuve.api.token"
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "id": "3",
      "type": "asset",
      "attributes": {
        "classification": "trailer",
        "name": "Truck 119",
        "slug": null,
        "description": null,
        "created": "2015-06-04T17:30:22.309Z",
        "modified": "2015-06-04T17:30:22.309Z",
        "tags": [],
        "permissions": []
      },
      "relationships": {
        "authorizations": {
          "links": {
            "related": "https://api.nuve.us/assets/3/authorizations"
          },
          "data": [
            {
              "type": "authorization",
              "id": "19"
            }
          ]
        },
        "org": {
          "links": {
            "related": "https://api.nuve.us/assets/3/org"
          },
          "data": {
            "type": "org",
            "id": "17"
          }
        }
      }
    }
  ]
}
```

This endpoint retrieves an asset specified by its unique identifier.

### HTTP Request

`GET https://api.nuve.us/v/events/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the asset to retrieve

