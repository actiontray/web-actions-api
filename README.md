# Domain Learn API open specification RFC

<b>Learn API</b> is the proposal specification standard for fetching details of learning content and user learning state available under given Internet domain.

This is an RFC, so if you have some ideas or thoughts regarding it, please submit an issue in this repository.

## Table of content

- [Specification](#specification)
- [Examples](#examples)
- [Specification development](#specification-development)

## Specification

Specification of `Learn API` version `1.0.0-rfc-0`.

Learn API can be implemented using `REST API` or `GRAPHQL`.

### REST API

`/learnapi/items`

Returns JSON with items representing available learning content.

`/learnapi/state`

Returns JSON with learning state for learning items returned in `/learnapi/items`.

### GRAPHQL

`/learnapi`

```
items {
  ...
}
```

Returns items representing available learning content.

`/learnapi`

```
state {
  ...
}
```

Returns learning state for learning items returned in items query.

## Examples

### REST API

1. Query for available learing content

Request:

REST API:

```http
GET /learnapi/items HTTP/1.1
Authorization: Bearer <optional-auth-token>
```

GRAPHQL:

```http
GET /learnapi HTTP/1.1
Authorization: Bearer <optional-auth-token>
```

```graphql
query getItems() {
  items {
    id
    name
    url
    description
    completeness
    steps
  }
}
```

Response:

REST API and GRAPHQL:

```http
HTTP/1.1 200 OK
```

```json
{
  "learnapi": "1.0.0-rfc-0",
  "items": [
    {
      "id": "introduction.example.com",
      "name": "Website Introduction Article",
      "url": "https://example.com/introductions/example",
      "description": "Example Introduction Article",
      "completeness": true,
      "steps": null
    },
    {
      "id": "tutorial.example.com",
      "name": "Website Tutorial",
      "url": "https://example.com/tutorials/example",
      "description": "Example Website Tutorial",
      "completeness": false,
      "steps": [
        {
          "id": "step-1",
          "name": "Step 1",
        },
        {
          "id": "step-2",
          "name": "Step 2",
        }
        {
          "id": "step-3",
          "name": "Step 3",
        }
      ]
    },
  ],
}
```

2. Query for learning items state

Request:

REST API:

```http
GET /learnapi/state HTTP/1.1
Authorization: Bearer <optional-auth-token>
```

GRAPHQL:

```http
GET /learnapi HTTP/1.1
Authorization: Bearer <optional-auth-token>
```

```graphql
query getState() {
  state {
    userId
    itemId
    completeness
    steps
  }
}
```

Response:

REST API and GRAPHQL:

```http
HTTP/1.1 200 OK
```

```json
{
  "learnapi": "1.0.0-rfc-0",
  "state": [
    {
      "userId": "john",
      "itemId": "introduction.example.com",
      "completeness": true,
      "steps": null
    },
    {
      "userId": "adam",
      "itemId": "introduction.example.com",
      "completeness": true,
      "steps": null

    },
    {
      "userId": "john",
      "itemId": "tutorial.example.com",
      "completeness": null,
      "steps": [
        {
          "id": "step-1",
          "value": true
        },
        {
          "id": "step-2",
          "value": true
        }
        {
          "id": "step-3",
          "value": true
        }
      ]
    },
    {
      "userId": "adam",
      "itemId": "tutorial.example.com",
      "completeness": null,
      "steps": [
        {
          "id": "step-1",
          "value": false
        },
        {
          "id": "step-2",
          "value": false
        }
        {
          "id": "step-3",
          "value": false
        }
      ]
    },
  ]
}
```

## Specification development

The specification is initially developed by [@orzechdev](https://github.com/orzechdev). If you have any questions or feedback please create an issue in this repository.

For previous deprecated `learnme.json` specification file please go [here](LEARNME.md).
