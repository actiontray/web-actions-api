# Domain Web Actions API open specification RFC

<b>Web Actions API</b> is the proposal specification standard for fetching details of actionable items with their state available under given Internet domain.

This is an RFC, if you have some ideas or thoughts regarding it, please submit an issue in this repository.

Web Actions API examplary usage you can find in [the exemplary Next.js with Supabase project](https://github.com/actiontray/nextjs-supabase-events-website).

Web Actions API has 1:1 similarity to [Learn API](https://github.com/learntray/learn-api), for which the exemplary usage you can find in [another exemplary Next.js with Supabase educational project](https://github.com/learntray/nextjs-supabase-learning-website).

## Table of content

- [Objective](#objective)
- [Specification](#specification)
- [Examples](#examples)
- [Specification development](#specification-development)

## Objective

The current Internet and software have created solutions which solved many people problems. Although the observation of the author of this specification has concluded that there has not been yet created specification for communication standard which will fetch details of actionable items with their state available under given Internet domain. These actionable items might represent any type of typical actions performed on behalf of domain e.g. course learning, shop order, delegated task. Users of this API could get knowledge about what new actions have occured on given Internet domains and what are the states of these actions. The control of these actions could not be performed via this API, API could only play representational role of these actions, what is the important assumption which guarantees freedom for domains in performing these actions and in choosing a way of performing them.

## Specification

Specification of `Web Actions API` version `1.0.0-rfc-1`.

Web Actions API can be implemented using `REST API` with the following endpoints:

`/actionapi`

Returns Web Actions API version.

`/actionapi/items`

Returns JSON with actionable items.

`/actionapi/state`

Returns JSON with state of actionable items.

`/actionapi/auth`

Returns JSON with temporary auth token or action token. Once auth token is used, the server should revoke it and another auth token should be requested again in order to use it for receiving action token - auth token is one-time use token, while action token is multi-use token and should be kept in secret. Auth token should be also automatically revoked after short period of time, like 60 seconds, if no used at all.

## Examples

1. Ping to check Web Actions API version

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
      "id": "tutorial.example.com",
      "name": "Website Tutorial",
      "url": "https://example.com/tutorials/example",
      "description": "Example Website Tutorial",
      "categories": [
        "learning"
      ],
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
    },
    {
      "id": "1.orders.example.com",
      "name": "Colorful T-shirt",
      "url": "https://example.com/orders/1",
      "description": "Example Bought Product",
      "categories": [
        "commerce"
      ],
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
      "id": "do-something.tasks.example.com",
      "name": "Task: Do Something",
      "url": "https://example.com/tasks/do-something",
      "description": "Example Task",
      "categories": [
        "management"
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
          "completed": false
        }
      ]
    },
    {
      "userId": "adam",
      "itemId": "1.orders.example.com",
      "steps": [
        {
          "id": "ordered",
          "completed": true
        },
        {
          "id": "paid",
          "completed": false
        },
        {
          "id": "sent",
          "completed": false
        },
        {
          "id": "shipped",
          "completed": false
        }
      ]
    },
    {
      "userId": "john",
      "itemId": "do-something.tasks.example.com",
      "completed": true
    },
    {
      "userId": "adam",
      "itemId": "do-something.tasks.example.com",
      "completed": false
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

5. Query for action token

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

6. Query for action token (creates new one, it signals the need to later on either create new user or attach it to existing user)

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

## Specification development

The specification is developed by [@orzechdev](https://github.com/orzechdev). If you have any questions or feedback please create an issue in this repository.
