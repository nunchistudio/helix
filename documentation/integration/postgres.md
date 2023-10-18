---
location: "/documentation/integration/postgres.md"
title: "PostgreSQL"
---

# {% $markdoc.frontmatter.title %}

The `postgres` integration provides an opinionated way to interact with PostgreSQL
as OLTP database for helix services.

The integration works with all PostgreSQL-compatible databases, such as:
- [Citus](https://www.citusdata.com/)
- [Timescale](https://www.timescale.com/)
- [CockroachDB](https://www.cockroachlabs.com/)
- [YugabyteDB](https://www.yugabyte.com/)
- [Neon](https://neon.tech/)
- [Google AlloyDB](https://cloud.google.com/alloydb)
- [Amazon Aurora](https://aws.amazon.com/rds/aurora/)

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
