# Trade-offs: What You Gain, What You Lose

Every technology choice is a trade-off. AI presents choices as recommendations. A senior architect presents choices as trade-offs and lets the context decide.

## Database Selection

### SQLite
**Choose when:** Single-server app, single write source, embedded systems, prototyping, CLI tools, mobile apps, IoT, any project where operational simplicity matters most.
**You gain:** Zero configuration, zero separate process, single-file backup (just copy the file), incredibly fast reads, no network overhead, works everywhere.
**You lose:** No concurrent writes from multiple servers. Limited to one machine's storage. No built-in replication (but Litestream solves backups). No LISTEN/NOTIFY for real-time.
**People underestimate:** SQLite in WAL mode handles hundreds of concurrent readers. It's fast enough for most web applications. It's the most tested database in existence.
**Scaling escape hatch:** When you outgrow SQLite, migrating to PostgreSQL is straightforward since both speak SQL. Do it when you need multi-server writes, not before.

### PostgreSQL
**Choose when:** Multi-server application, need concurrent writes from multiple sources, need advanced features (JSONB, full-text search, PostGIS, pub/sub, row-level security), or when your ORM/framework assumes it.
**You gain:** Battle-tested at any scale, incredible feature set, excellent tooling, huge community, extensions for everything (TimescaleDB, pgvector, PostGIS).
**You lose:** Operational overhead (backups, vacuuming, connection pooling, upgrades). Need a separate process/server. Configuration tuning for performance. Connection limits.
**The connection pooling reality:** Postgres has a hard connection limit (~100-500 depending on config). Serverless functions create a new connection per invocation. You WILL need PgBouncer or similar if using serverless. This catches people off guard.

### MySQL/MariaDB
**Choose when:** Legacy integration, WordPress/PHP ecosystem, or when your team knows it well.
**You gain:** Mature, fast for simple queries, good replication, huge hosting ecosystem.
**You lose:** Quirky SQL behavior (silent truncation, implicit type conversion), weaker JSONB support than Postgres, fewer advanced features.
**Honest take:** Unless you have a specific reason, Postgres is better for new projects. MySQL's quirks have bitten too many teams.

### MongoDB
**Choose when:** Your data is genuinely document-shaped with variable schemas, and you need horizontal scaling from day one.
**You gain:** Flexible schema, good horizontal scaling, natural fit for document-shaped data.
**You lose:** No joins (or expensive $lookup), eventual consistency by default, larger storage footprint, no transactions across collections (well, limited support now). The "flexible schema" often becomes "no schema" which becomes "nobody knows what shape the data is."
**Honest take:** 90% of MongoDB usage would be better served by PostgreSQL with JSONB columns. Use MongoDB when your data is genuinely document-shaped AND you need sharding. Not for "flexibility."

### Redis
**Choose when:** You need sub-millisecond reads for hot data, pub/sub, rate limiting, session storage, or a simple job queue.
**You gain:** Incredibly fast, versatile data structures (sorted sets, streams, HyperLogLog), pub/sub, Lua scripting.
**You lose:** Data must fit in RAM (expensive at scale), persistence is best-effort (RDB snapshots or AOF), another process to operate, data modeling is different from SQL.
**The "Redis as primary database" trap:** Don't. Redis is a cache/accelerator/broker. Your source of truth should survive a restart without thinking about it.

## API Style

### REST
**Choose when:** Public APIs, CRUD-dominant apps, when you want cacheability, or when your consumers are unknown/varied.
**You gain:** Universal understanding, HTTP caching works naturally, stateless, every framework supports it.
**You lose:** Over-fetching/under-fetching, multiple round trips for complex data needs, no standard for real-time.
**Design rule:** REST endpoints should represent RESOURCES (nouns), not ACTIONS (verbs). `POST /orders` not `POST /createOrder`. The HTTP method IS the verb.

### GraphQL
**Choose when:** Multiple frontends need different data shapes, deeply nested relational data, or you're building a developer platform.
**You gain:** Clients get exactly the data they need, strong typing, introspection, one endpoint.
**You lose:** Complexity (resolvers, N+1 queries, authorization per field, caching is harder), harder to rate-limit, learning curve, tooling overhead.
**Honest take:** If you're one team building one frontend, GraphQL adds complexity without proportional benefit. REST with well-designed endpoints is simpler. GraphQL shines when you have 3+ consumers with different data needs.

### tRPC / RPC-style
**Choose when:** TypeScript full-stack, single team owns both client and server, rapid development.
**You gain:** End-to-end type safety, no code generation, feels like calling local functions, very fast development.
**You lose:** Tied to TypeScript, not suitable for public APIs, harder to use from non-TypeScript clients.

### WebSockets / SSE
**Choose when:** Real-time data (chat, live updates, collaborative editing, monitoring dashboards).
**WebSockets:** Bidirectional. Use when the client needs to SEND real-time data too (chat, collaborative editing).
**SSE (Server-Sent Events):** Server-to-client only. Simpler. Use when you just need to PUSH updates (live feeds, notifications, progress bars). Works over HTTP, auto-reconnects, simpler to load-balance.

## Programming Language / Runtime

Don't overthink this. Use what you know. But here are honest trade-offs:

### Python
**Good for:** AI/ML, data processing, scripting, APIs (FastAPI is excellent), prototyping.
**Watch out:** GIL limits true concurrency. Async is powerful but adds complexity. Type hints are optional (which means half the team won't use them). Dependency management is a mess (use `uv` or `poetry`, not raw pip).

### TypeScript/Node
**Good for:** Full-stack web apps, real-time systems, API servers, when your frontend is also TypeScript.
**Watch out:** Single-threaded event loop. CPU-bound work blocks everything. The ecosystem moves fast (too fast). Package quality varies wildly.

### Go
**Good for:** Infrastructure tools, CLIs, high-concurrency network services, anything where deployment simplicity matters (single binary).
**Watch out:** Verbose error handling. Limited generics (improving). No exceptions. Boring by design (that's the point).

### Rust
**Good for:** Performance-critical systems, CLI tools, WebAssembly, systems programming, when correctness matters most.
**Watch out:** Steep learning curve. Slower development velocity. Compile times. The borrow checker is a feature, but it hurts at first.

## Hosting / Deployment

### Single VPS (Hetzner, DigitalOcean, Linode)
**Cost:** €4-20/month for a capable server.
**Good for:** Most projects. Seriously. A €10/month Hetzner box (4 vCPU, 8GB RAM) handles surprising load.
**Watch out:** You're responsible for security updates, backups, monitoring. Single point of failure. But for a small SaaS? Perfectly fine.

### PaaS (Railway, Fly.io, Render)
**Cost:** $5-50/month typical.
**Good for:** Zero-ops deployment. Git push and it's live. Good for MVPs and small teams.
**Watch out:** More expensive per unit of compute. Less control. Vendor lock-in varies. Cold starts on some platforms.

### Serverless (AWS Lambda, Vercel Functions)
**Cost:** Pay-per-invocation. Can be very cheap at low traffic, expensive at high traffic.
**Good for:** Irregular traffic patterns, event-driven workloads, when you truly want zero server management.
**Watch out:** Cold starts. Connection pooling issues with databases. Debugging is harder. Vendor lock-in. State management is your problem. Not all workloads fit the model.

### Kubernetes
**Cost:** Minimum viable cluster: ~$50-100/month plus your time.
**Good for:** Multiple services, multiple teams, auto-scaling, sophisticated deployment strategies.
**Watch out:** Enormous operational complexity. Weeks to learn properly. YAML everywhere. If your team doesn't have dedicated platform/infra engineers, Kubernetes will slow you down.
