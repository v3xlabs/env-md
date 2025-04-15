# Prometheus

In order to gather metrics from applications one of the most popular tools is Prometheus.
By default, prometheus uses a pull model for scraping metrics. Meaning that some prometheus daemon (prometheus, alloy, etc) will hit your applications HTTP (or grpc) endpoint every X amount of time to get the metrics.

## Label Naming

When you want to make sure metrics end up looking nice on a grafana dashboard, use the default label name `label_name`.

## OpenTelemetry

Semantic Conventions
https://opentelemetry.io/docs/concepts/semantic-conventions/
