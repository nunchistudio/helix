---
location: "/documentation/opentelemetry.md"
title: "OpenTelemetry"
---

# {% $markdoc.frontmatter.title %}

helix leverages [OpenTelemetry](https://opentelemetry.io/) to instrument, generate,
collect, and export telemetry data (metrics, logs, and traces) to help you analyze
your software's performance and behavior.

By being configured at its core, OpenTelemetry on helix services brings strong
observability consistency across integrations and services.

## Environment variables
 
helix relies on the following required environment variables:

- `ENVIRONMENT` represents the environment the service is currently running in.
  When value is one of `local`, `localhost`, `dev`, `development`, the logger
  handles logs at `debug` level and higher. Otherwise, the logger handles logs at
  `info` level and higher.
- `OTEL_EXPORTER_OTLP_TRACES_ENDPOINT` sets the target endpoint the trace exporter
  will connect to. See [example using Grafana Agent](#grafana-agent) below for
  more details.

## Telemetry packages

### Traces

When possible, it's strongly advised to leverage [Event propagation](/helix/event-propagation)
within distributed tracing.

{% tabs %}
  {% tab name="Go" %}
    {% partial file="helix-go/opentelemetry-traces.md" /%} 
  {% /tab %}
{% /tabs %}

{% accordion title="Running on Kubernetes" %}
  When running a service in Kubernetes, traces are automatically populated with
  these additional attributes:

  - `kubernetes.namespace`
  - `kubernetes.pod`
{% /accordion %}

{% accordion title="Running on Nomad" %}
  When running a service in Nomad, traces are automatically populated with these
  additional attributes:

  - `nomad.datacenter`
  - `nomad.job`
  - `nomad.namespace`
  - `nomad.region`
  - `nomad.task`
{% /accordion %}

### Logs

Logs are JSON formatted and have by default the following keys:

- `message`: The log's message.
- `timestamp`: Timetamp of the log, formatted for RFC-3339 with nano precision.
- `trace_id` and `span_id`: Trace details, if log is part of a trace.

Logs levels are `debug`, `info`, `warn`, `error`, `fatal`.

If environment variable `ENVIRONMENT` is one of `local`, `localhost`, `dev`,
`development`, the logger handles logs at `debug` level and higher. Otherwise,
the logger handles logs at `info` level and higher.

{% tabs %}
  {% tab name="Go" %}
    {% partial file="helix-go/opentelemetry-logs.md" /%} 
  {% /tab %}
{% /tabs %}

{% accordion title="Running on Kubernetes" %}
  When running a service in Kubernetes, logs are automatically populated with these
  additional fields:

  - `kubernetes_namespace`
  - `kubernetes_pod`
{% /accordion %}

{% accordion title="Running on Nomad" %}
  When running a service in Nomad, logs are automatically populated with these
  additional fields:

  - `nomad_datacenter`
  - `nomad_job`
  - `nomad_namespace`
  - `nomad_region`
  - `nomad_task`
{% /accordion %}

## OpenTelemetry collectors

### Grafana Agent

You will find below *examples* for configuring Grafana Agent with appropriate
log labels (using Loki) and trace attributes (using Tempo) when running helix
services.

{% accordion title="Running on Kubernetes" %}
  ```yaml
  server:
    log_level: "warn"
    log_format: "json"

  logs:
    configs:
      - name: "loki"
        clients:
          - url: "https://loki.endpoint.tld/api/v1/push"
            basic_auth:
              username: "username"
              password: "password"
        positions:
          filename: "/tmp/positions.yaml"
        target_config:
          sync_period: "10s"
        scrape_configs:
          - job_name: "pod-logs"
            kubernetes_sd_configs:
              - role: "pod"
            pipeline_stages:
              - docker: {}
            relabel_configs:
              - source_labels:
                  - "__meta_kubernetes_pod_node_name"
                target_label: "__host__"
              - action: "labelmap"
                regex: "__meta_kubernetes_pod_label_(.+)"
              - action: "replace"
                replacement: "$1"
                separator: /
                source_labels:
                  - "__meta_kubernetes_namespace"
                  - "__meta_kubernetes_pod_name"
                target_label: "job"
              - action: "replace"
                source_labels:
                  - "__meta_kubernetes_namespace"
                target_label: "namespace"
              - action: "replace"
                source_labels:
                  - "__meta_kubernetes_pod_name"
                target_label: "pod"
              - action: "replace"
                source_labels:
                  - "__meta_kubernetes_pod_container_name"
                target_label: "container"
              - replacement: "/var/log/pods/*$1/*.log"
                separator: /
                source_labels:
                  - "__meta_kubernetes_pod_uid"
                  - "__meta_kubernetes_pod_container_name"
                target_label: "__path__"
            pipeline_stages:
              - docker:
              - json:
                  expressions:
                    level: "level"
                    kubernetes_namespace: "kubernetes_namespace"
                    kubernetes_pod: "kubernetes_pod"
                    span: "span"
                    timestamp: "timestamp"
                  timestamp:
                    source: "timestamp"
                    format: "RFC3339Nano"
              - labels:
                  level:
                  kubernetes_namespace:
                  kubernetes_pod:
                  span:

  traces:
    configs:
      - name: "tempo"
        automatic_logging:
          backend: "logs_instance"
          logs_instance_name: "loki"
          spans: true
          roots: true
          processes: true
          process_attributes:
            - "kubernetes.namespace"
            - "kubernetes.pod"
            - "span"
          span_attributes:
            - "kubernetes.namespace"
            - "kubernetes.pod"
            - "span"
          labels:
            - "kubernetes.namespace"
            - "kubernetes.pod"
            - "span"
          overrides:
            duration_key: "duration"
            service_key: "kubernetes_pod"
            trace_id_key: "trace_id"
        receivers:
          otlp:
            protocols:
              grpc:
                endpoint: "0.0.0.0:7021"
        remote_write:
          - endpoint: "tempo.endpoint.tld"
            insecure: true
            basic_auth:
              username: "username"
              password: "password"
  ```
{% /accordion %}

{% accordion title="Running on Nomad" %}
  ```yaml
  server:
    log_level: "warn"
    log_format: "json"

  logs:
    configs:
      - name: "loki"
        clients:
          - url: "https://loki.endpoint.tld/api/v1/push"
            basic_auth:
              username: "username"
              password: "password"
        positions:
          filename: "/tmp/positions.yaml"
        target_config:
          sync_period: "10s"
        scrape_configs:
          - job_name: "docker/system"
            docker_sd_configs:
              - host: "unix:///var/run/docker.sock"
                refresh_interval: "5s"
            relabel_configs:
              - action: "replace"
                replacement: "docker/system"
                source_labels:
                  - "__meta_docker_container_id"
                target_label: "job"
              - source_labels:
                  - "__meta_docker_container_name"
                regex: "/(.*)"
                target_label: "container"
              - source_labels:
                  - "__meta_docker_container_log_stream"
                target_label: "stream"
          - job_name: "docker/services"
            static_configs:
              - targets:
                  - "localhost"
                labels:
                  job: "docker/services"
                  __path__: "/var/lib/docker/containers/*/*log"
            pipeline_stages:
              - docker:
              - json:
                  expressions:
                    level: "level"
                    nomad_datacenter: "nomad_datacenter"
                    nomad_job: "nomad_job"
                    nomad_namespace: "nomad_namespace"
                    nomad_region: "nomad_region"
                    nomad_task: "nomad_task"
                    span: "span"
                    timestamp: "timestamp"
                  timestamp:
                    source: "timestamp"
                    format: "RFC3339Nano"
              - labels:
                  level:
                  nomad_datacenter:
                  nomad_job:
                  nomad_namespace:
                  nomad_region:
                  nomad_task:
                  span:

  traces:
    configs:
      - name: "tempo"
        automatic_logging:
          backend: "logs_instance"
          logs_instance_name: "loki"
          spans: true
          roots: true
          processes: true
          process_attributes:
            - "nomad.namespace"
            - "nomad.datacenter"
            - "nomad.region"
            - "nomad.job"
            - "nomad.task"
            - "span"
          span_attributes:
            - "nomad.namespace"
            - "nomad.datacenter"
            - "nomad.region"
            - "nomad.job"
            - "nomad.task"
            - "span"
          labels:
            - "nomad.datacenter"
            - "nomad.job"
            - "nomad.namespace"
            - "nomad.region"
            - "nomad.task"
            - "span"
          overrides:
            duration_key: "duration"
            service_key: "nomad_task"
            trace_id_key: "trace_id"
        receivers:
          otlp:
            protocols:
              grpc:
                endpoint: "0.0.0.0:7021"
        remote_write:
          - endpoint: "tempo.endpoint.tld"
            insecure: true
            basic_auth:
              username: "username"
              password: "password"
  ```
{% /accordion %}
