---
location: "/documentation/integration/bucket.md"
title: "Bucket"
---

# {% $markdoc.frontmatter.title %}

The `bucket` integration provides an opinionated and standardized way to interact
with bucket providers through drivers.

The integration supports the following drivers:
- AWS S3
- Azure Blob Storage
- Google Cloud Storage
- Local files

## Trace attributes

The `bucket` integration sets the following trace attributes:
- `bucket.driver`
- `bucket.bucket`
- `span.kind`

When applicable, these attributes can be set as well:
- `bucket.key`
- `bucket.key_source`
- `bucket.key_destination`
- `bucket.subfolder`

Example:
```
bucket.driver: "aws"
bucket.bucket: "my-bucket"
bucket.key: "blob.json"
bucket.subfolder: "path/to/subfolder/"
span.kind: "client"
```

## Usage

{% tab name="Go" %}
  {% partial file="helix-go/integration/bucket.md" /%} 
{% /tab %}
