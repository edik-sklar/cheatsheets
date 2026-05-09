RED — for services (APIs, microservices)
R — Rate
Requests per second
How much traffic is hitting your service right now.
http_requests_total / 60s
Signal: Rate drops suddenly → upstream broken, traffic not reaching you, or users gone.
E — Errors
Failed requests
HTTP 5xx, exceptions, timeouts — anything that isn't a successful response.
http_errors_total / http_requests_total = error rate %
Signal: Error rate spikes → service is broken. Page someone now.
D — Duration
Request latency
How long requests take. Measured as percentiles — p50, p95, p99. Not averages (averages hide pain).
p99 latency = 99% of requests finish within X ms
Signal: p99 climbs → slow DB query, memory pressure, downstream dependency degraded.
USE — for resources (CPU, memory, disk, network)
U — Utilization
How busy is the resource?
Percentage of time the resource is busy. CPU at 90% = running hot.
node_cpu_seconds_total — disk_io_time
Signal: Utilization > 80% sustained → capacity problem coming.
S — Saturation
Is work queuing up?
Even if a resource isn't 100% busy, work might be piling up waiting. Load average, queue depth, memory swap.
load average > num CPUs = saturated
Signal: Queue growing → resource can't keep up, things will start failing.
E — Errors
Hardware/resource errors
Disk I/O errors, network packet drops, memory ECC errors. Often silent until catastrophic.
node_disk_io_time_seconds_total — dropped packets
Signal: Any errors → investigate immediately, hardware may be failing.