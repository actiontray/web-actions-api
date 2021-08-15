# Domain learnme.json open specification

<b>learnme.json</b> file is the proposal for describing learning possibilities available under each domain address, where it is placed on. Learning possibilities may include guides, courses, trainings or other educational resources available at the given website.

Specification is adopted by [learntray.com](https://learntray.com) to keep track of users learning progress on Internet websites.

## Table of content

- [Specification](#specification)
- [Specification implementation](#specification-implementation)
- [Specification development](#specification-development)

## Specification

Specification of `learnme.json` version `0.1.0`.

### Specific parameters

| Object                    | Description                                                                                                                                                                |
| ------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `learnme`                 | learnme file specification version.                                                                                                                                        |
| `version`                 | File version.                                                                                                                                                              |
| `agreement`               | Agreement for collecting and processing data described in learnme file given for specified individuals.                                                                    |
| `organisations`           | List of available organisations under given domain.                                                                                                                        |
| `authors`                 | List of available authors under given domain.                                                                                                                              |
| `resources`               | List of available learning resources (guides, courses or trainings) under given domain.                                                                                    |
| `resources.type`          | `Guide` or `Course`.                                                                                                                                                       |
| `resources.categories`    | Categories to which resource can be assigned.                                                                                                                              |
| `resources.prerequisites` | Prerequisites which must be learned before learning resource. Must be identifier (url) of resource or resource part described in learnme file of the same or other domain. |
| `resources.owners`        | Authors of the resource. Must be identifier (url) of owner described in learnme file of the same or other domain.                                                          |
| `resources.authors`       | Authors of the resource. Must be identifier (email) of author described in learnme file of the same or other domain.                                                       |
| `resources.parts`         | Parts of which the resource is composed.                                                                                                                                   |

### Common parameters

| Object        | Description                                                                                                                                                                                                                                                                                                        |
| ------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `name`        | Name of the thing.                                                                                                                                                                                                                                                                                                 |
| `description` | Description of the thing.                                                                                                                                                                                                                                                                                          |
| `url`         | Url address under which given thing may be accessed. Is treated as unique course or course resource identifier. Url domain must be the same as the current domain name or may be not included at all (meaning that the path is relative to the current domain) - you can't define anything outside of your domain. |

### Example

```json
{
  "learnme": "0.1.0",
  "version": "1.0.0",
  "agreement": {
    "agreement": "As an owner of the website I agree for agreement individuals to collect and process data described here for period of time described in agreement duration and for the purposes described in agreement scopes.",
    "duration": "6 months from now",
    "individuals": ["example-individual.com"],
    "scopes": ["learning", "teaching"]
  },
  "organisations": [
    {
      "name": "Example",
      "url": "https://example.com",
      "admin": "admin@example.com",
      "description": "Example organisation"
    }
  ],
  "authors": [
    {
      "name": "Adam Nowak",
      "email": "adam@example.com"
    }
  ],
  "resources": [
    {
      "name": "Introduction",
      "type": "Guide",
      "categories": ["Software development"],
      "url": "https://example.com/guides/introduction",
      "description": "Example Introduction Guide",
      "owners": ["https://example.com"],
      "authors": ["adam@example.com"],
      "prerequisites": [
        "https://dependent-example.com/guides/introduction/first-chapter",
        "https://dependent-example.com/guides/introduction/second-chapter"
      ],
      "parts": [
        {
          "name": "Getting started",
          "url": "https://example.com/guides/introduction/getting-started"
        },
        {
          "name": "Setup",
          "url": "https://example.com/guides/introduction/setup"
        }
      ]
    },
    {
      "name": "Architecture Overview",
      "type": "Guide",
      "categories": ["Software development"],
      "url": "https://example.com/guides/architecture-overview",
      "description": "Example Architecture Overview Guide",
      "owners": ["https://example.com"],
      "authors": ["adam@example.com"],
      "prerequisites": ["https://dependent-example.com/guides/introduction"],
      "parts": [
        {
          "name": "Frontend",
          "url": "https://example.com/guides/architecture-overview/frontend"
        },
        {
          "name": "Backend",
          "url": "https://example.com/guides/architecture-overview/backend"
        },
        {
          "name": "Connection",
          "url": "https://example.com/guides/architecture-overview/connection"
        }
      ]
    }
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
