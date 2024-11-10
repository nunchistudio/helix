---
location: "/documentation/openapi-asyncapi.md"
title: "OpenAPI & AsyncAPI"
---

# {% $markdoc.frontmatter.title %}

When it makes sense, OpenAPI and AsyncAPI specifications can optionally be leveraged
for data validation. This can happen in the REST router for request/response
validation against an OpenAPI description, or in other integrations for message
validation against an AsyncAPI description.

You will find below OpenAPI and AsyncAPI definitions of helix objects, shared
across languages and integrations.

Each object can be used as a `$ref` in your OpenAPI and AsyncAPI descriptions,
either as a "schema" object or as a whole response. Example:
```yaml
post:
  operationId: custom
  tags:
    - System
  summary: Custom operation
  description: |
    This is a custom operation.
  responses:
    '202':
      description: Accepted
      content:
        application/json:
          schema:
            allOf:
              - $ref: https://raw.githubusercontent.com/nunchistudio/helix/refs/heads/main/descriptions/openapi/components/schemas/rest/200.yaml
              - type: object
                properties:
                  metadata:
                    type: object
                    properties:
                      temporal:
                        $ref: https://raw.githubusercontent.com/nunchistudio/helix/refs/heads/main/descriptions/openapi/components/schemas/temporal/Metadata.yaml
                    required:
                      - temporal
                required:
                  - metadata
    '400':
      $ref: https://raw.githubusercontent.com/nunchistudio/helix/refs/heads/main/descriptions/openapi/components/responses/400.yaml
    '401':
      $ref: https://raw.githubusercontent.com/nunchistudio/helix/refs/heads/main/descriptions/openapi/components/responses/401.yaml
    '429':
      $ref: https://raw.githubusercontent.com/nunchistudio/helix/refs/heads/main/descriptions/openapi/components/responses/429.yaml
    '500':
      $ref: https://raw.githubusercontent.com/nunchistudio/helix/refs/heads/main/descriptions/openapi/components/responses/500.yaml
    '503':
      $ref: https://raw.githubusercontent.com/nunchistudio/helix/refs/heads/main/descriptions/openapi/components/responses/503.yaml
```

## Shared objects

Below are objects compatible with both OpenAPI and AsyncAPI specifications, used
by helix core and all integrations.

{% schema specification="OpenAPI & AsyncAPI" name="Event" color="primary"
  file="/helix/descriptions/shared/components/schemas/Event.yaml"
/%}

{% schema specification="OpenAPI & AsyncAPI" name="Error" color="primary"
  file="/helix/descriptions/shared/components/schemas/Error.yaml"
/%}

## OpenAPI

### REST integration

Below are objects compatible with OpenAPI designed for the the REST router
integration.

#### 2xx responses

{% schema specification="OpenAPI" type="HTTP response" name="200" color="success"
  file="/helix/descriptions/openapi/components/responses/200.yaml"
/%}

{% schema specification="OpenAPI" type="HTTP response" name="201" color="success"
  file="/helix/descriptions/openapi/components/responses/201.yaml"
/%}

{% schema specification="OpenAPI" type="HTTP response" name="202" color="success"
  file="/helix/descriptions/openapi/components/responses/202.yaml"
/%}

#### 4xx responses

{% schema specification="OpenAPI" type="HTTP response" name="400" color="warning"
  file="/helix/descriptions/openapi/components/responses/400.yaml"
/%}

{% schema specification="OpenAPI" type="HTTP response" name="401" color="warning"
  file="/helix/descriptions/openapi/components/responses/401.yaml"
/%}

{% schema specification="OpenAPI" type="HTTP response" name="402" color="warning"
  file="/helix/descriptions/openapi/components/responses/402.yaml"
/%}

{% schema specification="OpenAPI" type="HTTP response" name="403" color="warning"
  file="/helix/descriptions/openapi/components/responses/403.yaml"
/%}

{% schema specification="OpenAPI" type="HTTP response" name="404" color="warning"
  file="/helix/descriptions/openapi/components/responses/404.yaml"
/%}

{% schema specification="OpenAPI" type="HTTP response" name="405" color="warning"
  file="/helix/descriptions/openapi/components/responses/405.yaml"
/%}

{% schema specification="OpenAPI" type="HTTP response" name="409" color="warning"
  file="/helix/descriptions/openapi/components/responses/409.yaml"
/%}

{% schema specification="OpenAPI" type="HTTP response" name="413" color="warning"
  file="/helix/descriptions/openapi/components/responses/413.yaml"
/%}

{% schema specification="OpenAPI" type="HTTP response" name="429" color="warning"
  file="/helix/descriptions/openapi/components/responses/429.yaml"
/%}

#### 5xx responses

{% schema specification="OpenAPI" type="HTTP response" name="500" color="danger"
  file="/helix/descriptions/openapi/components/responses/500.yaml"
/%}

{% schema specification="OpenAPI" type="HTTP response" name="503" color="danger"
  file="/helix/descriptions/openapi/components/responses/503.yaml"
/%}

### Temporal integration

Below are objects compatible with OpenAPI designed and scoped in the Temporal
integration, and that can be returned in a HTTP response of a REST API.

{% schema specification="OpenAPI" type="Temporal" name="Metadata" color="primary"
  file="/helix/descriptions/openapi/components/schemas/temporal/Metadata.yaml"
/%}
