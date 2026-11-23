# Observability: How Linux Servers Expose Metrics, Logs & Traces

## 1. Basic Concept

An AWS EC2 Linux server is isolated from the outside world by default, but it can still communicate with other systems through private networking or outbound connections.
Q:"If the EC2 instance is isolated, how can external tools like Prometheus, Grafana, Loki, or Jaeger collect its metrics, logs, and traces?"

To collect observability data, we use **agents, exporters, and collectors**.

There are two main communication models:

- **Pull Model:** Monitoring tools connect to the server and collect data.
- **Push Model:** Agents running on the server send data to external monitoring tools.

## 2. Metrics Collection (Prometheus)

**Node Exporter** is installed on the Linux EC2 instance to collect system metrics.

It collects:
- CPU utilization
- Memory usage
- Disk usage
- Disk I/O
- Network traffic
- System load

Node Exporter exposes metrics through an HTTP endpoint:

```text
http://EC2_PRIVATE_IP:9100/metrics
```

Prometheus periodically scrapes this endpoint and stores the metrics.

**Flow:**

```text
Linux EC2 → Node Exporter → Prometheus → Grafana
```

**Important:** Node Exporter exposes metrics. Prometheus pulls them.

## 3. Logs Collection (Loki)

Linux applications and services generate logs in files such as:

```text
/var/log/syslog
/var/log/auth.log
/var/log/nginx/access.log
/var/log/nginx/error.log
```

We install a log collection agent such as:

- Grafana Alloy
- Fluent Bit
- OpenTelemetry Collector

The agent reads log files and forwards them to Loki.

**Flow:**

```text
Log Files → Grafana Alloy → Loki → Grafana
```

**Important:** The agent usually pushes logs to Loki.

## 4. Traces Collection (OpenTelemetry)

Traces help track requests across applications and services.

For example, a Python FastAPI application can use the OpenTelemetry SDK to capture:

- HTTP requests
- Request duration
- Database calls
- External API calls
- Errors
- Trace IDs and Span IDs

An OpenTelemetry Collector receives and forwards traces to a backend such as Jaeger or Grafana Tempo.

**Flow:**

```text
Application → OpenTelemetry SDK → OTel Collector → Jaeger / Tempo
```

Common OTLP ports:

| Protocol | Port |
|---|---:|
| OTLP gRPC | 4317 |
| OTLP HTTP | 4318 |

## 5. Network Communication

Observability tools communicate with EC2 through network connections.

Example:

| Component | Communication | Port |
|---|---|---:|
| Prometheus → Node Exporter | Inbound to EC2 | 9100 |
| Alloy → Loki | Outbound from EC2 | 3100 |
| Application → OTel Collector | Local/private | 4317 / 4318 |
| Collector → Jaeger | Outbound | Backend-dependent |
| Alloy → Grafana Cloud | Outbound HTTPS | 443 |

### AWS Security Groups

For secure monitoring:

- Allow inbound port 9100 only from the Prometheus server's Security Group.
- Allow required outbound connections to observability backends.
- Avoid exposing monitoring ports publicly.
- Prefer private networking or secure outbound HTTPS connections.

## 6. Pull vs Push Architecture

| Feature | Pull | Push |
|---|---|---|
| Communication | Monitoring system requests data | Agent sends data |
| Example | Prometheus | Grafana Alloy |
| Common use | Metrics | Logs and traces |
| Network requirement | Monitoring server must reach exporter | Server needs outbound connectivity |

## 7. Typical Production Observability Stack

| Requirement | Tool |
|---|---|
| Linux metrics | Node Exporter |
| Metrics collection | Prometheus |
| Log collection | Grafana Alloy |
| Log storage | Loki |
| Trace instrumentation | OpenTelemetry SDK |
| Trace collection | OpenTelemetry Collector |
| Trace storage | Jaeger / Tempo |
| Visualization | Grafana |

## 8. Key Takeaways

1. EC2 does not need a public IP to support observability.
2. Exporters expose metrics through HTTP endpoints.
3. Prometheus generally pulls metrics from exporters.
4. Log agents read files and forward logs to Loki.
5. OpenTelemetry collects and forwards application traces.
6. Security Groups control which network connections are allowed.
7. Private networking is preferred for internal monitoring.
8. Outbound HTTPS is commonly used for external observability platforms.

**Remember:**

- Metrics = What is happening?
- Logs = Why did it happen?
- Traces = Where did the request spend its time?

Observability combines all three to understand and troubleshoot production systems.
