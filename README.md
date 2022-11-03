# Domain Learn API open specification RFC

<b>Learn API</b> is the proposal specification standard for fetching details of learning content and user learning state available under given Internet domain.

This is an RFC, if you have some ideas or thoughts regarding it, please submit an issue in this repository.

The working example of Learn API you can find in [the exemplary Next.js with Supabase project](https://github.com/learntray/nextjs-supabase-learning-website).

## Table of content

- [Specification](#specification)
- [Examples](#examples)
- [Specification development](#specification-development)

## Specification

Specification of `Learn API` version `1.0.0-rfc-1`.

Learn API can be implemented using `REST API` with the following endpoints:

`/learnapi`

Returns Learn API version.

`/learnapi/items`

Returns JSON with learning items representing available learning content.

`/learnapi/state`

Returns JSON with learning state for learning items.

`/learnapi/join`

Joins items to start recording their learning state for a given user.

`/learnapi/auth`

Returns temporary auth token or learning token. Once auth token is used, the server should revoke it and another auth token should be requested again in order to use it for receiving learning token - auth token is one-time use token, while learning token is multi-use token and should be kept in secret. Auth token should be also automatically revoked after short period of time, like 60 seconds, if no used at all.

## Examples

1. Ping to check Learn API version

Request:

```http
GET /learnapi HTTP/1.1
```

Response:

```http
HTTP/1.1 200 OK

{
  "learnapi": "1.0.0-rfc-1"
}
```

2. Query for available learing content

Request:

```http
GET /learnapi/items HTTP/1.1
Authorization: Bearer <optional-learning-token>
```

Response:

```http
HTTP/1.1 200 OK

{
  "learnapi": "1.0.0-rfc-1",
  "items": [
    {
      "id": "introduction.example.com",
      "name": "Website Introduction Article",
      "url": "https://example.com/introductions/example",
      "description": "Example Introduction Article"
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

3. Query for learning items state

Request:

```http
GET /learnapi/state HTTP/1.1
Authorization: Bearer <optional-learning-token>
```

Response:

```http
HTTP/1.1 200 OK

{
  "learnapi": "1.0.0-rfc-1",
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
GET /learnapi/auth HTTP/1.1
Authorization: Bearer <learning-token>
```

Response:

```http
HTTP/1.1 200 OK

{
  "learnapi": "1.0.0-rfc-1",
  "authToken": "<auth-token>"
}
```

5. Query for learning token

Request:

```http
GET /learnapi/auth HTTP/1.1
Authorization: Bearer <auth-token>
```

Response:

```http
HTTP/1.1 200 OK

{
  "learnapi": "1.0.0-rfc-1",
  "learningToken": "<learning-token>"
}
```

6. Query for learning token (creates new one, it signals the need to later on either create new user or attach it to existing user)

Request:

```http
GET /learnapi/auth HTTP/1.1
```

Response:

```http
HTTP/1.1 200 OK

{
  "learnapi": "1.0.0-rfc-1",
  "learningToken": "<learning-token>"
}
```

7. Query to join learning items

Request:

```http
POST /learnapi/join HTTP/1.1
Authorization: Bearer <learning-token>

{
  "learnapi": "1.0.0-rfc-1",
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
  "learnapi": "1.0.0-rfc-1",
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

For previous deprecated `learnme.json` specification file please go [here](LEARNME.md).
