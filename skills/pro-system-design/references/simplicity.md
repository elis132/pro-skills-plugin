# Simplicity: Start at the Bottom, Justify Each Step Up

## The Complexity Ladder

For every architectural component, start at the bottom and justify each step up. If you can't name a specific, current reason to move up, stay where you are.

### Data Storage

```
Level 0: In-memory / variables
Level 1: JSON/YAML files on disk
Level 2: SQLite (single file, zero config, surprisingly powerful)
Level 3: PostgreSQL (single instance)
Level 4: PostgreSQL with read replicas
Level 5: Distributed database (CockroachDB, Vitess, Citus)
Level 6: Purpose-built stores (TimescaleDB, ClickHouse, Elasticsearch)
```

**When to move up:**
- 0 → 1: Data needs to survive a restart
- 1 → 2: You need queries more complex than key lookup, or concurrent writes
- 2 → 3: You need concurrent write access from multiple processes/servers, or your data exceeds ~1TB, or you need advanced features (JSONB, full-text search, PostGIS, pub/sub)
- 3 → 4: Read queries are the bottleneck and you've already optimized indexes/queries
- 4 → 5: Single-region Postgres can't handle the write volume (this is RARE, you probably never get here)
- 5 → 6: You have a specific workload that a general database handles poorly (time-series, full-text search, analytics)

**The SQLite reality check:** SQLite handles thousands of concurrent readers, writes at ~50,000 inserts/second, supports databases up to 281TB, has full SQL support including CTEs, window functions, and JSON. It's the most deployed database in the world. For a single-server application with one write source, it's often the right answer. Litestream can replicate it to S3 for backups.

### Application Architecture

```
Level 0: Single script/file
Level 1: Monolithic application (one codebase, one deploy)
Level 2: Monolith with background workers (same codebase, separate process)
Level 3: Monolith with one or two extracted services (for genuinely different scaling needs)
Level 4: Service-oriented architecture (small number of well-defined services)
Level 5: Microservices (many small services, service mesh, distributed tracing)
```

**When to move up:**
- 0 → 1: The script is doing multiple things that change independently
- 1 → 2: You have long-running tasks that block request handling (email sending, PDF generation, data processing)
- 2 → 3: One specific part has genuinely different scaling needs (e.g., image processing needs GPUs, the rest doesn't)
- 3 → 4: Multiple teams need to deploy independently (this is an ORG problem, not a tech problem)
- 4 → 5: You have 50+ engineers and the org structure demands it

**The monolith reality check:** A well-structured monolith can serve millions of requests/day. Shopify runs on a monolith. Stack Overflow runs on a monolith (one!). Basecamp runs on a monolith. If your team is under 10 people, microservices will slow you down, not speed you up.

### Task Processing

```
Level 0: Synchronous (do it in the request)
Level 1: Cron job (scheduled task)
Level 2: In-process background thread/task
Level 3: Background worker (same codebase, separate process, database as queue)
Level 4: Simple message queue (Redis pub/sub, or PostgreSQL LISTEN/NOTIFY)
Level 5: Dedicated message broker (RabbitMQ, BullMQ)
Level 6: Event streaming (Kafka, Redpanda)
```

**When to move up:**
- 0 → 1: Task needs to run on a schedule, not triggered by a request
- 1 → 2: Task takes >100ms and you don't want to block the response
- 2 → 3: Task might fail and needs retry logic, or outlives the request process
- 3 → 4: Multiple consumers need the same events, or you need pub/sub semantics
- 4 → 5: You need guaranteed delivery, complex routing, dead letter queues, and your volume exceeds what Postgres NOTIFY handles
- 5 → 6: You need event replay, stream processing, or you're handling >100k events/second

**The "database as queue" reality check:** Using your existing PostgreSQL as a job queue (with a `jobs` table and polling, or `SKIP LOCKED`) works fine up to thousands of jobs/minute. Libraries like `pgboss` (Node), `good_job` (Ruby), and `procrastinate` (Python) make this trivial. You don't need RabbitMQ until you do.

### Caching

```
Level 0: No cache (just query the database)
Level 1: Application-level memoization (in-memory, per-process)
Level 2: HTTP caching headers (browser/CDN cache)
Level 3: CDN for static assets
Level 4: Read-through cache at the application level
Level 5: Dedicated cache (Redis/Memcached)
Level 6: Multi-layer caching strategy
```

**When to move up:**
- 0 → 1: Same expensive computation runs multiple times per request/session
- 1 → 2: Responses don't change often and can be cached by the browser or a CDN
- 2 → 3: Static assets (images, CSS, JS) are served from your application server
- 3 → 4: Database queries are the bottleneck and you've already optimized them
- 4 → 5: Cache needs to be shared across multiple application instances, or cache data exceeds available RAM on application servers
- 5 → 6: You have different TTLs for different data types and complex invalidation needs

**The "no cache" reality check:** A well-indexed PostgreSQL query returns in <5ms. If your page loads in 200ms without cache, adding Redis to make it 180ms adds complexity for marginal gain. Cache when you have an ACTUAL performance problem, not a theoretical one. Measure first.

### Deployment

```
Level 0: Run locally / ssh and restart
Level 1: Single VPS with systemd service
Level 2: Single VPS with reverse proxy (Caddy/Nginx) + auto-TLS
Level 3: PaaS (Railway, Fly.io, Render, Heroku)
Level 4: Cloud VMs with load balancer (2-3 instances)
Level 5: Container orchestration (Docker Compose on a single host)
Level 6: Kubernetes
```

**When to move up:**
- 0 → 1: Other people need to use the application
- 1 → 2: You need HTTPS and/or multiple apps on the same server
- 2 → 3: You want zero-ops deployment (git push to deploy). Trade-off: costs more per unit of compute
- 3 → 4: Single instance can't handle the traffic, or you need high availability
- 4 → 5: You have multiple services that need to run together with defined networking
- 5 → 6: You have 10+ services, multiple teams, and need auto-scaling, rolling deploys, service discovery. If you're asking whether you need Kubernetes, you don't.

## The Golden Rule

**If a technology doesn't solve a problem you CURRENTLY HAVE, it creates a problem you CURRENTLY DON'T HAVE.**

Redis is not free. It's another process to monitor, another connection to manage, another thing that can run out of memory. Kubernetes is not free. It's weeks of learning, YAML files to maintain, networking to debug. Microservices are not free. They're distributed systems problems (network partitions, eventual consistency, distributed tracing) that monoliths simply don't have.

Every addition to the stack must pay for itself with a specific, current problem it solves.
