# Tinybird OpenTelemetry Template

[OpenTelemetry](https://opentelemetry.io/) is an open-source observability framework for collecting, processing, and exporting telemetry data (metrics, traces, and logs) from your applications and infrastructure.

By integrating OpenTelemetry with Tinybird, you can analyze observability data in real time, build dashboards, and enrich it with other data sources.

Some common use cases for sending OpenTelemetry data to Tinybird include:

1. Centralizing metrics, traces, and logs for unified analytics.
2. Building custom dashboards and alerts on top of observability data.
3. Enriching telemetry with business or application data.

Read on to learn how to send data from OpenTelemetry to Tinybird.

## Before you start

Before you connect OpenTelemetry to Tinybird, ensure:

* You have a Tinybird workspace.
* You have a Tinybird Token with **append** permissions to the target Data Sources.
* You are running a release version of the OpenTelemetry Collector higher than release v0.131.0.

- [GitHub Releases](https://github.com/open-telemetry/opentelemetry-collector-contrib/releases)
- [Docker Hub](https://hub.docker.com/r/otel/opentelemetry-collector-contrib)

> [!TIP]
> The Tinybird OpenTelemetry Exporter is officially available in the [OpenTelemetry Collector Contrib repository](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/exporter/tinybirdexporter) as of release v0.131.0. You can use the official OpenTelemetry Collector distributions that include the Tinybird exporter out of the box.

## Use the Tinybird OpenTelemetry project template

To get started quickly, you can use the [Tinybird OpenTelemetry project template](https://github.com/tinybirdco/tinybird-otel-template). This template provides ready-to-use Data Sources and Pipes for storing and analyzing your telemetry data in Tinybird.

```bash
# select or create a new workspace
tb login

# deploy the template
tb --cloud deploy --template https://github.com/tinybirdco/tinybird-otel-template/tree/main/
```

## Example OpenTelemetry Collector configuration

Below is an example configuration for the Tinybird OpenTelemetry Collector to export metrics, traces, and logs to Tinybird:

```yaml
receivers:
  otlp:
    protocols:
      grpc:
      http:

processors:
  batch:
    timeout: 10s
    send_batch_size: 8192

exporters:
  tinybird:
    endpoint: ${OTEL_TINYBIRD_API_HOST}
    token: ${OTEL_TINYBIRD_TOKEN}
    metrics:
      sum:
        datasource: otel_metrics_sum
      histogram:
        datasource: otel_metrics_histogram
      exponential_histogram:
        datasource: otel_metrics_exponential_histogram
      gauge:
        datasource: otel_metrics_gauge
    traces:
      datasource: otel_traces
    logs:
      datasource: otel_logs

service:
  pipelines:
    metrics:
      receivers: [otlp]
      processors: [batch]
      exporters: [tinybird]

    traces:
      receivers: [otlp]
      processors: [batch]
      exporters: [tinybird]

    logs:
      receivers: [otlp]
      processors: [batch]
      exporters: [tinybird]
```

### Environment variables

* `OTEL_TINYBIRD_API_HOST`: The API host for your Tinybird workspace (e.g., `https://api.tinybird.co`).
* `OTEL_TINYBIRD_TOKEN`: A Tinybird token with **append** permissions to the target Data Sources (`otel_metrics`, `otel_traces`, `otel_logs`).

## Next steps

* Explore and customize the [Tinybird OpenTelemetry project template](https://github.com/tinybirdco/tinybird-otel-template) to fit your needs.
* Use the ingested data to build real-time analytics, dashboards, and alerts in Tinybird.

For more details on the available configuration options, see the [Tinybird OpenTelemetry Collector documentation](https://github.com/tinybirdco/opentelemetry-collector-contrib).
