---
title: "Observability"
type: "docs"
weight: 8
---

The Nauthera platform provides comprehensive observability features to monitor the health and performance of its components.

## Metrics and Traces

The Nauthera platform components expose metrics in the Prometheus format, allowing for easy integration with modern monitoring solutions. Additionally, the platform supports OpenTelemetry (OTEL) for distributed tracing, enabling detailed insights into request flows and performance bottlenecks. The OTEL endpoint can be configured to send traces to a variety of backends like Jaeger or Grafana Tempo.

```mermaid
graph TD
    subgraph "Nauthera Platform"
        AuthServer[Authentication Server]
        Operator[Nauthera Operator]
    end

    subgraph "Monitoring Stack"
        Prometheus[Prometheus]
        OTELCollector[OpenTelemetry Collector]
        Jaeger[Jaeger / Tempo]
    end

    AuthServer -- "Exposes /metrics" --> Prometheus
    Operator -- "Exposes /metrics" --> Prometheus
    AuthServer -- "Sends traces" --> OTELCollector
    Operator -- "Sends traces" --> OTELCollector
    OTELCollector -- "Forwards traces" --> Jaeger
