---
location: "/documentation/integration/temporal.md"
title: "Temporal"
---

# {% $markdoc.frontmatter.title %}

The `temporal` integration provides an opinionated way to interact with Temporal
for workflow orchestration for helix services.

## Trace attributes

The `temporal` integration sets the following trace attributes:
- `temporal.server.address`
- `temporal.namespace`
- `span.kind`

When applicable, these attributes can be set as well:
- `temporal.worker.taskqueue`
- `temporal.workflow.id`
- `temporal.workflow.run_id`
- `temporal.workflow.namespace`
- `temporal.workflow.type`
- `temporal.workflow.attempt`
- `temporal.activity.id`
- `temporal.activity.type`
- `temporal.activity.attempt`
- `temporal.schedule.id`
- `temporal.search.query`
- `temporal.signal.name`

Example:
```
temporal.server.address: "temporal.mydomain.tld"
temporal.namespace: "default"
temporal.worker.taskqueue: "demo"
temporal.workflow.namespace: "default"
temporal.workflow.type: "hello_world"
temporal.workflow.attempt: 2
span.kind: "internal"
```

## Usage

{% tab name="Go" %}
  {% partial file="helix-go/integration/temporal.md" /%} 
{% /tab %}
