---
location: "/documentation/integration/openfeature.md"
title: "OpenFeature"
---

# {% $markdoc.frontmatter.title %}

The `openfeature` integration provides an opinionated way to interact with feature
flags for helix services. It uses the OpenFeature specification and the GO Feature
Flag provider.

## Event attributes

The `openfeature` integration doesn't create traces/spans. Instead, it creates a
new event on each flag evaluation associated to the active span. It sets the
following event attributes:
- `openfeature.flag`
- `openfeature.default_value`
- `openfeature.target`
- `openfeature.variant`
- `openfeature.value`

Example:
```
name: "openfeature.evaluate"
openfeature.flag: "color-testing"
openfeature.default_value: "red"
openfeature.target: "bf3eab40-29ec-46e1-afa9-7dd8c08dc6a0"
openfeature.variant: "version-blue"
openfeature.value: "blue"
```

## Usage

{% tab name="Go" %}
  {% partial file="helix-go/integration/openfeature.md" /%} 
{% /tab %}
