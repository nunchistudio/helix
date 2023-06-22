---
location: "/documentation/integration/rest.md"
title: "REST router"
---

# {% $markdoc.frontmatter.title %}

The `rest` integration provides an opinionated way to build a HTTP REST API with
support for OpenAPI validations.

## Trace attributes

The `rest` integration sets the following trace attributes:
- `http.client_ip`
- `http.flavor`
- `http.method`
- `http.route`
- `http.scheme`
- `http.status_code`
- `http.target`
- `http.user_agent`
- `http.wrote_bytes`
- `net.host.name`
- `net.host.port`
- `net.sock.peer.addr`
- `net.sock.peer.port`
- `span.kind`

Example:
```
http.client_ip: "127.0.0.1"
http.flavor: "1.1"
http.method: "POST"
http.route: "/anything"
http.scheme: "http"
http.status_code: 202
http.target: "/anything"
http.user_agent: "insomnia/2023.2.2"
http.wrote_bytes: 21
net.host.name: "localhost"
net.host.port: 8080
net.sock.peer.addr: "127.0.0.1"
net.sock.peer.port: 50643
span.kind: "server"
```

## Usage

{% tab name="Go" %}
  {% partial file="helix-go/integration/rest.md" /%} 
{% /tab %}
