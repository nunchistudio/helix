---
location: "/documentation/integration/postgres.md"
title: "PostgreSQL"
---

# {% $markdoc.frontmatter.title %}

The `postgres` integration provides an opinionated way to interact with PostgreSQL
as OLTP database for helix services.

## Trace attributes

The `postgres` integration sets the following trace attributes:
- `postgres.database`
- `span.kind`

When applicable, these attributes can be set as well:
- `postgres.query`
- `postgres.batch.length`
- `postgres.transaction.query`
- `postgres.transaction.batch.length`

Example:
```
postgres.database: "my_db"
postgres.query: "SELECT id, username FROM users;"
span.kind: "server"
```

## Usage

{% tab name="Go" %}
  {% partial file="helix-go/integration/postgres.md" /%} 
{% /tab %}
