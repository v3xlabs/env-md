# Prometheus

In order to gather metrics from applications one of the most popular tools is Prometheus.
By default, prometheus uses a pull model for scraping metrics. Meaning that some prometheus daemon (prometheus, alloy, etc) will hit your applications HTTP (or grpc) endpoint every X amount of time to get the metrics.

The ideal metrics for this endpoint are things like:

- HTTP request count
- Cache hits and misses
- Database query count and duration
- Average request duration
- Error count
- Rate limits

## Metrics Endpoint

Generally speaking you should use a library to expose your metrics. But to get a general idea, the endpoint responds to a plain GET request and looks like this:

```prometheus
# TYPE http_requests_total counter
http_requests_total{method="GET", path="/"} 1234
```

## Label Naming

When you want to make sure metrics end up looking nice on a grafana dashboard, use the default label name `label_name`.

## OpenTelemetry

Semantic Conventions
https://opentelemetry.io/docs/concepts/semantic-conventions/
