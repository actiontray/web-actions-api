# Domain learnme.json open specification (deprecated)

Note: this spec is deprecated in favor of [Learn API](README.md).

<b>learnme.json</b> file is the proposal for describing learning possibilities available under Internet domains. Learning possibilities may include guides, courses, trainings or other educational resources available at the given website.

Specification is adopted by [learntray.com](https://learntray.com) to keep track of users learning progress on Internet websites.

## Table of content

- [Specification](#specification)
- [Specification implementation](#specification-implementation)
- [Specification development](#specification-development)

## Specification

Specification of `learnme.json` version `1.1.0`.

### Specific parameters

| Object                    | Description                                                                                                                                                                                            |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `learnme`                 | learnme file specification version.                                                                                                                                                                    |
| `agreement`               | Agreement for collecting and processing data described in learnme file given for specified individuals.                                                                                                |
| `api`                     | Learn API under which authorised details of learning content can be fetched and modified, details like e.g. user learning progress. Learn API specification proposal can be found [here](LEARNAPI.md). |
| `organisations`           | List of available organisations under given domain.                                                                                                                                                    |
| `organisations.id`        | Unique organisation identifier. By convention format `organisation.domain.com` might be used to allow referencing the organisation on the whole Internet without name collision.                       |
| `authors`                 | List of available authors under given domain.                                                                                                                                                          |
| `resources`               | List of available learning resources (guides, courses or trainings) under given domain.                                                                                                                |
| `resources.id`            | Unique resource identifier. By convention format `resource.domain.com` might be used to allow referencing the resource on the whole Internet without name collision.                                   |
| `resources.version`       | Version of the resource.                                                                                                                                                                               |
| `resources.type`          | `Guide` or `Course`.                                                                                                                                                                                   |
| `resources.categories`    | Categories to which resource can be assigned.                                                                                                                                                          |
| `resources.prerequisites` | Prerequisites which must be learned before learning resource. Must be identifier of resource described in learnme file of the same or other domain.                                                    |
| `resources.owners`        | Authors of the resource. Must be identifier of owner described in learnme file of the same or other domain.                                                                                            |
| `resources.authors`       | Authors of the resource. Must be identifier (email) of author described in learnme file of the same or other domain.                                                                                   |
| `resources.parts`         | Parts of which the resource is composed.                                                                                                                                                               |
| `parts`                   | Parts of resources, referenced in resources.                                                                                                                                                           |

### Common parameters

| Object        | Description                                                                                                                                                                                                                                             |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`        | Name of the thing.                                                                                                                                                                                                                                      |
| `description` | Description of the thing.                                                                                                                                                                                                                               |
| `url`         | Url address under which given thing may be accessed. Url domain must be the same as the current domain name or may be not included at all (meaning that the path is relative to the current domain) - you can't define anything outside of your domain. |

### Example

```json
{
  "actionable": "2.0.0",
  "items": [
    {
      "id": "tutorial.example.com",
      "name": "Website Tutorial",
      "url": "https://example.com/tutorials/example",
      "requiresAuthorisation": false,
      "description": "Example Website Tutorial",
      "prerequisites": [
        "first.guides.dependent-example.com",
        "second.guides.dependent-example.com"
      ],
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
    {
      "id": "auth-tutorial.example.com",
      "name": "Website Authorised Tutorial",
      "url": "https://example.com/tutorials/auth-example",
      "requiresAuthorisation": true,
      "description": "Example Website Requiring Single-Sign-On Tutorial",
      "prerequisites": [
        "first.guides.dependent-example.com",
        "second.guides.dependent-example.com"
      ],
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
    {
      "id": "conference.example.com",
      "name": "Conference",
      "url": "https://example.com/conference",
      "requiresAuthorisation": true,
      "description": "Example Conference",
      "value": "Welcome",
      "time-frames": [
        {
          "start": "2050-01-01T10:00",
          "end": "2050-01-01T20:00"
        }
      ]
    },
    {
      "id": "on-site-training.example.com",
      "name": "On-site training",
      "url": "https://example.com/events/on-site-training",
      "requiresAuthorisation": true,
      "description": "Example on-site training",
      "minMax": {
        "min": 0,
        "max": 10,
      },
      "time-frames": [
        {
          "start": "10:00",
          "end": "11:00",
          "weekday": "1"
        },
        {
          "start": "11:00",
          "end": "12:00",
          "weekday": "1"
        },
        {
          "start": "10:00",
          "end": "11:00",
          "weekday": "2"
        },
        {
          "start": "11:00",
          "end": "12:00",
          "weekday": "2"
        }
      ]
    }
  ],
  "state": [
    {
      "userId": "john",
      "itemId": "auth-tutorial.example.com",
      "stepsState": [
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
          "value": false
        }
      ]
    },
    {
      "userId": "adam",
      "itemId": "on-site-training.example.com",
      "minMaxState": 8
    },
    {
      "itemId": "conference.example.com",
      "valueState": "Track A, Talk 2/10"
    },
    {
      "userId": "ewa",
      "itemId": "conference.example.com",
      "valueState": "Track A, Talk 2/10"
    },
    {
      "userId": "david",
      "itemId": "conference.example.com",
      "valueState": "VIP Track B, Talk 2/8"
    },
  ]
}
```

## Specification implementation

The specification may be implemented by website by placing file on domain root path, e.g. `example.com/learnme.json`.

The specification is used for planning and tracking education in [learntray.com](https://learntray.com), so by setting

```js
{
  ...
  "agreement": {
    ...
    "individuals": ["learntray.com"]
  }
}
```

the content described by learnme file may be indexed and used by LearnTray website and then users may include that content in their learning plans. To learn more check out [learntray documentation](https://learntray.com/docs).

Due to the security reasons, only HTTPS connection is recommended, not HTTP.

## Specification development

The specification is founded and maintained by [@orzechdev](https://github.com/orzechdev). In case of needed contact please reach founder directly or create an issue in this repository.
