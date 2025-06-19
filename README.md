# Ingest from OpenTelemetry

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
* You are running the Tinybird distribution of the OpenTelemetry Collector.

You can find the latest release of the Tinybird OpenTelemetry Collector here:

- [GitHub Releases](https://github.com/tinybirdco/opentelemetry-collector-contrib/releases)
- [Docker Hub](https://hub.docker.com/r/tinybirdco/opentelemetry-collector-contrib/tags)

## Use the Tinybird OpenTelemetry project template

To get started quickly, you can use the [Tinybird OpenTelemetry project template](https://github.com/tinybirdco/tinybird-otel-template). This template provides ready-to-use Data Sources and Pipes for storing and analyzing your telemetry data in Tinybird.

## Minimal OpenTelemetry Collector configuration

Below is a minimal example configuration for the Tinybird OpenTelemetry Collector to export metrics, traces, and logs to Tinybird:

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
    metrics_sum:
      datasource: opentelemetry_metrics_sum
    metrics_histogram:
      datasource: opentelemetry_metrics_histogram
    metrics_exponential_histogram:
      datasource: opentelemetry_metrics_exponential_histogram
    metrics_gauge:
      datasource: opentelemetry_metrics_gauge
    traces_datasource: otel_traces
    logs_datasource: otel_logs

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

- `OTEL_TINYBIRD_API_HOST`: The API host for your Tinybird workspace (e.g., `https://api.tinybird.co`).
- `OTEL_TINYBIRD_TOKEN`: A Tinybird token with **append** permissions to the target Data Sources (`otel_metrics`, `otel_traces`, `otel_logs`).

You can create a token in the Tinybird UI under **Tokens**. Make sure it has the required append permissions for the Data Sources you want to ingest into.

## Next steps

- Explore and customize the [Tinybird OpenTelemetry project template](https://github.com/tinybirdco/tinybird-otel-template) to fit your needs.
- Use the ingested data to build real-time analytics, dashboards, and alerts in Tinybird.

For more details on the available configuration options, see the [Tinybird OpenTelemetry Collector documentation](https://github.com/tinybirdco/opentelemetry-collector-contrib).
