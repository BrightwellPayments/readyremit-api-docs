---
title: About the API
excerpt: ""
slug: about-the-api
category: 6202c91258ac9600635fb56b
---

# Data Types

### Currency

An amount of money is represented by an object consisting of two properties: `value` and `currency`. The value of the `currency` key is an object consisting of four properties: `name`, `iso3Code`, `symbol`, and `decimalPlaces`.

``` json
{
  "value": 100000,
  "currency": {
    "name": "US Dollar",
    "iso3Code": "USD",
    "symbol": "$",
    "decimalPlaces": 2
  }
}
```

The object above represents $1000.00. The *value* is always an integer. It is never a decimal. You calculate the amount like this: `value / decimalPlaces * 10`. An amount of money often appears in a response body as the value of a key such as `sendAmount`, `receiveAmount`, and `totalCost`. Here is an example:

``` json
{
  ...
  "sendAmount": {
    "value": 100000,
    "currency": {
      "name": "US Dollar",
      "iso3Code": "USD",
      "symbol": "$",
      "decimalPlaces": 2
    }
  }
  ...
}
```

### Date and Time

All dates and times are represented in [Coordinated Universal Time (UTC)](https://en.wikipedia.org/wiki/ISO_8601#Coordinated_Universal_Time_(UTC)).

# Status Codes

### 200 OK

Successful GET and PUT operations return this status code.

### 201 Created

Successful POST operations return this status code.

### 204 No Content

Successful DELETE operations return this status code.

### 400 Bad Request

``` json
{
  "code": "string",
  "message": "string",
  "fieldId": "string"
}
```

* `code` is useful to Brightwell Support.
* `message` is meant to be useful to the end user.
* `fieldId` is optional. It identifies the offending json field.

### 401 Unauthorized

This status code means the request has an invalid access token.

### 403 Forbidden

This status code means the request has a valid access token, but the caller is not authorized to execute the operation.

### 404 Not Found

*GET*, *PUT*, and *DELETE* operations that use an id to specify a particular resource (e.g. sender, receiver) return this status code when the service cannot match the id to a resource.

### 405 Method Not Allowed

This status code means the specified endpoint does not support the specified method (e.g. PATCH).

### 500 Internal Server Error

``` json
[
  {
    "code": "string",
    "message": "string",
    "fieldId": "string"
  }
]
```

* `code` is useful to Brightwell Support.
* `message` is meant to be useful to the end user.
* `fieldId` is optional. It identifies the offending json field.
