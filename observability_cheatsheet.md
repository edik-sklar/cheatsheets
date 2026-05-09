# Observability Cheatsheet

---

## RED — For Services (APIs, Microservices)

| Letter | Metric | What it means |
|--------|--------|---------------|
| **R** | Rate | Requests per second hitting your service |
| **E** | Errors | Failed requests — 5xx, timeouts, exceptions |
| **D** | Duration | How long requests take — measure p50, p95, p99 |

### Signals
- **Rate drops** → upstream broken, traffic not reaching you
- **Errors spike** → service is broken, page someone now
- **p99 latency climbs** → slow DB, memory pressure, downstream degraded

### Why p99 and not average?
> 99 requests take 10ms, 1 request takes 10s → average = ~110ms (looks fine).  
> But 1% of users are suffering. At 1000 req/s that's 10 users/sec having a terrible experience.  
> **Percentiles expose the tail. Averages hide it.**

---

## USE — For Resources (CPU, Memory, Disk, Network)

| Letter | Metric | What it means |
|--------|--------|---------------|
| **U** | Utilization | How busy is the resource? CPU at 90% = running hot |
| **S** | Saturation | Is work queuing up? Load average, queue depth, swap |
| **E** | Errors | Disk I/O errors, dropped packets, hardware faults |

### Signals
- **Utilization > 80% sustained** → capacity problem coming
- **Queue growing** → resource can't keep up, failures incoming
- **Any hardware errors** → investigate immediately

---

## How RED and USE Work Together

> RED tells you **something is wrong**. USE tells you **why**.

```
RED: p99 latency up → USE: CPU at 95% → Scale up or fix hot path
RED: error rate spike → USE: disk I/O errors → Replace disk, restore from backup
```

---

## SLI / SLO / SLA

```
SLI (what you measure) → SLO (internal target) → SLA (customer contract)
```

| Term | What it is | Example |
|------|-----------|---------|
| **SLI** | The actual metric. A number. Fact, not judgment. | 99.3% of requests succeeded |
| **SLO** | Internal target set by your team. The line you don't cross. | Availability ≥ 99.9% over 30 days |
| **SLA** | Legal contract with customer. Weaker than SLO — buffer needed. | 99.5% uptime or refund |

### Why SLO is stricter than SLA
SLO = 99.9% | SLA = 99.5%  
The gap is your **safety buffer** — fix issues before customers ever feel them.

---

## Error Budget

```
Error budget = 1 − SLO
```

| SLO | Budget | Downtime allowed/month |
|-----|--------|----------------------|
| 99.9% | 0.1% | ~43 minutes |
| 99.99% | 0.01% | ~4.3 minutes |

- **Budget healthy** → ship features fast, take risks
- **Budget burning** → freeze releases, focus on reliability

### Apple Pay Example
- **SLI:** % of payment transactions succeeding within 2 seconds
- **SLO:** 99.95% of transactions succeed within 2s over 30 days
- **SLA:** 99.9% uptime guaranteed to merchants — breach = financial penalty
- **Error budget:** ~21 min/month. One bad deploy burns 10 min. Ship carefully.
