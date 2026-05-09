# Interview Cheatsheet — Apple ASE SRE

---

## Python Patterns

### Two Sum
```python
def two_sum(nums, target):
    seen = {}
    for i, n in enumerate(nums):
        comp = target - n
        if comp in seen:
            return (seen[comp], i)
        seen[n] = i
```

### BFS
```python
from collections import deque
def bfs(graph, start):
    visited = set()
    q = deque([start])
    while q:
        n = q.popleft()
        if n in visited:
            continue
        visited.add(n)
        for neighbor in graph.get(n, []):
            q.append(neighbor)
    return visited
```

### DFS — Iterative
```python
def dfs(graph, start):
    visited = set()
    stack = [start]
    while stack:
        n = stack.pop()
        if n in visited:
            continue
        visited.add(n)
        for neighbor in graph.get(n, []):
            stack.append(neighbor)
    return visited
```

### DFS — Recursive
```python
def dfs(node, graph, visited=None):
    if visited is None:
        visited = set()
    if node in visited:
        return
    visited.add(node)
    for neighbor in graph.get(node, []):
        dfs(neighbor, graph, visited)
    return visited
```

### Binary Search
```python
def binary_search(arr, target):
    lo, hi = 0, len(arr) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            lo = mid + 1
        else:
            hi = mid - 1
    return -1
```

### Sliding Window — Fixed
```python
def max_sum_window(arr, k):
    window = sum(arr[:k])
    best = window
    for i in range(k, len(arr)):
        window += arr[i] - arr[i-k]
        best = max(best, window)
    return best
```

### Merge Intervals
```python
def merge(intervals):
    intervals.sort(key=lambda x: x[0])
    result = [intervals[0]]
    for start, end in intervals[1:]:
        if start <= result[-1][1]:
            result[-1][1] = max(result[-1][1], end)
        else:
            result.append([start, end])
    return result
```

### Number of Islands (BFS)
```python
def numIslands(grid):
    seen = set()
    count = 0
    def bfs(r, c):
        q = deque([(r, c)])
        seen.add((r, c))
        while q:
            row, col = q.popleft()
            for nr, nc in [(row-1,col),(row+1,col),(row,col-1),(row,col+1)]:
                if 0 <= nr < len(grid) and 0 <= nc < len(grid[0]):
                    if (nr,nc) not in seen and grid[nr][nc] == "1":
                        seen.add((nr,nc))
                        q.append((nr,nc))
    for r, row in enumerate(grid):
        for c, val in enumerate(row):
            if val == "1" and (r,c) not in seen:
                bfs(r, c)
                count += 1
    return count
```

---

## Data Structures — Quick Reference

| Structure | Use case | Key ops |
|-----------|----------|---------|
| dict | hash map, counting | O(1) get/set |
| set | membership, visited | O(1) add/in |
| deque | BFS queue, sliding window | O(1) popleft |
| heapq | top-k, priority | O(log n) push/pop |
| defaultdict(int) | counting without KeyError | auto 0 |
| Counter | letter/word counts | Counter(s) |

---

## Redis

**What it is:** In-memory key-value store. Microsecond latency.

**Use cases:**
- Caching — store expensive DB query results with TTL
- Sessions — fast lookup, any server in cluster can read
- Rate limiting — atomic INCR counter per user per minute
- Leaderboards — sorted sets, native ranking
- Pub/Sub — lightweight message bus
- Distributed locks — atomic operations

**vs Postgres:**
- Redis = speed + temporary. Lose it = annoying
- Postgres = durability + correctness. Lose it = catastrophic

**Persistence:**
- RDB — periodic snapshots. Fast restore, lose recent writes
- AOF — append only file, logs every write. Durable, slower
- Use both in production

**CAP:** Redis is AP — available and partition tolerant. Eventually consistent by default.

**Cache invalidation:**
- Delete key on Postgres update (explicit invalidation)
- TTL on every key (safety net)
- Use both

**Data types:** string, list, set, sorted set, hash, pub/sub

---

## CAP Theorem

A distributed system can only guarantee 2 of 3:
- **C** — Consistency: all nodes see same data
- **A** — Availability: system always responds
- **P** — Partition tolerance: survives network splits

P is unavoidable → real choice is **CP vs AP**

- **CP** — banks, payment systems, inventory. Consistency over availability.
- **AP** — Redis, Cassandra, social feeds. Availability over consistency.

---

## ACID vs BASE

**ACID** — traditional DBs (Postgres, MySQL)
- **A**tomicity — all or nothing
- **C**onsistency — valid state to valid state
- **I**solation — concurrent transactions don't interfere
- **D**urability — committed data survives crashes

**BASE** — distributed DBs (Redis, Cassandra)
- **B**asically Available
- **S**oft state
- **E**ventually consistent

---

## Isolation Levels (weakest → strongest)

| Level | What you can see |
|-------|-----------------|
| Read Uncommitted | Uncommitted data from other transactions |
| Read Committed | Only committed data (default in most DBs) |
| Repeatable Read | Same row reads same value in same transaction |
| Serializable | Transactions run as if sequential. Safest, slowest |

---

## RED / USE / SLI / SLO / SLA

**RED** — for services
- **R**ate — requests/sec
- **E**rrors — failed requests %
- **D**uration — latency, use p99 not average

**USE** — for resources
- **U**tilization — how busy (CPU %)
- **S**aturation — work queuing up (load average)
- **E**rrors — hardware/resource errors

**SLI** — the actual metric measured (the number)
**SLO** — internal target set on that metric (99.9% availability)
**SLA** — customer contract, weaker than SLO (99.5%)

**Error budget** = 1 − SLO
- 99.9% SLO → 43 min/month downtime allowed
- Budget healthy → ship fast
- Budget burning → freeze releases

**Prometheus** — collects metrics, stores time series
**Grafana** — visualizes metrics, builds dashboards
**Alertmanager** — fires alerts when SLO thresholds breached

---

## TCP/IP

**TCP** — connection oriented, reliable, ordered
- 3-way handshake: SYN → SYN-ACK → ACK
- Sequence numbers start random (ISN) — prevents injection attacks
- Window size — flow control, how much data before ACK needed
- Retransmission — resends if no ACK received
- SYN flood — DDoS attack, sends SYN never sends ACK

**UDP** — connectionless, fast, no guarantee
- Use for: video calls, gaming, DNS, streaming
- Dropped packet = skip and continue, not retransmit

---

## TLS / mTLS

**TLS:**
1. Server sends certificate (contains public key, signed by CA)
2. Client verifies CA signature
3. Client encrypts session key with server's public key
4. Server decrypts with private key
5. Both use symmetric key (AES) for data

**mTLS:** Both sides present certificates. Used in microservices, zero-trust.

**Asymmetric** (RSA/ECDSA) — for key exchange and identity
**Symmetric** (AES) — for bulk data encryption (fast)

**CA** — Certificate Authority. Signs certs. Browser trusts CA list.
**Internal CA** — company runs own CA for mTLS between services.

**JWT:**
- Server issues token after login
- Client sends token in every request header
- Server validates signature — stateless, no DB lookup
- Store in httpOnly cookie (safe from XSS)
- Short TTL (15min-24hr) + refresh token for new JWT

---

## Kubernetes — Quick Reference

**Pod** — smallest unit. One or more containers.
**Deployment** — manages pods, handles rolling updates, replicas.
**Service** — stable network endpoint for pods (ClusterIP, NodePort, LoadBalancer)
**Ingress** — routes external HTTP traffic to services
**ConfigMap** — inject config into pods
**Secret** — inject sensitive data (passwords, tokens)
**Namespace** — logical isolation within cluster
**Node** — physical/virtual machine running pods
**kubectl get pods** — list pods
**kubectl describe pod <name>** — debug a pod
**kubectl logs <pod>** — view logs
**kubectl exec -it <pod> -- bash** — shell into pod

---

## Docker — Quick Reference

```bash
docker build -t myapp .          # build image
docker run -p 8080:80 myapp      # run container
docker ps                        # list running containers
docker exec -it <id> bash        # shell into container
docker logs <id>                 # view logs
docker stop <id>                 # stop container
```

**Dockerfile key instructions:**
```dockerfile
FROM python:3.11
WORKDIR /app
COPY . .
RUN pip install -r requirements.txt
CMD ["python", "app.py"]
```

---

## Terraform — Quick Reference

Infrastructure as code. Declarative.

```hcl
resource "aws_instance" "web" {
  ami           = "ami-12345"
  instance_type = "t2.micro"
}
```

```bash
terraform init      # initialize
terraform plan      # preview changes
terraform apply     # apply changes
terraform destroy   # tear down
```

**State file** — tracks what's deployed. Store in S3 with locking via DynamoDB.

---

## AWS — Quick Reference

**EC2** — virtual machines
**EBS** — block storage attached to EC2 (like a hard drive)
**S3** — object storage, unlimited, cheap
**RDS** — managed relational DB (Postgres, MySQL)
**ElastiCache** — managed Redis/Memcached
**ELB** — load balancer
**VPC** — private network, subnets, security groups
**IAM** — identity and access management, roles and policies
**CloudWatch** — monitoring and alerting

---

## OSI Model — What Matters

| Layer | Name | Protocols |
|-------|------|-----------|
| 7 | Application | HTTP, DNS, SSH, TLS |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, IPSec |
| 2 | Data Link | Ethernet, MAC |

**IPSec** — encrypts at Layer 3. How VPNs work.
**TLS** — encrypts at Layer 7. How HTTPS works.
**QUIC** — UDP + TLS 1.3 baked in. HTTP/3. Fast.
**MPLS** — Layer 2.5, private virtual circuits, enterprise WAN (your AT&T background).

---

## Things to Say in the Interview

- "I'd instrument this with RED metrics — rate, errors, and p99 latency"
- "I'd set an SLO and track the error budget"
- "Redis is the right call here — it's ephemeral data, speed matters, we can tolerate eventual consistency"
- "This is a CAP tradeoff — for payment data we need CP, for caching we're fine with AP"
- "I'd use mTLS for service-to-service auth in Kubernetes"
- "Prometheus for metrics collection, Grafana for visualization, Alertmanager for SLO-based paging"
- Mention Apple Pay and Fenrir experience — you've lived this at scale

---

## You're ready. Go get it.
