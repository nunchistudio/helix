---
location: "/documentation/integration/nats.md"
title: "NATS JetStream"
---

# {% $markdoc.frontmatter.title %}

The `nats` integration provides an opinionated way to interact with NATS for
helix services. It uses JetStream only for distributed key-value store and higher
Quality of Service (QoS).

## Trace attributes

The `nats` integration sets the following trace attributes:
- `span.kind`

When applicable, these attributes can be set as well:
- `nats.message.subject`
- `nats.subscription.subject`
- `nats.subscription.queue`
- `nats.consumer.name`
- `nats.consumer.group`
- `nats.consumer.subject`
- `nats.jetstream.stream.name`
- `nats.jetstream.stream.subjects`
- `nats.jetstream.kv.key`
- `nats.jetstream.kv.bucket.name`

Example:
```
nats.message.subject: "demo"
nats.subscription.queue: "demo-queue"
nats.subscription.subject: "_INBOX.92V248IJYsOunA5qe5I22c"
span.kind: "consumer"
```

## Usage

{% tab name="Go" %}
  {% partial file="helix-go/integration/nats.md" /%} 
{% /tab %}
