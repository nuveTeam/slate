---
title: Nuve API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='/schema.html'>API schema</a>

includes:
  - errors

search: true
---

# Introduction

## Summary
The Nuve Asset Protection API adheres to REST design principals. URLs aim to be predictable, resource oriented, use
standard HTTP verbs, and return standard HTTP status codes to indicate success or failure.

JSON encoding is used for request and response payloads.

## Response format

> A successful request and response

```json
{
  "data": []
}
```

> A problem with the request or response

```json
{
  "error": {}
}
```

All JSON response formats take has one of the following root elements:

* `data`: indicates a successful request and response. The data attribute will contain the requested resource(s).
* `error`: indicates a problem with the request or the server encountered an issue generating the response. See [errors](#errors) for more info.


## Date & time

Date and time are specified and returned in ISO 8601 format:

`YYYY-MM-DDTHH:MM:SSZ`

## Coordinates

Coordinates are always specified in a tuple of either two values, meaning longitude and latitude, or three values
which specifies longitude, latitude and altitude.

* Longitude: A value between -180 and +180 which represents the east-west position of a point on the Earth's surface.
* Latitude: A value between -180 and +180 which represents the north-south position of a point on the Earth's surface.
* Altitude: Specified in kilometers above sea level

### Longitude and latitude

`[45.850875, -84.62456]`

### Longitude, latitude, and altitude

`[45.850875, -84.62456, 657.95]`

# API schema

The Nuve API has a schema provided in [Swagger](http://swagger.io/) format. The schema can be utilized
to generate API clients in many different languages utilizing the [Swagger Editor](http://editor.swagger.io/). Additional
API endpoints are also available, above and beyond what is disccused in this documentation, and available for browsing [here](/schema.html).

# Authentication

> To authorize, use this code:

```shell
curl "api_endpoint_here"
  -u "nuve.api.token:"
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
  -u "nuve.api.token:"
```

> Filter the returned events by an asset with ID `1`:

```shell
curl "https://api.nuve.us/v/events?filter[asset]=1"
  -u "nuve.api.token:"
```

> The above command returns JSON structured as follows:

```json
{
  "data": [
    {
      "id": "1",
      "type": "location",
      "attributes": {
        "generated": "2015-06-03T22:56:14.256Z",
        "location": {
          "coordinates": [45.850875, -84.62456]
        },
        "gps": {
          "fixTime": "2015-06-03T22:56:14.256Z"
        },
        "metrics": {
          "nuve.fuel.left": 1
        },
        "created": "2015-06-03T22:56:14.148Z"
      },
      "relationships": {
        "asset": {
          "links": {
            "related": "https://api.nuve.us/v/events/1/asset"
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
filter[created] | If provided, the ISO-8601 time interval for which events will be included in the response, based upon their creation date and time
filter[generated] | If provided, the ISO-8601 time interval for which events will be included in the response, based upon their generated date and time

## Get a specific event

```shell
curl "https://api.nuve.us/v/events/3"
  -u "nuve.api.token:"
```

> The above commands returns JSON structured as follows:

```json
{
  "data": [
    {
      "id": "3",
      "type": "location",
      "attributes": {
        "generated": "2015-06-03T22:56:14.256Z",
        "location": {
          "coordinates": [45.850875, -84.62456]
        },
        "gps": {
          "fixTime": "2015-06-03T22:56:14.256Z"
        },
        "metrics": {
          "nuve.fuel.left": 1
        },
        "created": "2015-06-03T22:56:14.148Z",
        "modified": "2015-06-03T22:56:14.148Z"
      },
      "relationships": {
        "asset": {
          "links": {
            "related": "https://api.nuve.us/v/events/1/asset"
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

### HTTP request

`GET https://api.nuve.us/v/events/{id}`

### URL parameters

Parameter | Description
--------- | -----------
ID | The ID of the event to retrieve

## Create an event

```shell
curl -d \
'{
    "data": [
      {
        "attributes": {
          "generated": "2015-06-03T22:56:14.256Z",
          "location": {
            "coordinates": [45.850875, -84.62456],
            "speed": 45.6,
            "heading": 180.1
          },
          "gps": {
            "fixTime": "2015-06-03T22:56:14.256Z"
          },
          "metrics": {
            "nuve.fuel.left": 1
          }
        },
        "relationships": {
          "asset": {
            "data": {
              "type": "asset",
              "id": "1"
            }
          }
        }
      }
    ]
}' \
-H "Content-Type: application/json" \
-u "nuve.api.token:" \
-X POST https://api.nuve.us/v/events
```

> The above command returns JSON structured like this:

```json
{
  "data": [
    {
      "id": "3",
      "type": "location",
      "attributes": {
        "generated": "2015-06-03T22:56:14.256Z",
        "location": {
          "coordinates": [45.850875, -84.62456],
          "speed": 45.6,
          "heading": 180.1
        },
        "gps": {
          "fixTime": "2015-06-03T22:56:14.256Z"
        },
        "metrics": {
          "nuve.fuel.left": 1
        },
        "created": "2015-06-03T22:56:14.148Z",
        "modified": "2015-06-03T22:56:14.148Z"
      },
      "relationships": {
        "asset": {
          "links": {
            "related": "https://api.nuve.us/v/events/1/asset"
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

This endpoint creates new location events. Multiple events can be provided in one request. Each event requires that
an asset be provided

### HTTP request

`POST https://api.nuve.us/v/events`

### HTTP request body

A JSON formatted payload is required. The root element of the document must be `data`.

#### Attributes

If the data originates from a GPS device, `gps` attributes should be included. If the data was sent over a cellular
connection, `cellular` attributes should be included.

Parameter | Description
--------- | -----------
generated | The [date and time](#date-&-time) at which the event was generated

#### Location attributes

Parameter | Description
--------- | -----------
location.coordinates | Array of longitude, lattitude, and altitude. See [coordinates](#coordinates)
location.speed | Speed of the asset in kilometers per hour (KPH)
location.heading | Heading of the asset in degrees from true north

#### GPS attributes

Parameter | Description
--------- | -----------
gps.fixTime | The [date and time](#date-&-time) at which the GPS fix was aquired
gps.hdop | The GPS horizontal dilution of precision
gps.sattelites | The number of satellites used to acquire a GPS fix

#### Cellular attributes

Parameter | Description
--------- | -----------
cellular.signal | The cellular signal strength in Decibel-milliwatts
cellular.status | Status of the cellular connection, roaming or not roaming
cellular.carrier | The carrier of the cellular data

#### Metrics attributes

Metrics for the asset, including, but not limited to Nuve sensor and sensor group metrics. Unless otherwise noted metrics
represent a binary digital signal, a value of `1` indicates that a sensor group is activated and a status of `0` indicates it is
not:

Metric | Description
-----  | -----------
nuve.fuel.{group} | Status of the fuel sensor group and its orientation, for instance, `nuve.fuel.left` or `nuve.fuel.right`
nuve.switch.{group} | Status of the unlock switch and its group, e.g. `nuve.switch.1`
nuve.battery | Level of the Nuve battery in volts
nuve.bolt.{id} | Status of the Nuve cargo bolt, with the given identifier: `nuve.bolt.0`
nuve.lock.{id} | Status of the Nuve cargo lock permission, a value of `1` indicating permission is granted `0` meaning permission is not granted
ignition | Status of the vehicle ignition
door.cargo | Status of the cargo door

#### Relationships

There is only one required relationship when creating an event, the asset that the event relates to:

```{"asset": {"data": {"type": "asset", "id": "1" }}}```

The above code will link the event to the asset with identifier `1`.

# Assets

## Get all assets

```shell
curl "https://api.nuve.us/v/assets"
  -u "nuve.api.token:"
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
            "related": "https://api.nuve.us/v/assets/1/authorizations"
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
            "related": "https://api.nuve.us/v/assets/1/org"
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
  -u "nuve.api.token:"
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
            "related": "https://api.nuve.us/v/assets/3/authorizations"
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
            "related": "https://api.nuve.us/v/assets/3/org"
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

`GET https://api.nuve.us/v/assets/{id}`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the asset to retrieve

