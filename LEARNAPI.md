# Domain Learn API open specification

<b>Learn API</b> is the proposal specification standard for fetching and modifying details of learning content available under Internet domains. Learning content may include e.g. user learning progress within educational resources used at the given website.

## Table of content

- [Specification](#specification)
- [Specification implementation](#specification-implementation)
- [Specification development](#specification-development)

## Specification

Specification of `Learn API` version `1.0.0`.

You can select type of implementation you want to implement. The two possible are `REST API` or `GRAPHQL`.

### `REST API`

`GET /progress resourceId: string`

Endpoint to get user learning progress for given resource. Requires authorisation. Example response:

```json
{
  "ref": "introduction.example.com",
  "version": 1,
  "parts": [
    {
      "ref": "introduction-getting-started",
      "passed": true
    },
    {
      "ref": "introduction-setup",
      "passed": false
    }
  ]
}
```

`POST /progress resourceId: string, input: ProgressInput`

Endpoint to set user learning progress for given resource. Requires authorisation. Example input (marking last part of course as passed):

```json
{
  "ref": "introduction.example.com",
  "version": 1,
  "parts": [
    {
      "ref": "introduction-setup",
      "passed": true
    }
  ]
}
```

### `GRAPHQL`

`query getProgress(resourceId: string)`

Method to get user learning progress for given resource. Requires authorisation. Example response:

```json
{
  "ref": "introduction.example.com",
  "version": 1,
  "parts": [
    {
      "ref": "introduction-getting-started",
      "passed": true
    },
    {
      "ref": "introduction-setup",
      "passed": false
    }
  ]
}
```

`mutation setProgress(resourceId: string, input: ProgressInput)`

Method to set user learning progress for given resource. Requires authorisation. Example input (marking last part of course as passed):

```json
{
  "ref": "introduction.example.com",
  "version": 1,
  "parts": [
    {
      "ref": "introduction-setup",
      "passed": true
    }
  ]
}
```

## Specification implementation

TBA

## Specification development

The specification is founded and maintained by [@orzechdev](https://github.com/orzechdev). In case of needed contact please reach founder directly or create an issue in this repository.
