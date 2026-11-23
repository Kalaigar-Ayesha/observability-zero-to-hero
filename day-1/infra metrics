
# Infrastructure Monitoring (Metrics) – Short Notes

Infrastructure monitoring tracks server resources to ensure applications run smoothly and efficiently.

The four main resources are **CPU, Memory, Network, and Disk**.

---

## 1. CPU Monitoring

**CPU (Central Processing Unit)** is the brain of the server. It processes instructions, calculations, application requests, and background tasks.

### Important Concepts

- **Measured in:** Percentage (%)
- **CPU Cores:** Number of processing units.
- **CPU Utilization:** Percentage of CPU capacity being used.
- **Load Average:** Number of tasks running or waiting for resources over 1, 5, and 15 minutes.

### CPU Usage Levels

| Usage | Interpretation |
|---|---|
| 0–30% | Low |
| 30–70% | Normal / Moderate |
| 70–85% | High |
| 85–95% | Very High |
| 95–100% | Critical |

### Linux Commands

```bash
top
htop
uptime
mpstat -P ALL 1
```

---

## 2. Memory Monitoring (RAM)

**RAM (Random Access Memory)** is temporary storage used by running applications and processes.

### Important Concepts

- **Measured in:** MB, GB, Percentage.
- **Used:** Memory currently occupied.
- **Free:** Completely unused memory.
- **Available:** Estimated memory available for applications.
- **Swap:** Disk space used as an extension of RAM when memory pressure increases.
- **OOM (Out Of Memory):** Occurs when the system cannot satisfy memory requirements and may kill processes.

### Memory Usage Levels

| Usage | Interpretation |
|---|---|
| 0–50% | Normal |
| 50–75% | Moderate |
| 75–85% | High |
| 85–95% | Very High |
| 95–100% | Critical |

### Linux Commands

```bash
free -h
top
vmstat 1
ps aux --sort=-%mem | head
```

---

## 3. Network Monitoring

Network monitoring tracks data communication between servers, users, databases, and external services.

### Important Network Metrics

| Metric | Meaning |
|---|---|
| RX (Receive) | Incoming traffic |
| TX (Transmit) | Outgoing traffic |
| Bandwidth | Maximum data transfer capacity |
| Latency | Time taken for data to travel |
| Packet Loss | Percentage of packets that fail to reach their destination |

### Network Units

- **bps:** Bits per second
- **Kbps:** Kilobits per second
- **Mbps:** Megabits per second
- **Gbps:** Gigabits per second
- **Latency:** Milliseconds (ms)

**Remember:**

1 Byte = 8 bits

Example:

100 Mbps = 12.5 MB/s (theoretical maximum).

### Linux Commands

```bash
ip -s link
iftop
nload
ss -tulnp
ping google.com
```

---

## 4. Disk Monitoring (Storage)

Disk monitoring tracks storage space and disk read/write performance.

### A. Disk Space

- **Total:** Total storage capacity.
- **Used:** Occupied storage.
- **Free:** Remaining storage.
- **Inode:** Data structure used to store file metadata. Inodes can run out even when disk space remains.

### B. Disk I/O (Input/Output)

| Metric | Meaning |
|---|---|
| IOPS | Input/Output operations per second |
| Read Throughput | Data read per second |
| Write Throughput | Data written per second |
| I/O Latency | Time taken for disk operations |
| I/O Utilization | How busy the disk is |

### Linux Commands

```bash
df -h
df -i
du -sh /var/log
iostat -xz 1
ncdu /
```

---

## 5. Quick Revision Table

| Resource | What It Monitors | Unit |
|---|---|---|
| CPU | Processing usage | % |
| Memory | RAM consumption | MB / GB / % |
| Network | Data transfer | Mbps / Gbps |
| Disk | Storage consumption | GB / TB / % |
| IOPS | Disk operations | Operations/sec |
| Latency | Response time | ms |
| Packet Loss | Lost network packets | % |

---

## 6. Monitoring Tools

| Tool | Purpose |
|---|---|
| [Prometheus](https://prometheus.io/) | Collects and stores metrics |
| [Grafana](https://grafana.com/) | Visualizes metrics in dashboards |
| [Node Exporter](https://github.com/prometheus/node_exporter) | Exposes Linux system metrics |
| [Alertmanager](https://prometheus.io/docs/alerting/latest/alertmanager/) | Manages and routes alerts |

---

## 7. Prometheus + Grafana Architecture

```text
Linux Server
(CPU / RAM / Network / Disk)
        |
        v
   Node Exporter
   (Exposes Metrics)
        |
        v
    Prometheus
 (Collects & Stores Metrics)
        |
        +------------------+
        |                  |
        v                  v
     Grafana           Alertmanager
   (Dashboards)       (Notifications)
```

### Component Responsibilities

1. **Node Exporter:** Collects and exposes Linux system metrics.
2. **Prometheus:** Scrapes, stores, and evaluates metrics.
3. **Grafana:** Displays metrics through dashboards and graphs.
4. **Alertmanager:** Handles and routes alert notifications.

---

## 8. Example Production Alert Thresholds

| Metric | Warning | Critical |
|---|---|---|
| CPU | 85% | 95% |
| Memory | 85% | 95% |
| Disk | 80% | 90% |
| Packet Loss | 1% | 2% |

> **Note:** These are example thresholds. Actual thresholds depend on workload and infrastructure.

---

## 9. Key Takeaways

Monitoring helps DevOps engineers:

- Identify resource bottlenecks.
- Troubleshoot performance issues.
- Detect unusual resource consumption.
- Prevent infrastructure downtime.
- Maintain application availability.
- Improve infrastructure performance.

**Remember:** Infrastructure monitoring is not just about watching metrics. It is about identifying problems, understanding their root causes, and resolving them before they affect users.
