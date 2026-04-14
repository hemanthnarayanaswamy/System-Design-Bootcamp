# System Design Mastery Roadmap — SRE / Platform Staff Track

> **Profile**: Fresh beginner → Staff-level interview ready
> **Target**: SRE / Platform staff roles at Fortune 500 (infra depth, reliability, observability focus)
> **Stack**: Python + AWS + open-source tooling
> **Scope**: Both High-Level Design (HLD) _and_ Low-Level Design (LLD)
> **Style**: Balanced theory + practical, not paper-heavy
> **Pace**: 5 hrs/week, open-ended
> **Estimated duration**: 8–12 months to staff-interview-ready

---

## How to Use This Roadmap

1. **System design can't be learned by reading alone.** You must build, diagram, and defend designs aloud.
2. **Three modes every week**: _learn_ (read/watch), _build_ (code a piece), _design_ (whiteboard a system from scratch).
3. **Keep two repos**: `system-design-projects/` (the 12 builds) and `system-design-journal/` (whiteboard designs, diagrams, trade-off notes).
4. **Draw everything.** Use [Excalidraw](https://excalidraw.com/) or [draw.io](https://draw.io). An interviewer will hand you a whiteboard — you need to be fluent.
5. **Talk out loud.** Staff-level interviews test communication as much as knowledge. Record yourself explaining a design. Watch it back. Cringe. Improve.

---

## Phase 0 — What "System Design" Actually Means (Week 1)

Before the roadmap, calibrate your mental model.

**Two kinds of interviews, both expected at staff:**

|           | HLD (High-Level Design)                      | LLD (Low-Level Design)                            |
| --------- | -------------------------------------------- | ------------------------------------------------- |
| Scope     | Distributed system, whole product            | One service, one module                           |
| Focus     | Scale, data flow, storage choice, trade-offs | Classes, methods, patterns, APIs                  |
| Example   | "Design Twitter"                             | "Design a parking lot / rate limiter / LRU cache" |
| Output    | Boxes-and-arrows diagram + numbers           | Class diagram + code stubs                        |
| Key skill | Trade-off articulation                       | Clean OOP, SOLID, patterns                        |

**Staff-level bar** (what separates you from senior):

- You quantify: "This needs ~50 k RPS at p99 < 100 ms, so we need ~X shards."
- You own failure modes, not just happy paths.
- You reason about blast radius, rollback, migrations, cost.
- You compare two valid designs and defend your choice with numbers, not opinion.
- You push back on the problem statement when it's ambiguous or wrong.

### Deliverable

A one-page personal "what I already know / don't know" gap map. Update monthly.

---

## Phase 1 — Networking, HTTP & The Web (Weeks 2–5)

**Goal**: Fluent in the substrate every distributed system runs on.

### Topics

1. **TCP/IP refresher** — handshake, TIME_WAIT, congestion control (cubic, BBR), connection pooling
2. **DNS** — recursive vs authoritative, TTL, geo-DNS, Route53 routing policies
3. **HTTP/1.1, HTTP/2, HTTP/3 (QUIC)** — head-of-line blocking, multiplexing, 0-RTT
4. **TLS** — handshake, SNI, session resumption, mTLS, cert chains, rotation
5. **Load balancers** — L4 vs L7; round-robin, least-conn, consistent hash, maglev
6. **Reverse proxies & API gateways** — Nginx, Envoy, AWS ALB/NLB, API Gateway
7. **CDN** — edge caching, cache keys, invalidation, origin shield, CloudFront
8. **WebSockets, SSE, long-polling** — when to use which
9. **gRPC vs REST vs GraphQL** — trade-offs, streaming, schema evolution
10. **Idempotency & retries** — idempotency keys, exponential backoff, jitter, circuit breakers

### Labs (build, don't just read)

- [ ] Capture and annotate a full TLS 1.3 handshake with Wireshark.
- [ ] Deploy an ALB in front of two EC2 instances; configure health checks; kill one; observe.
- [ ] Put CloudFront in front of an S3 static site; measure latency from 3 geographies.
- [ ] Write a Python client using `httpx` with exponential backoff + jitter + idempotency-key header.
- [ ] Compare throughput: Flask vs FastAPI vs gRPC (Python) for the same endpoint on your laptop.

### Interview questions (answer aloud)

1. Walk me through what happens when `curl https://example.com` runs — DNS → TCP → TLS → HTTP → response.
2. Why does HTTP/2 help page load time? Why doesn't it always?
3. When would you choose gRPC over REST? When not?
4. A client hammers your API with retries during an outage. What breaks? How do you protect the server?
5. Explain the trade-offs of L4 vs L7 load balancing.

### Checkpoint

You can draw — from memory — the path of a packet from browser to a multi-region web app, labeling every hop: DNS, CDN edge, regional LB, service mesh proxy, app server, database.

---

## Phase 2 — Databases Deep Dive (Weeks 6–12)

**Goal**: Pick the right database _with reasons_. This is the #1 discriminator in system design interviews.

### 2A — Storage fundamentals

1. **How a disk works** — HDD vs SSD vs NVMe; sequential vs random I/O; fsync
2. **Indexes** — B-tree, B+ tree, LSM tree, hash index, bitmap, inverted, covering
3. **Page cache & buffer pool** — OS cache vs DB cache; why double-buffering hurts
4. **Write-ahead logging (WAL)** — redo/undo, checkpoints, crash recovery
5. **MVCC** — how Postgres/MySQL avoid read locks

### 2B — Relational (OLTP)

6. **SQL internals** — query planner, EXPLAIN, index choice, join algorithms (nested loop, hash, merge)
7. **Transactions & isolation levels** — Read Uncommitted / Committed / Repeatable Read / Serializable; anomalies each prevents
8. **Replication** — sync vs async, primary-replica, failover, replication lag
9. **Partitioning / sharding** — range, hash, directory-based; hot shards
10. **Postgres vs MySQL** — concrete differences (MVCC strategies, replication, extensions)
11. **Amazon RDS / Aurora** — what Aurora actually does differently (storage disaggregation)

### 2C — NoSQL & beyond

12. **Key-value** — Redis, DynamoDB, Memcached
13. **Document** — MongoDB, DynamoDB document
14. **Wide-column** — Cassandra, DynamoDB, HBase, Bigtable
15. **Graph** — Neo4j; when you actually need one
16. **Time-series** — InfluxDB, TimescaleDB, Prometheus TSDB
17. **Search** — Elasticsearch/OpenSearch; inverted index, sharding, relevance
18. **Vector** — pgvector, Pinecone, Weaviate (AI-era table stakes)
19. **Object storage** — S3 semantics, consistency model (strong read-after-write since 2020), storage classes

### 2D — Caching

20. **Cache strategies** — cache-aside, write-through, write-back, write-around
21. **Cache pitfalls** — stampede, penetration, avalanche, inconsistency
22. **Redis deep dive** — data structures, persistence (RDB/AOF), Cluster, Sentinel
23. **TTL & eviction** — LRU, LFU, allkeys vs volatile

### Labs

- [ ] Spin up Postgres locally. Create a 10 M row table. Query without an index, measure; add index, measure. `EXPLAIN ANALYZE` both.
- [ ] Demonstrate every SQL anomaly (dirty read, non-repeatable, phantom, write skew) in Postgres by manually setting isolation levels.
- [ ] Set up Postgres streaming replication (primary + replica) on Docker. Simulate failover.
- [ ] Build a tiny KV store in Python using an LSM-tree approach (memtable + SSTables + compaction). ~300 lines.
- [ ] Deploy DynamoDB on AWS; design the schema for a chat app using single-table design; load test with `boto3`.
- [ ] Build a cache-aside layer with Redis in front of Postgres for a blog post API; measure cache hit rate.
- [ ] Break a cache: induce thundering herd on Redis expiry; fix with jittered TTL + request coalescing.
- [ ] Set up OpenSearch; index 1 M documents; compare exact vs fuzzy search performance.

### Interview questions

1. SQL or NoSQL for \[use case\]? Defend with at least 3 dimensions.
2. Explain LSM-tree vs B-tree. When is each the right choice?
3. A query is slow. Walk me through diagnosis.
4. Your replica is 30 s behind primary. What breaks? How do you detect and mitigate?
5. Design the schema for \[Instagram feed / Uber rides / Slack messages\] in Postgres _and_ DynamoDB. Compare.
6. Explain MVCC. Why does Postgres have VACUUM?
7. How do you shard a relational database? When does sharding become a nightmare?
8. Explain eventual consistency. Give a concrete user-visible consequence.
9. Cache-aside vs write-through — trade-offs?
10. Redis vs Memcached — when each wins?

### Checkpoint

Given any feature request, you can propose a storage design in 5 minutes, listing: data model, read/write patterns, expected QPS, sharding strategy, consistency requirement, backup/DR.

---

## Phase 3 — Distributed Systems Fundamentals (Weeks 13–18)

**Goal**: Speak the language of distributed systems without hand-waving.

### Topics

1. **CAP theorem** — what it actually means, what it doesn't
2. **PACELC** — the more useful successor
3. **Consistency models** — strict, linearizable, sequential, causal, eventual; read-your-writes
4. **Replication strategies** — primary-replica, multi-primary, leaderless (Dynamo-style)
5. **Consensus** — why it's hard; Paxos (overview), Raft (understand deeply), ZAB (Zookeeper)
6. **Quorums** — R + W > N; sloppy quorums; hinted handoff
7. **Conflict resolution** — last-write-wins, version vectors, CRDTs
8. **Clocks** — wall clock pitfalls, logical clocks, vector clocks, Google TrueTime
9. **Failure detection** — heartbeats, phi-accrual, SWIM protocol (gossip)
10. **Distributed transactions** — 2PC, 3PC, Saga pattern, outbox pattern
11. **Idempotency & exactly-once** — why "exactly-once" is a lie; effectively-once in practice
12. **Split-brain** — causes, fencing tokens, STONITH

### Light paper reading (one per week, skim-level)

- Amazon Dynamo (2007) — leaderless replication
- Google Bigtable (2006) — wide-column foundations
- Raft (2014) — consensus you can actually understand
- Kafka (LinkedIn, 2011) — the log abstraction
- Google Spanner (2012) — TrueTime & global transactions
- _Optional_: Lamport's "Time, Clocks, and the Ordering of Events" (1978)

### Labs

- [ ] Implement Raft leader election in Python (3-node toy). Don't build log replication yet — just elections + heartbeats.
- [ ] Extend it with log replication. Force partitions; observe behavior.
- [ ] Implement a simple CRDT (G-Counter and LWW-Register). Test under network partitions.
- [ ] Build a 2-phase-commit coordinator + participants. Induce coordinator crash mid-commit; observe blocked participants.
- [ ] Replace 2PC with a Saga for the same workflow. Compare recovery behavior.
- [ ] Use `aws-sdk` to implement the outbox pattern with DynamoDB streams → SQS.

### Interview questions

1. Explain CAP. Now explain why CAP is an oversimplification.
2. You need to replicate user profiles across 3 regions. Walk me through options.
3. Your system has "exactly-once processing." Prove it to me. (Trick — help the interviewer correct their own question.)
4. Design a leader election mechanism. What are the failure modes?
5. What is a fencing token? Why does it matter?
6. When do you choose CP over AP? Give a concrete system for each.
7. Explain linearizability vs serializability. They're different.
8. A node was partitioned for 10 minutes and rejoined. What could go wrong?
9. Saga vs 2PC — trade-offs?
10. Why are distributed systems hard? (Staff-level expected answer: failure partial-ness, time, ordering, state divergence.)

### Checkpoint

You can explain Raft in 10 minutes with a hand-drawn diagram — leader election, log replication, commit, safety — without referring to notes.

---

## Phase 4 — Messaging, Streaming & Event-Driven Architecture (Weeks 19–22)

**Goal**: Async is how real systems scale. Master the primitives.

### Topics

1. **Message queues vs log streams** — SQS vs Kafka mental model
2. **Delivery semantics** — at-most-once, at-least-once, effectively-once
3. **Kafka deep dive** — topics, partitions, consumer groups, offsets, ISR, exactly-once semantics
4. **SQS, SNS, EventBridge** — AWS messaging stack
5. **Kinesis vs Kafka** on AWS
6. **Dead letter queues & poison pills**
7. **Backpressure** — how to not drown your consumers
8. **Change Data Capture (CDC)** — Debezium, DynamoDB Streams, logical replication
9. **Event sourcing & CQRS** — when it's worth the complexity
10. **Stream processing** — Flink, Kafka Streams, Spark Streaming (conceptual)
11. **Outbox pattern** — the practical way to get atomic DB + message

### Labs

- [ ] Stand up Kafka (Redpanda for simplicity) in Docker. Produce + consume. Crash a broker; observe.
- [ ] Build a Python producer/consumer with proper error handling, retries, DLQ.
- [ ] Implement consumer group rebalancing: run 3 consumers, kill one, observe partition reassignment.
- [ ] Build an outbox pattern: Postgres → CDC (Debezium or polling) → Kafka → downstream consumer.
- [ ] Compare SQS (standard vs FIFO) vs Kafka for the same workload; quantify latency & throughput.
- [ ] Build an event-sourced shopping cart: state is derived from events in a Kafka topic. Include snapshots.

### Interview questions

1. Kafka vs SQS vs Kinesis — when each?
2. Your consumer is lagging by 4 hours. Diagnose.
3. How does Kafka achieve exactly-once? (Idempotent producer + transactional writes + `read-committed` consumer.)
4. What is a poison pill message? How do you handle it?
5. Design notifications for 100 M users. (Fan-out: push vs pull.)
6. Explain the outbox pattern. Why not just publish-then-commit?
7. How do you handle ordering when you have multiple partitions?
8. What does "backpressure" mean? Give two ways to implement it.
9. When should a team NOT adopt event sourcing?

### Checkpoint

You can design and defend an async pipeline end-to-end: producer → broker → consumer with retries, DLQ, idempotency, ordering guarantees, and lag monitoring.

---

## Phase 5 — Scale Patterns & Performance (Weeks 23–27)

**Goal**: Handle millions of users with the same confidence you handle hundreds.

### Topics

1. **Back-of-envelope math** — power-of-10 memorization; QPS → storage → bandwidth
2. **Vertical vs horizontal scaling** — when vertical is underrated
3. **Stateless vs stateful services** — why statelessness is cheaper
4. **Sharding strategies** — consistent hashing, range, geographic
5. **Read-heavy vs write-heavy** patterns — caching, read replicas, CQRS
6. **Write amplification** — writing one record touches N systems
7. **Fan-out** — push vs pull vs hybrid (Twitter home timeline)
8. **Rate limiting** — token bucket, leaky bucket, fixed window, sliding window
9. **Connection pooling** — DB connections are expensive
10. **Bulkheads & circuit breakers** — isolating failures
11. **Hotspots** — celebrity problem, hot shard, mitigation
12. **Multi-region** — active-passive, active-active; data residency; latency trade-offs
13. **Cost at scale** — the "oh god AWS bill" phase

### Back-of-envelope numbers to memorize

| Operation             | Latency    |
| --------------------- | ---------- |
| L1 cache ref          | 1 ns       |
| L2 cache ref          | 4 ns       |
| Main memory ref       | 100 ns     |
| SSD random read       | 100 µs     |
| HDD seek              | 10 ms      |
| Round trip same DC    | 0.5 ms     |
| Round trip cross-US   | 70 ms      |
| Round trip US ↔ EU    | 100–150 ms |
| TCP handshake same DC | ~1 ms      |
| TLS handshake         | ~20 ms     |

| Throughput                           | Value            |
| ------------------------------------ | ---------------- |
| 1 server (modest)                    | ~1–10 k RPS      |
| 1 Postgres (well-tuned, single node) | ~5–10 k writes/s |
| 1 Redis instance                     | ~100 k ops/s     |
| 1 Kafka broker                       | ~100 k msg/s     |
| 1 Gbps link                          | ~125 MB/s        |

### Labs

- [ ] Do back-of-envelope for 10 systems: Twitter, YouTube, Uber, WhatsApp, Instagram, Netflix, Dropbox, Slack, Stripe, Google Drive. Storage, QPS, bandwidth. Write in your journal.
- [ ] Build a rate limiter in Python: all 4 algorithms (fixed window, sliding log, sliding window counter, token bucket). Benchmark.
- [ ] Implement consistent hashing in Python; visualize the ring; simulate node add/remove rebalance cost.
- [ ] Build a circuit breaker library; integrate with `httpx`; trigger it with a flaky downstream.
- [ ] Load-test one of your earlier projects with `locust` or `k6`. Find the knee of the curve. Scale it out.

### Interview questions

1. Estimate storage for YouTube (all of it, order of magnitude).
2. Twitter's fan-out: push vs pull. Justin Bieber has 100 M followers. How do you handle it?
3. Your DB is overloaded on reads. List 5 things to try, in order of invasiveness.
4. A rate limiter must work across 100 app servers. Design it.
5. You have a hot shard. Name 4 strategies to fix.
6. Design for active-active across 2 regions. What goes wrong? What needs to be designed region-local?
7. Explain consistent hashing. Why "consistent"? Compare to modulo hashing.
8. When is vertical scaling the right answer? (It often is.)

### Checkpoint

Given any "design at X scale" prompt, you can produce QPS/storage/bandwidth numbers within 90 seconds.

---

## Phase 6 — Reliability, Observability & the SRE Core (Weeks 28–33)

**This is your differentiator for SRE/Platform staff roles. Spend extra time here.**

### Topics

1. **SLI / SLO / SLA / Error budgets** — the Google SRE framework
2. **Availability math** — serial vs parallel failure, the "nines"
3. **Failure modes** — cascading failures, retry storms, thundering herds, metastable states
4. **Graceful degradation** — feature flags, fallbacks, load shedding
5. **Chaos engineering** — principles, what to break, blast radius
6. **Deployment strategies** — blue-green, canary, rolling, feature flags, dark launches
7. **Progressive delivery** — Argo Rollouts, Flagger, LaunchDarkly patterns
8. **Observability — the three pillars**
   - **Metrics** — Prometheus, CloudWatch, RED & USE methods
   - **Logs** — structured, centralized, queryable (Loki, ELK, CloudWatch Logs)
   - **Traces** — OpenTelemetry, Jaeger, AWS X-Ray
9. **Profiling in prod** — continuous profiling (Pyroscope, Parca)
10. **Incident management** — on-call, paging, runbooks, blameless postmortems
11. **Capacity planning** — utilization targets, headroom, forecasting
12. **Disaster recovery** — RTO, RPO, backup strategies, restore drills
13. **Autoscaling** — HPA/VPA in K8s, AWS Auto Scaling Groups, predictive vs reactive
14. **Multi-AZ and multi-region design** — blast radius, failover, cost
15. **Cost observability** — tagging, FinOps, per-request cost

### Books to read here (even if you skip reading elsewhere)

- **Site Reliability Engineering** (Google, free online)
- **The Site Reliability Workbook** (Google, free online)
- **Observability Engineering** — Majors, Fong-Jones, Miranda

### Labs

- [ ] Define SLIs/SLOs for a Python API you own. Instrument with Prometheus. Build error-budget burn-rate alerts.
- [ ] Set up the full observability stack locally: Prometheus + Grafana + Loki + Tempo + OpenTelemetry collector. Instrument a service with all three pillars.
- [ ] Run a chaos experiment: kill pods randomly with Chaos Mesh (or `chaoskube`); watch SLO burn.
- [ ] Write a runbook for a known failure mode of your service. Test it. Revise.
- [ ] Simulate a cascading failure: one service slow → others retry → retry storm → total outage. Add circuit breakers. Repeat.
- [ ] Do a "GameDay" — deliberately break a project and fix it under time pressure, measure MTTR.
- [ ] Write a blameless postmortem for a (real or simulated) incident. Include timeline, root cause, contributing factors, action items.
- [ ] Set up canary deploy on EKS with Argo Rollouts or Flagger.
- [ ] Build a dashboard (Grafana) that would actually be useful during an incident — not vanity metrics.

### Interview questions

1. Walk me through designing SLOs for a new service.
2. What's the difference between availability and reliability?
3. A service is at 99.9 % — is that good? Depends: good for what, bad for what?
4. Explain error budgets. What do you do when you burn through one?
5. You're on-call. Pager fires. Walk me through your first 10 minutes.
6. What makes a good runbook? A bad one?
7. RED vs USE — when each?
8. A cascading failure took down your payment system. Walk me through the postmortem.
9. Retries are causing an outage to get worse. What do you do, architecturally?
10. How do you capacity-plan for Black Friday (10× normal traffic)?
11. Three pillars of observability — describe each, how they connect, what trace-log correlation gives you.
12. Why is distributed tracing hard in practice? (Sampling, context propagation, vendor lock-in.)
13. Your CEO asks "are we up?" You have 30 seconds. What do you look at?
14. Design a deployment pipeline that prevents bad code from reaching prod.

### Checkpoint

You have a pre-written story for each of these incident archetypes (real or simulated):

- A cascading failure you diagnosed
- A capacity surprise you handled
- A bad deploy you rolled back
- A data loss / integrity issue
- A security or availability trade-off call you made

---

## Phase 7 — Kubernetes, Containers & Cloud-Native Platforms (Weeks 34–38)

**Goal**: Fluent in the platform layer that runs nearly everything now.

### Topics

1. **Container primitives recap** — namespaces, cgroups, overlayfs (from Linux roadmap)
2. **Kubernetes architecture** — control plane (API server, scheduler, controller-manager, etcd), data plane (kubelet, kube-proxy, CNI)
3. **Core objects** — Pod, Deployment, StatefulSet, DaemonSet, Service, Ingress, ConfigMap, Secret, PV/PVC
4. **Networking** — CNI, Services (ClusterIP, NodePort, LoadBalancer), Ingress vs Gateway API, NetworkPolicy
5. **Scheduling** — requests vs limits, affinity, taints/tolerations, PriorityClasses, disruption budgets
6. **Autoscaling** — HPA, VPA, Cluster Autoscaler, KEDA
7. **Operators & CRDs** — the operator pattern; building one (concept)
8. **Service mesh** — Istio, Linkerd; what they buy you vs what they cost
9. **Platform engineering patterns** — paved paths, golden paths, internal developer platforms
10. **GitOps** — ArgoCD, Flux; declarative delivery
11. **Multi-cluster & multi-region** — federation, failover, data gravity
12. **EKS specifics** — IRSA, VPC CNI, Fargate vs EC2 node groups, Karpenter

### Labs

- [ ] Run `kind` or `minikube` locally. Deploy a Python FastAPI app with a Postgres StatefulSet.
- [ ] Deploy an EKS cluster with `eksctl` or Terraform. Use IRSA for pod → S3 access.
- [ ] Build a custom controller in Python with `kopf` — e.g., a CRD that creates Postgres users.
- [ ] Install Istio or Linkerd; enable mTLS; measure overhead.
- [ ] Configure HPA based on custom Prometheus metrics (not just CPU).
- [ ] Break things: delete etcd data, kill a node, network-partition two nodes. Observe, recover, document.
- [ ] Implement GitOps with ArgoCD: PR merges → auto-deploy to staging, manual sync to prod.
- [ ] Use Karpenter for node autoscaling; compare cost vs Cluster Autoscaler.

### Interview questions

1. What happens, end to end, when you `kubectl apply -f deployment.yaml`?
2. Pod stuck in `Pending`. List 8 possible causes.
3. Requests vs limits — what happens if you set one but not the other? What's the right policy?
4. How does a Service actually route traffic to pods? (kube-proxy, iptables vs IPVS, endpoints.)
5. StatefulSet vs Deployment — when to use which?
6. Your cluster's API server is getting hammered. What do you do?
7. Rolling update vs blue-green vs canary in Kubernetes — how do you implement each?
8. When is a service mesh worth the complexity?
9. Explain the operator pattern. Give a case where you'd write one vs use an off-the-shelf operator.
10. How do you design a multi-tenant Kubernetes platform?

### Checkpoint

You can draw the Kubernetes control-plane flow for pod creation, labeling every component and what it does.

---

## Phase 8 — Security in System Design (Weeks 39–41)

**Goal**: Security can't be a bolt-on. Think about it at design time.

### Topics

1. **AuthN vs AuthZ** — OAuth 2.0, OIDC, SAML, JWT (and why JWTs are overused)
2. **Zero trust** — principles, concrete controls
3. **Secrets management** — Vault, AWS Secrets Manager, SOPS, sealed-secrets; rotation
4. **Network security** — VPC design, security groups, NACLs, PrivateLink, bastion-less access
5. **TLS everywhere** — mTLS for service-to-service
6. **Encryption at rest** — KMS, envelope encryption, field-level encryption
7. **Common vulnerabilities** — OWASP Top 10, SSRF, IDOR, injection, auth bypass
8. **Rate limiting & abuse protection** — bots, credential stuffing, DDoS
9. **Multi-tenancy** — data isolation, noisy neighbor, tenant-per-X patterns
10. **Audit logging & compliance basics** — SOC 2, GDPR, HIPAA awareness

### Labs

- [ ] Implement OIDC login for a Python app using Cognito or Auth0.
- [ ] Set up Vault; rotate a Postgres password every hour; have your app pick up the new one.
- [ ] Design and implement field-level encryption with AWS KMS for PII columns in Postgres.
- [ ] Threat-model one of your earlier projects: STRIDE or similar. Write up findings.
- [ ] Configure mTLS between two services with Istio or plain cert-manager.
- [ ] Build a tenant-per-schema and tenant-per-row version of the same app; compare isolation, cost, complexity.

### Interview questions

1. Design session management for a web app serving 100 M users.
2. JWT vs opaque session tokens — trade-offs?
3. How do you rotate a signing key without downtime?
4. What's SSRF? How do you prevent it in a service that fetches user-provided URLs?
5. Multi-tenancy: row-level vs schema vs database vs cluster isolation — compare.
6. Your API returns different errors for "invalid password" vs "unknown user." What's wrong?
7. Design rate limiting that an attacker can't bypass by rotating IPs.

---

## Phase 9 — Low-Level Design (LLD) & Design Patterns (Weeks 42–46)

**Goal**: Write clean, extensible OO code on the spot.

### Topics

1. **SOLID principles** — each one, with concrete Python examples
2. **OOP in Python** — classes, inheritance, composition, dataclasses, ABCs, protocols
3. **GoF design patterns** (the essential ones, not all 23)
   - **Creational**: Factory, Singleton (and why it's overused), Builder
   - **Structural**: Adapter, Decorator, Facade, Proxy
   - **Behavioral**: Strategy, Observer, State, Command, Chain of Responsibility, Iterator
4. **Concurrency patterns** — producer-consumer, reader-writer, worker pool, circuit breaker (Python: `threading`, `asyncio`, `concurrent.futures`)
5. **Testing design** — unit vs integration; mocks, fakes, stubs; contract testing
6. **API design** — REST resource modeling, pagination, versioning, idempotency keys, error formats (RFC 7807)
7. **Domain modeling** — bounded contexts, aggregates, value objects (DDD-lite)

### Classic LLD problems (solve each, code + class diagram)

1. **Parking Lot** — classic OOP test; multiple vehicle types, levels, pricing strategies.
2. **Elevator system** — state machines, scheduling strategies.
3. **LRU Cache** — O(1) get and put; extend to LFU.
4. **Rate Limiter** — multi-algorithm, configurable.
5. **Logging framework** — levels, handlers, async, formatters (study Python's `logging` afterward).
6. **In-memory database** — CRUD + transactions + simple indexes.
7. **Tic-tac-toe / Chess** — rules, board, move validation, extensibility.
8. **Splitwise** — users, groups, expenses, balances, settle-up algorithm.
9. **Vending machine** — state machine, payment, inventory.
10. **Notification service** — strategy (email/SMS/push), retries, templates, opt-out.
11. **Library management** — borrow, return, reserve, late fees.
12. **URL shortener (code-level)** — encoder, storage, collision handling.
13. **Distributed task queue (code-level)** — job class hierarchy, retry policy, backoff.
14. **File-based KV store** — append-only log + compaction.
15. **Concurrent bounded blocking queue** — producer/consumer, thread-safe.

### Labs

- [ ] Solve 5 LLD problems cold, timed at 45 min each. Diagram first, code second, test third.
- [ ] Refactor one of your earlier projects (from any phase) using at least 3 design patterns. Document why each pattern applies.
- [ ] Write contract tests between two of your services using Pact or a simple JSON-schema approach.

### Interview questions

1. Explain Liskov Substitution with a Python example that violates it and one that doesn't.
2. Inheritance vs composition — when each? (Staff answer: composition by default.)
3. When is Singleton an anti-pattern? When is it OK?
4. Strategy vs State pattern — what's the difference? They look identical.
5. Design classes for \[X\] (any of the 15 above).
6. How do you handle cross-cutting concerns (logging, auth, metrics) without polluting business logic?
7. What makes an API "RESTful" vs "HTTP with JSON"? Does it matter?

### Checkpoint

Given any "design classes for X" problem, you produce a clean UML-ish class diagram in 10 minutes and working Python code in 30.

---

## Phase 10 — Classic HLD Interview Problems (Weeks 47–52)

**Goal**: Have a mental template for every "Design X" question.

### The universal 5-step framework (apply to every problem)

1. **Clarify & scope** (3–5 min) — functional requirements, non-functional requirements (scale, latency, availability), what's out of scope.
2. **Estimate** (3–5 min) — users, QPS, storage, bandwidth. Numbers must be written on the board.
3. **High-level design** (10–15 min) — boxes and arrows. Client → LB → services → storage → async backbone. Call out the data flow.
4. **Deep dive** (10–15 min) — let the interviewer pick a component, or you volunteer the juiciest one (usually storage, hotspot handling, or a consistency problem).
5. **Wrap-up** — failure modes, bottlenecks, what you'd revisit, monitoring, cost.

### The 20 classics (build a written design doc for each)

**Tier 1 — fundamentals (master first)**

1. **URL shortener** (TinyURL) — KGS, collisions, analytics, custom aliases.
2. **Pastebin** — storage tiers, expiration, rate limiting.
3. **Key-value store** — consistent hashing, replication, failure detection.
4. **Rate limiter** — distributed, multi-tenant, fairness.
5. **Unique ID generator** — Twitter Snowflake, UUIDs, sortable IDs.

**Tier 2 — feed & social** 6. **Twitter / X** — timeline fan-out, celebrity problem, search. 7. **Instagram** — photo upload, feed, stories. 8. **Facebook News Feed** — ranking, edge cases, real-time. 9. **Reddit / HackerNews** — ranking algorithms, nested comments. 10. **Notification service** — multi-channel, rate-limits, opt-out, priority.

**Tier 3 — real-time & geo** 11. **WhatsApp / Messenger** — online presence, E2E encryption basics, group chat. 12. **Uber / Lyft** — driver-rider matching, geo-indexing (quadtree, geohash, S2, H3), surge pricing. 13. **DoorDash / food delivery** — three-sided marketplace, ETA prediction, dispatch. 14. **Google Maps / directions** — tile serving, routing graph. 15. **Live video streaming (Twitch, YouTube Live)** — ingest, transcode, CDN, chat.

**Tier 4 — content & commerce** 16. **YouTube / Netflix (VOD)** — upload pipeline, CDN, adaptive bitrate, recommendations. 17. **Dropbox / Google Drive** — file sync, delta updates, dedup, conflict resolution. 18. **Amazon e-commerce** — product catalog, search, cart, inventory, order. 19. **Stripe / payment processing** — idempotency, fraud, reconciliation, PCI scope. 20. **Ticketmaster / event booking** — inventory contention, queue-based sales, fairness.

**Tier 5 — infra-flavored (these matter most for SRE/Platform staff)** 21. **Distributed log (Kafka clone)** — partitions, replication, consumer groups. 22. **Distributed task queue (Celery/Sidekiq clone)** — priorities, retries, scheduling. 23. **Metrics system (Prometheus clone)** — TSDB, pull vs push, cardinality. 24. **Log aggregation (ELK/Loki clone)** — ingest, index, query. 25. **Distributed tracing (Jaeger clone)** — context propagation, sampling, storage. 26. **CI/CD system** — pipelines, caching, artifact storage, secrets. 27. **Feature flag service** — targeting rules, low-latency eval, audit. 28. **Object storage (S3 clone)** — partitioning, replication, consistency. 29. **CDN** — edge caching, cache coherence, purging. 30. **Kubernetes scheduler** — constraints, bin-packing, preemption.

### Labs

- [ ] For each of the 30, produce a 2-page design doc: requirements, estimates, architecture diagram, component deep-dive, failure modes. Store in your journal repo.
- [ ] Record yourself whiteboarding 5 of them from scratch. Watch back. Cringe-improve.
- [ ] Schedule mock interviews (see Resources) for at least 10 of them with another human.

### Checkpoint

Given a fresh "design X" prompt, you can fill the whole whiteboard confidently for 45 minutes without running out of things to say — and your interviewer is nodding, not correcting.

---

## Phase 11 — The Hands-On Build Portfolio (12 projects)

These projects run _in parallel_ to the phases above. Each is designed to teach specific concepts and produce a GitHub-worthy artifact. Budget 2–4 weeks per project at 5 hrs/week.

### Project 1 — URL Shortener (Python + AWS)

**Teaches**: API design, DB choice, basic caching, idempotency, rate limiting
**Stack**: FastAPI + Postgres (RDS) + Redis (ElastiCache) + CloudFront
**Stretch**: Add analytics pipeline with Kinesis Firehose → S3 → Athena

### Project 2 — Distributed Rate Limiter Service

**Teaches**: Sliding window + token bucket, Redis Lua scripting, multi-tenant design
**Stack**: FastAPI + Redis Cluster, load-tested with `locust`
**Deliverable**: Benchmark report showing 50 k RPS with p99 < 10 ms

### Project 3 — S3-Backed File Storage Service (Mini Dropbox)

**Teaches**: Chunking, multipart upload, presigned URLs, metadata DB, dedup
**Stack**: FastAPI + Postgres + S3 + SQS for async processing
**Stretch**: Add client-side encryption and versioning

### Project 4 — Async Job Queue (Celery Clone Lite)

**Teaches**: Message semantics, retries, scheduled jobs, worker scaling
**Stack**: Python + Redis Streams (or SQS) + APScheduler
**Stretch**: Add a web UI for job status, implement priority queues

### Project 5 — Observability Stack from Scratch

**Teaches**: The three pillars, OpenTelemetry, SLO calculation
**Stack**: Python services instrumented with OTel → Prometheus + Loki + Tempo + Grafana (all on EKS)
**Deliverable**: SLO dashboard with error-budget burn alerts for your earlier projects

### Project 6 — Chat App (WebSockets + Presence)

**Teaches**: Long-lived connections, presence, fan-out, horizontal scaling of stateful services
**Stack**: FastAPI + WebSockets + Redis pub/sub + DynamoDB
**Stretch**: Group chat, typing indicators, delivery receipts

### Project 7 — Geo-Indexed Ride Matcher

**Teaches**: Geohashing / H3, proximity queries, real-time updates
**Stack**: FastAPI + Redis (geo commands) + Kinesis + DynamoDB
**Stretch**: Simple surge pricing based on supply/demand per cell

### Project 8 — News Feed with Fan-Out

**Teaches**: Push vs pull vs hybrid, hot-key handling, caching tiers
**Stack**: FastAPI + DynamoDB + Redis + SQS for fan-out workers
**Stretch**: Implement both push and pull; benchmark at 1 M users

### Project 9 — Distributed KV Store (Raft-Based)

**Teaches**: Consensus, replication, durability, membership changes
**Stack**: Pure Python, 3+ nodes, gRPC between nodes
**Deliverable**: Pass a correctness test suite you write (linearizability checks with Jepsen-style reasoning)

### Project 10 — Event-Driven Order Processing (Saga Pattern)

**Teaches**: Distributed transactions without 2PC, outbox pattern, compensations
**Stack**: Python services + Postgres (with outbox) + Kafka/Kinesis + DynamoDB
**Deliverable**: Demonstrate recovery from every possible failure point

### Project 11 — Mini CI/CD / Deploy Platform

**Teaches**: Platform engineering, GitOps, progressive delivery
**Stack**: GitHub Actions + ArgoCD + Argo Rollouts on EKS
**Deliverable**: A developer can `git push` → canary deploy → auto-promote or rollback based on SLO

### Project 12 — Capstone: Multi-Region, Multi-Tenant SaaS Platform

**Teaches**: Everything — this is your demo for interviews
**Stack**: All of the above. Two AWS regions, active-active for reads, active-passive for writes.
**Components**: Auth, tenant isolation, API, async pipeline, observability, CI/CD, DR plan
**Deliverable**: A 30-minute walkthrough video. This goes on your resume.

---

## Phase 12 — Mock Interviews & Staff-Level Behavioral (ongoing from Month 6)

System design interviews at staff level also have a **behavioral/leadership** layer. Don't neglect it.

### Staff-level behavioral themes

1. **Scope & influence** — driving a decision that affected multiple teams
2. **Technical judgment under ambiguity** — making the call without perfect info
3. **Mentorship & multiplier effect** — making others better, not just yourself
4. **Handling disagreement** — with peers, with your manager, with execs
5. **Owning a failure** — real one, real consequences, what you changed
6. **Strategic thinking** — choosing _not_ to do something

Prepare **6–8 STAR-format stories** (Situation, Task, Action, Result) covering these themes. Reuse them across questions — good interviewers can tell which story you're pulling from but they don't care, they care about the lesson.

### Mock interview platforms

- **[Exponent](https://www.tryexponent.com/)** — structured system design mocks
- **[Pramp](https://www.pramp.com/)** — free peer mocks
- **[interviewing.io](https://interviewing.io/)** — anonymized with senior engineers
- **[HelloInterview](https://www.hellointerview.com/)** — walkthroughs by ex-FAANG
- Find an engineer friend — do weekly mocks, swap roles

### Aim for ~15 full mocks before your first real interview.

---

# Resources

## Books (non-negotiable core)

- **Designing Data-Intensive Applications** — Martin Kleppmann. _The_ book. Read every chapter twice.
- **System Design Interview Vol. 1 & 2** — Alex Xu. Structured, interview-shaped.
- **Site Reliability Engineering** + **The SRE Workbook** — Google (free). Phase 6 is built from these.
- **Observability Engineering** — Majors et al. Modern view of metrics/logs/traces.
- **Release It!** — Michael Nygard. Stability patterns, circuit breakers, bulkheads.
- **Database Internals** — Alex Petrov. When you're ready to go deep on storage.
- **Head First Design Patterns** — for LLD / Phase 9. Java but concepts translate.

## Courses

- **Grokking the System Design Interview** (Educative) — interview-shaped.
- **Grokking the Advanced System Design Interview** (Educative) — next level.
- **MIT 6.824 Distributed Systems** (free, YouTube) — for depth.
- **AWS Skill Builder** — free AWS service fluency.

## YouTube channels

- **Jordan Has No Life** — excellent modern system design breakdowns
- **Gaurav Sen** — classic interview-style
- **ByteByteGo** — visual, clean
- **Hussein Nasser** — database and networking internals
- **SREcon talks** (USENIX YouTube) — the real-world SRE content

## Papers (if you ever want to go deeper — don't force it)

- Dynamo (2007), Bigtable (2006), Spanner (2012), Kafka (2011), Raft (2014), Chubby (2006), MapReduce (2004), Borg (2015)

## Blogs to follow

- High Scalability (archive is gold)
- AWS Architecture Blog
- Uber Engineering, Netflix TechBlog, Dropbox Tech, Shopify Engineering, Stripe Engineering, Canva Engineering, Discord Engineering
- Dan Luu's blog (dan.luu.com)
- Marc Brooker's blog (brooker.co.za/blog) — AWS principal, excellent on reliability

## Practice platforms

- **HelloInterview** — guided mock problems
- **LeetCode System Design** tag
- **System Design Primer** (donnemartin/system-design-primer on GitHub) — the free starting point
- **Jepsen.io** — real-world consistency bug reports; read them like detective stories

---

# Weekly Cadence (5 hrs/week)

| Day | Time   | Activity                                                           |
| --- | ------ | ------------------------------------------------------------------ |
| Mon | 1 hr   | Read current phase (DDIA chapter / blog / doc)                     |
| Tue | 0.5 hr | One back-of-envelope problem + one LLD warm-up                     |
| Wed | 1.5 hr | Hands-on: project of the month                                     |
| Fri | 1 hr   | Continue project + commit                                          |
| Sat | 1 hr   | Whiteboard one HLD problem; write it up; post to your journal repo |

From month 6 onward, replace Saturday with a **mock interview** every other week.

---

# Self-Assessment Milestones

- **End of Phase 2**: You can pick SQL vs NoSQL with 3 supporting reasons for any new feature.
- **End of Phase 4**: You design async pipelines reflexively — retries, DLQs, idempotency are automatic.
- **End of Phase 6**: You speak SRE as a first language — SLOs, error budgets, postmortems.
- **End of Phase 8**: Security is a design constraint you raise, not something you're asked to bolt on.
- **End of Phase 10**: You can whiteboard any of the 30 classics confidently. Interviewers struggle to find gaps.
- **Post-capstone**: You have a 30-minute portfolio walkthrough that makes recruiters slide into your DMs.

---

# Universal System Design Wisdom

- **Start with requirements, not technology.** The worst interview answer begins with "I'd use Kafka."
- **Numbers beat adjectives.** "High throughput" is nothing. "50 k writes/sec, p99 < 50 ms" is a design constraint.
- **There is no right answer.** There are defensible answers. Your job is to defend, not guess.
- **Everything fails, all the time.** Design assumes failure; don't defend against it at the end.
- **Simple scales further than clever.** Monolith-first is often correct. Say so in interviews.
- **The question behind the question**: _"Would I trust this person to own a critical system at scale?"_ Every word you say should answer that.
- **Staff-level tell**: you spend as much time talking about what you'd _not_ build, monitoring, rollout, and failure recovery as you do on the happy path.

Go design. Go build. Go break it.
