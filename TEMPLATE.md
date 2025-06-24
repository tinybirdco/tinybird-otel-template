The Tinybird OpenTelemetry Template lets you instrument your apps using your pre-existing OpenTelemetry data while leveraging Tinybird's real-time features.

By integrating OpenTelemetry with Tinybird, you can analyze observability data in real time, build dashboards, and enrich it with other data sources.

Some common use cases for sending OpenTelemetry data to Tinybird include:

1. Centralizing metrics, traces, and logs for unified analytics.
2. Building custom dashboards and alerts on top of observability data.
3. Enriching telemetry with business or application data.

Read on to learn how to send data from OpenTelemetry to Tinybird.

## Before you start

Before you connect OpenTelemetry to Tinybird, ensure you are running the Tinybird distribution of the OpenTelemetry Collector.

You can find the latest release of the Tinybird OpenTelemetry Collector here:

* [GitHub Releases](https://github.com/tinybirdco/opentelemetry-collector-contrib/releases)
* [Docker Hub](https://hub.docker.com/r/tinybirdco/opentelemetry-collector-contrib/tags)

## Setup the template

Fork the GitHub repository and deploy the data project to Tinybird.

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
    metrics_sum:
      datasource: otel_metrics_sum
    metrics_histogram:
      datasource: otel_metrics_histogram
    metrics_exponential_histogram:
      datasource: otel_metrics_exponential_histogram
    metrics_gauge:
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
