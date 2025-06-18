# Tinybird OTEL Template

This repository is a template for building Tinybird projects that leverage [OpenTelemetry](https://opentelemetry.io/) (OTEL) for observability and analytics. Use this project as a starting point to instrument your applications with OpenTelemetry, collect metrics, traces, and logs, and analyze or expose them efficiently via Tinybird.

## Overview
- **Project type:** Tinybird SRC repository
- **Purpose:** Enable rapid ingestion and analysis of OpenTelemetry data using Tinybird
- **Contents:**
   - `.datasource` files defining Tinybird tables for OTEL metrics, logs, and traces
   - Folders for connections, pipes, endpoints, and materializations

## How to Use
1. **Fork/clone this repository as your Tinybird project root.**
2. **Instrument your services using OpenTelemetry SDKs.**
3. **Forward OTEL metrics, logs, and traces to this project's datasources (using the OpenTelemetry Collector, for example).**
4. **Create and publish Tinybird pipes and endpoints as needed for analytics and querying.**

## Typical Structure
- `datasources/`: Definitions for raw OTEL data ingestion
- `pipes/`: Transform/query data
- `endpoints/`: Expose analytics via API
- Other folders: `connections/`, `copies/`, `fixtures/`, `materializations/`, `tests/`

## Getting Started
- See Tinybird documentation: [docs.tinybird.co](https://docs.tinybird.co/)
- See OpenTelemetry docs: [opentelemetry.io/docs/](https://opentelemetry.io/docs/)

## Who is this for?
Developers and SREs who want plug-and-play observability using OTEL and analytical capabilities via Tinybird.

---
Want to contribute or extend? Open issues or submit PRs!
