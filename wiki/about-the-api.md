---
title: About the API
excerpt: ""
slug: about-the-api
category: 6202c91258ac9600635fb56b
---

# Status Codes

### 200 OK

Successful GET and PUT operations return this status code.

### 201 Created

Successful POST operations return this status code.

### 204 No Content

Successful DELETE operations return this status code.

### 400 Bad Request

```
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

### 500 Internal Server Error

```
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
