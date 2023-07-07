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

## Health check

The `rest` integration allows to pass a custom HTTP handler function for health
check. It is exposed at `GET /health`.

Example:
```sh
$ curl --request GET \
  --url http://localhost:8080/health
```

By default if no custom function is passed, the `rest` integration retrieves the
health status of each integration attached to the service running the `rest`
integration, and returns the highest HTTP status code returned. This means if all
integrations are healthy (status `200`) but one is temporarily unavailable (status
`503`), the HTTP status code would be `503`, and therefore the response body of
the health check would be:
```json
{
	"status": "Service Unavailable"
}
```

## Usage

{% tab name="Go" %}
  {% partial file="helix-go/integration/rest.md" /%} 
{% /tab %}
