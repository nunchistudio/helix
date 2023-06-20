---
location: "/documentation/integration/nats.md"
title: "NATS JetStream"
---

# {% $markdoc.frontmatter.title %}

The `nats` integration provides an opinionated way to interact with NATS for
helix services. It uses JetStream only for distributed key-value store and higher
Quality of Service (QoS).

## Example

{% tab name="Go" %}
  {% partial file="helix-go/integration/nats.md" /%} 
{% /tab %}
