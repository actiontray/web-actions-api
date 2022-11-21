# Domain Web Action API open specification RFC

<b>Action API</b> is the proposal specification standard for fetching details of actionable items and user actionable items state available under given Internet domain.

This is an RFC, if you have some ideas or thoughts regarding it, please submit an issue in this repository.

The working example of Action API you can find in [the exemplary Next.js with Supabase project](https://github.com/learntray/nextjs-supabase-learning-website).

## Table of content

- [Specification](#specification)
- [Examples](#examples)
- [Specification development](#specification-development)

## Specification

Specification of `Action API` version `1.0.0-rfc-1`.

Action API can be implemented using `REST API` with the following endpoints:

`/actionapi`

Returns Action API version.

`/actionapi/items`

Returns JSON with actionable items representing available actionable content.

`/actionapi/state`

Returns JSON with actionable items state for actionable items.

`/actionapi/join`

Joins items to start recording their actionable items state for a given user.

`/actionapi/auth`

Returns temporary auth token or actionable token. Once auth token is used, the server should revoke it and another auth token should be requested again in order to use it for receiving actionable token - auth token is one-time use token, while actionable token is multi-use token and should be kept in secret. Auth token should be also automatically revoked after short period of time, like 60 seconds, if no used at all.

## Examples

1. Ping to check Action API version

Request:

```http
GET /actionapi HTTP/1.1
```

Response:

```http
HTTP/1.1 200 OK

{
  "actionapi": "1.0.0-rfc-1"
}
```

2. Query for available actionable items

Request:

```http
GET /actionapi/items HTTP/1.1
Authorization: Bearer <optional-action-token>
```

Response:

```http
HTTP/1.1 200 OK

{
  "actionapi": "1.0.0-rfc-1",
  "items": [
    {
      "id": "orders.example.com",
      "name": "Colorful T-shirt",
      "url": "https://example.com/products/colorful-t-shirt",
      "description": "Example Bought Product",
      "steps": [
        {
          "id": "ordered",
          "name": "Ordered"
        },
        {
          "id": "paid",
          "name": "Paid"
        },
        {
          "id": "sent",
          "name": "Sent"
        },
        {
          "id": "shipped",
          "name": "Shipped"
        }
      ]
    },
    {
      "id": "products.example.com",
      "name": "Course Subscription 1 Year Renewal",
      "url": "https://example.com/orders/1-year-subscription",
      "description": "Example Product to Sale"
    },
    {
      "id": "tutorial.example.com",
      "name": "Website Tutorial",
      "url": "https://example.com/tutorials/example",
      "description": "Example Website Tutorial",
      "steps": [
        {
          "id": "step-1",
          "name": "Step 1"
        },
        {
          "id": "step-2",
          "name": "Step 2"
        },
        {
          "id": "step-3",
          "name": "Step 3"
        }
      ]
    }
  ]
}
```

3. Query for actionable items state

Request:

```http
GET /actionapi/state HTTP/1.1
Authorization: Bearer <optional-action-token>
```

Response:

```http
HTTP/1.1 200 OK

{
  "actionapi": "1.0.0-rfc-1",
  "state": [
    {
      "userId": "john",
      "itemId": "introduction.example.com",
      "completed": true
    },
    {
      "userId": "adam",
      "itemId": "introduction.example.com",
      "completed": true
    },
    {
      "userId": "john",
      "itemId": "tutorial.example.com",
      "steps": [
        {
          "id": "step-1",
          "completed": true
        },
        {
          "id": "step-2",
          "completed": true
        },
        {
          "id": "step-3",
          "completed": true
        }
      ]
    },
    {
      "userId": "adam",
      "itemId": "tutorial.example.com",
      "steps": [
        {
          "id": "step-1",
          "completed": false
        },
        {
          "id": "step-2",
          "completed": false
        },
        {
          "id": "step-3",
          "completed": false
        }
      ]
    }
  ]
}
```

4. Query for auth token

Request:

```http
GET /actionapi/auth HTTP/1.1
Authorization: Bearer <action-token>
```

Response:

```http
HTTP/1.1 200 OK

{
  "actionapi": "1.0.0-rfc-1",
  "authToken": "<auth-token>"
}
```

5. Query for actionable token

Request:

```http
GET /actionapi/auth HTTP/1.1
Authorization: Bearer <auth-token>
```

Response:

```http
HTTP/1.1 200 OK

{
  "actionapi": "1.0.0-rfc-1",
  "actionToken": "<action-token>"
}
```

6. Query for actionable token (creates new one, it signals the need to later on either create new user or attach it to existing user)

Request:

```http
GET /actionapi/auth HTTP/1.1
```

Response:

```http
HTTP/1.1 200 OK

{
  "actionapi": "1.0.0-rfc-1",
  "actionToken": "<action-token>"
}
```

7. Query to join actionable items

Request:

```http
POST /actionapi/join HTTP/1.1
Authorization: Bearer <action-token>

{
  "actionapi": "1.0.0-rfc-1",
  "items": [
    {
      "userId": "john",
      "itemId": "introduction.example.com"
    },
    {
      "userId": "john",
      "itemId": "tutorial.example.com"
    }
  ]
}
```

Response:

```http
HTTP/1.1 200 OK

{
  "actionapi": "1.0.0-rfc-1",
  "items": [
    {
      "userId": "john",
      "itemId": "introduction.example.com"
    },
    {
      "userId": "john",
      "itemId": "tutorial.example.com"
    }
  ]
}
```

## Specification development

The specification is developed by [@orzechdev](https://github.com/orzechdev). If you have any questions or feedback please create an issue in this repository.
