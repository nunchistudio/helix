---
location: "/documentation/integration/clickhouse.md"
title: "ClickHouse"
---

# {% $markdoc.frontmatter.title %}

The `clickhouse` integration provides an opinionated way to interact with
ClickHouse as OLAP database for helix services.

## Trace attributes

The `clickhouse` integration sets the following trace attributes:
- `clickhouse.database`
- `span.kind`

When applicable, these attributes can be set as well:
- `clickhouse.async_insert.wait`
- `clickhouse.query`

Example:
```
clickhouse.database: "my_db"
clickhouse.query: "SELECT id, username FROM users;"
span.kind: "server"
```

## Usage

{% tab name="Go" %}
  {% partial file="helix-go/integration/clickhouse.md" /%} 
{% /tab %}
