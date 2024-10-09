---
location: "/documentation/from-zero-to-hero.md"
title: "From zero to hero in 20′"
---

# {% $markdoc.frontmatter.title %}

In this tutorial we will setup a local OpenTelemetry stack for helix services.
Then, we will create our first helix service in Go: a HTTP API exposing a single
endpoint. Finally, we will create a second helix service, with a publish/subscribe
mechanism between the two services.

At the end, you will be able to see how easy it is to use helix and how helix can
automate most of your counterproductive tasks when working with multiple services.

{% callout level="success" iconType="bullseye" title="TLDR; Source code is on GitHub" %}
  Source code of this guide is available in the [`platform-starter` repository
  on GitHub](https://github.com/nunchistudio/platform-starter).
{% /callout %}

## Local OpenTelemetry

To fully appreciate the power of helix, you should have an OpenTelemetry stack
running. If you don't, the best way to do so is to use the Docker environment made
for this guide: [`docker-opentelemetry`](https://github.com/nunchistudio/docker-opentelemetry).
If you already have one, you can skip this section and move on to the next.

{% callout level="primary" iconType="glasses" title="Before going further" %}
  If you are not familiar with OpenTelemetry, I first suggest to read a bit about
  it on the [OpenTelemetry website](https://opentelemetry.io/).
{% /callout %}

This repository is a Docker environment for running an OpenTelemetry stack locally
using:
- [**Prometheus**](https://prometheus.io/) for metrics;
- [**Mimir**](https://grafana.com/oss/mimir/) as Prometheus long-term storage;
- [**Loki**](https://grafana.com/oss/loki/) for logs;
- [**Tempo**](https://grafana.com/oss/tempo/) for traces;
- [**Grafana Agent**](https://grafana.com/oss/agent/) as telemetry collector;
- [**Grafana**](https://grafana.com/oss/grafana/) for visualization;
- [**MinIO**](https://min.io/) as storage for Mimir, Loki, Tempo.

The goal is to provide the "*simplest*" and "*lightest*" setup possible for running
these components together. This means no authentication, no HA configuration, no
load-balancer, no TLS.

Clone the repository with:
```sh
$ git clone git@github.com:nunchistudio/docker-opentelemetry.git
```

Then, start the Docker environment:
```sh
$ cd ./docker-opentelemetry
$ docker compose up -d
```

At this point you should have all containers running (truncated for better
visibility):
```sh
$ docker ps

IMAGE                    PORTS
grafana/agent:latest     0.0.0.0:7020-7021->7020-7021/tcp
grafana/grafana:latest   0.0.0.0:3000->3000/tcp
prom/prometheus:latest   0.0.0.0:9090->9090/tcp
grafana/mimir:latest     0.0.0.0:3300->3300/tcp, 8080/tcp
grafana/tempo:latest     0.0.0.0:3200-3201->3200-3201/tcp, 0.0.0.0:50397->7946/tcp
grafana/loki:latest      0.0.0.0:3100->3100/tcp, 0.0.0.0:50398->7946/tcp
minio/minio:latest       0.0.0.0:9000-9001->9000-9001/tcp
```

You can access the Grafana dashboard at <http://localhost:3000>. Datasources are
already configured to work with Mimir, Loki, and Tempo.

OpenTelemetry collector endpoint is exposed at `localhost:7021`.

## Your first service

helix requires the environment variable `OTEL_EXPORTER_OTLP_TRACES_ENDPOINT` to
properly run. This represents the endpoint for the OpenTelemetry trace collector.
If you followed the local setup above, you should run:
```sh
$ export OTEL_EXPORTER_OTLP_TRACES_ENDPOINT=localhost:7021
```

{% callout level="warning" iconType="alert" title="Warning" %}
  If the environment variable is not available from a service, traces will not be
  exported/collected. If this is the case you should see an error like this one
  (truncated for better visibility):
  ```sh
  traces export: connection error: desc = "transport: Error while dialing: dial tcp [::1]:7021"
  ```
{% /callout %}

Now that you're all set with an OpenTelemetry stack, you can write your first
helix service. This service will expose a HTTP endpoint on `POST /anything`,
using the [REST router integration](/helix/integration/rest).

{% tabs %}
  {% tab name="Go" %}
    {% partial file="helix-go/from-zero-to-hero-first.md" /%} 
  {% /tab %}
{% /tabs %}

You now have a helix service up and running, exposing a HTTP API on
`http://localhost:8080`. Let's try to request the `POST /anything` endpoint:
```sh
$ curl --request POST \
  --url http://localhost:8080/anything \
  --header 'Content-Type: application/json'
```

The JSON response should be:
```json
{
  "status": "Accepted"
}
```

In Grafana, go to the "*Explore*" section at <http://localhost:3000/explore>.
Select "*Tempo*" as datasource, and search for traces in `httpapi` service. You
should see your first trace:

![Your first trace with helix](/helix/screenshots/trace-simple.png)

It's important to notice here that the only observability step we did on the
application side is to set the environment variable `OTEL_EXPORTER_OTLP_TRACES_ENDPOINT`.
Other than that, tracing was fully automated!

This is only the beginning. We'll now discover automatic distributed tracing and
error recording with event propagation across services.

## Example with NATS JetStream

### Running NATS locally

To leverage the [NATS JetStream integration](/helix/integration/nats), we must
have a NATS server up and running with JetStream enabled. Let's start one with
Docker:
```sh
$ docker run -d -p 4222:4222 nats -js
```

### Publish from the HTTP API

The `httpapi` service will publish a message via NATS JetStream on each and every
HTTP requests made against the endpoint created earlier.

{% tabs %}
  {% tab name="Go" %}
    {% partial file="helix-go/from-zero-to-hero-publish.md" /%} 
  {% /tab %}
{% /tabs %}

### Your second service

The second service, which will be called `subscriber`, will subscribe to the
NATS JetStream subject and receive all messages of the said subject. In this
demo, all messages received will come from the HTTP endpoint of the `httpapi`
service.

{% tabs %}
  {% tab name="Go" %}
    {% partial file="helix-go/from-zero-to-hero-second.md" /%} 
  {% /tab %}
{% /tabs %}

### Event propagation in action

Now, in two different terminals, let's start our two services:
```sh
$ go run ./services/httpapi
```

And:
```sh
$ go run ./services/subscriber
```

Make a new request against the HTTP endpoint created earlier, now publishing a
message:
```sh
$ curl --request POST \
  --url http://localhost:8080/anything \
  --header 'Content-Type: application/json'
```

Let's see how it looks in Grafana "*Explore*" section:

![Your second trace with helix](/helix/screenshots/trace-distributed.png)

As you can notice, the span `Custom Span` created in the NATS subscription has
been added as a child span of the subscription, which itself is a child of the
one publishing the message. We can see the `Event` object has been propagated as
well across these two services.

Also, the NATS JetStream integration has automatically added trace attributes,
such as the queue and subject.

{% callout level="success" iconType="cheer" title="Congratulations!" %}
  You just leveraged helix power to embrace automatic observability across
  services within your stack!
{% /callout %}

## Next steps

To go a bit further with this example and discover helix step-by-step, you could:
- Enable and configure OpenAPI validation for the REST router in the `httpapi`
  service;
- Add another integration in the `subscriber` service (such as PostgreSQL);
- Run both services in Docker containers to explore logs using the Loki datasource
  in Grafana.
