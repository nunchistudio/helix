---
location: "/documentation/integration/vault.md"
title: "Vault"
---

# {% $markdoc.frontmatter.title %}

The `vault` integration provides an opinionated way to interact with Vault as
secret manager for helix services.

## Trace attributes

The `vault` integration sets the following trace attributes:
- `vault.server.address`
- `vault.agent.address`
- `vault.namespace`
- `span.kind`

When applicable, these attributes can be set as well:
- `vault.kv.mountpath`
- `vault.kv.secretpath`

Example:
```
vault.server.address: "http://localhost:8200"
vault.agent.address: ""
vault.namespace: "custom"
vault.kv.mountpath: "/secrets"
vault.kv.secretpath: "my_secret"
span.kind: "internal"
```

## Usage

{% tabs %}
  {% tab name="Go" %}
    {% partial file="helix-go/integration/vault.md" /%} 
  {% /tab %}
{% /tabs %}
